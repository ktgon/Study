## CMAKE_VERSION

Release version
<major>.<minor>.<patch>[-rc<n>] 
<path> component is less than 20,000,000

Development version
<major>.<minor>.<date>[-<id>]
<date> component's format CCYYMMDD 
<id> arbitary text

Individual component values are also available in variables:
CMAKE_MAJOR_VERSION
CMAKE_MINOR_VERSION
CMAKE_PATCH_VERSION
CMAKE_TWEAK_VERSION

Use 'if()' command 
operators VERSION_LESS, VERSION_GREATER, VERSION_EQUAL, VERSION_LESS_EQUAL, VERSION_GREATER_EQUAL
CMAKE_VERSION

CMP0054 
Only interpret if() arguments as variables or keywords when unquoted.

## cmake-language(7)

### Escape Sequences 
- Escape Sequence는 \ 다음에 하나의 문자가 나온다.  
\ 다음에 non-alphanumeric 문자가 오면 구문으로 해석하지 않고 리터럴 문자로 엔코딩한다.
- \t, \r, \n 은 tab, carriage return, newline 문자를 엔코딩한다.
- Variable References 외부에 있는 \; 는 자체적으로 엔코딩하지만  
Unquoted Argument에서 argument value를 분할하지 않고 ;를 엔코딩하는데 사용된다. 
- Variable References 내부에 있는 \; 는 리터럴 ; 문자를 엔코딩한다. 

### Variable References  
- Variable reference 는 ${<variable>} 형식을 갖고 있으며
Quoted Argument 나 Unquoted Argument 내에서 evaluation된다.
- Variable reference 는 특정 variable 이나 cache entry 의 value로 대체된다.  
또는 둘 다 설정되어 있지 않으면 empty string으로 대체된다.
- Variable reference는 중첩될 수 있고 내부에서 외부 순서로 evaluation 될 수 있다.  
e.g. ${outer_${inner_variable}_variable}
- Literal variable reference 는 alphanumeric 문자와 /_.+-, 와 Escape Sequences 로 구성된다.  
*리터럴 변수 참조는 변수의 값을 사용하지 않고 변수 그대로를 문자열로 사용하는 것을 말한다.*
- 중첩된(Nested) references는 어떤 이름의 변수에 대한 evaluation에도 사용될 수 있다.
- Variables section 에 variable name과 어떻게 value가 설정되는지에 대하여 문서화되어 있다.
- Environment variable reference는 $ENV{<variable>} 형식을 가진다.
- Cache variable reference는 $CACHE{<variable>} 형식을 가지며
설정된 cache entry의 값으로 대체된다. (이 때 동일 이름의 일반 변수에 대한 체크를 하지 않는다.)
cache entry에 없다면 empty stirng으로 대체된다.
- if() command는 ${variable>} 대신 <variable> 단축폼으로 variable references를   
허용하는 특별한 조건 구문을 가진다.  
그러나 environment variables는 항상 $ENV{<variable>}와 같이 참조되어야 한다.

### Comments  
- comment는 # 문자로 시작한다.  
# 문자는 Bracket Argument, Quoted Argument 안에 있지 않거나  
Unquoted Argument의 일부로 \로 escape 되지 않아야 한다. 
- Bracket Commnet와 Line Comment 가 있다.

#### Bracket Comment  
- bracket_open이 바로 뒤따르는 #은 bracket으로 둘러쌓인 전체로 구성된 bracket commnet를 형성한다.
- Example
```
#[[This is a bracket comment.
It runs until the close bracket.]]
message("First Argument\n" #[[Barcket Comment]] "SecondArgument")
```
- 3.0 이전 버전은 bracket comments를 지원하지 않는다.  
openning #을 Line Comment로 해석한다.

#### Line Comment 
- bracket_open이 바로 연결되지 않은 # 은 라인의 끝까지 적용되는 line comment를 형성한다.
- Example
``` 
# This is a line comment.
message("First Argument\n" # This is a line comment :)
        "Second Argument") # This is a line comment.
```

### Control Structures 

#### Conditional Blocks  
- if()/elseif()/else()/endif() commands 는 조건에 따라 실행될 코드 블록을 구분짓는다.

#### Loops  
- foreach()/endforeach(), while()/endwhile() commands는 loop안에서 실행될 코드 블록을 구분짓는다.
- blocks 안에서 break() command 는 loop 를 끝내는데 사용된다.  
반면 continue() command는 다음 iteration을 즉시 시작하는데 사용된다.

#### Command Definitions  
- macro()/endmacro(), function()/endfunction() commands는 이후 command로 호출하기 위해  
기록될 code block을 구분짓는다.

### Variables  
- 비록 일부 command는 이 값을 다른 타입으로 해석하지만 value는 항상 string 타입이다.
- set(), unset() command는 변수를 명시적으로 set, unset 한다.  
다른 commands도 변수를 변경하는 semantics를 가진다.
- 변수명은 case-sensitive하며 거의 모든 텍스트로 구성된다.  
하지만 alphanumeric 문자, _, - 를 권장한다.
- 변수는 dynamic scope를 가진다.  
각 변수 "set" "unset"은 현재 scope에서의 바인딩을 생성한다.  
Block Scope  
block() command는 variable binding에 대한 새로운 scope를 생성한다.  

Function Scope  
function() command으로 생성된 Command Definitions은 commands를 생성하며
호출되면 새로운 variable binding scope에서 저장된 commands를 진행한다.  
Variable "set" or "unset"은 이 scope에서 bind하며 
현재의 function과 그 안에서 중첩 호출에 대하여 visible하지만 
함수가 리턴된 후에는 그렇지 않다. 

Directory Scope  
source tree안의 각 Directory는 각각의 고유 variable bindings를 가진다.  
어느 directory에 대하여 CMakeLists.txt 파일을 진행하기 전에 
CMake는 parent directory에 현재 정의된 모든 variable bindings를 (무엇이든)
새로운 directory scope를 초기화하기 위해 복사한다.  
cmake -P로 진행될 때 CMake Scripts는 하나의 "directory" scope에서 bind 한다.  
function call 안에 있지 않은 변수 "set" or "unset"은 현재 directory scope에 바인드한다. 

Persistent Cache  
CMake는 별도의 "cache" variables 또는 "cache entries" set 을 저장한다.  
이 값들은 project build tree 안에서 multiple runs를 해도 유지된다.  
Cache entries는 고립된 명시적 요청(set(), unset() command의 CACHE option과 같은)에 
의해서만 수정되는 binding scope를 가진다.

- Variable References를 evaluation할 때 CMake는 
만일 있다면 function call stack에서 binding을 찾고 없다면 현재 directory scope에서 찾는다. 
"set" binding이 발견되면 그 값을 사용한다.  
"unset" binding이 발견되거나 binding이 없으면 cache entry를 검색한다.  
만약 cache entry가 발견되면 그 값을 사용한다. 
그 외의 경우에는 empty string으로 evaluation된다.  
$CACHE{VAR} 문법은 direct cache entry 검색시 사용된다.  
- Note  
다음 identifier는 예약되어 있다. 
begin with CMAKE_ (upper-, lower- or mixed-case),  
beign with _CMAKE_ (upper-, lower- or mixed-case),  
begin with _ followed by the name of any CMake Command

### Environment Variables  
일반 변수와 같으며 다음과 같은 차이가 있다.  
Scope 
Global scope를 가진다. 
절대로 cached되지 않는다.  

References  
Variable References 는 $ENV{<variable>} 형식을 가진다. (ENV operator 사용)  

Initialization  
CMake environment variables의 initial values 는 calling process이다.  
values는 set(), unset() command를 사용하여 변경할 수 있다.  
이 commands는 동작중인 CMake process에만 영향을 미친다. (not system environment at large)  
변경된 값은 호출한 프로세스에 다시 쓰여지지 않으며 후속 빌드나 테스트 프로세스에 보여지지 않는다.  

cmake -E env command-line tool은 변경된 environment에서 command를 실행시킨다.  

Inspection  
cmake -E environment command-line tool은 모든 current environment variables를 표시한다.  

cmake-env-variables(7) 메뉴얼은 CMake에서 특별한 의미를 가지는 environment variables를 정리했다. 

### Lists  
- CMake의 모든 값은 string으로 저장되더라도 특정 문맥에서 list로 처리된다. (Unquoted Argument에 대한 evaluation 시)
- string은 ; 문자를 기준으로 list elements로 분할된다. 
(수가 맞지 않는 [와 ] 뒤에 오거나 바로 앞에 \가 오지 않는 경우)  
\;는 값을 분리하지 않고 결과 엘리먼트에 ;로 대체된다.
- list는 ;를 구분자로 element를 합친 string으로 표시된다.  
- Example set() command 가 multiple values를 destination variabe에 list로 저장
```
set(srcs a.c b.c c.c) # sets "srcs" to "a.c;b.c;c.c" 
```
- Lists는 소스파일 목록과 같이 간단한 사용목적으로 설계되었고 복잡한 데이터 처리 작업에 사용해서는 안된다.
- list를 만드는 대부분의 command는 list elements에 ;문자를 escape하지 않으므로 
중첩된 lists를 평평하게 만든다. 
```
set(x a "b;c") # sets "x" to "a;b;c", not "a;b\;c"
```
- 일반적으로 list는 ;문자를 포함한 element를 지원하지 않는다.  
문제를 회피하기 위해 다음 조언을 고려하라.  

- 많은 CMake commands, variables, properties의 interface는 semicolon-separated list를 받아들인다.  
직접적인 지원이나 semicolon을 encoding하거나 escaping하는 방법을 문서화하지 않았다면 
semicolons을 포함한 element가 있는 list를 넘기는 것을 회피하라. 

- list를 만들 때 elements 내에 ;를 사용할 수 없다면 사용되지 않는 자리표시자로 대체하라. 
예를 들어 다음 코드는 ; 문자 대신 |를 사용한다.  
```
set(mylist a "b|c")
foreach(entry IN LISTS mylist)
  string(REPLACE "|" ";" entry "${entry}")
  # use "${entry}" normally
endforeach()
```
- ExternalProject module의 LIST_SEPARATOR option은 이러한 접근을 사용한 인터페이스 빌드의 예이다. 
- generator expressions의 list에서 $<SEMICOLON> generator expression을 사용하라 
- Command call에서 가능하면 Quoted Argument 구문을 사용하라.  
called command는 semicolon이 보존된 argument의 content를 받게 될 것이다.  
Unquoted Argument는 semicolons으로 split될 것이다.
- function() 실행부에서는 ARGV, ARGN을 회피하라. 
이들은 구분되는 값 안에 있는 semicolons와 값을 구분하는 semicolons을 구분하지 않는다.  
대신 named positional arguments와 ARGC, ARGV# 변수를 사용하라.  
cmake_parse_arguments()를 arguments를 parse하기 위해 사용할 때는 
ARGV# 변수를 사용하는 PARSE_ARGV signature를 우선시하라.
- 이 접근은 macro() 수행부에는 적용되지 않는다. 
왜냐하면 arguments에 대하여 real variable이 아니라 placeholder를 사용하여 reference하기 때문이다. 







































































