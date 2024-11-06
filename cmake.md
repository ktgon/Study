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
- Literal variable reference 는 alphanumeric 문자와 /_.+-, 와 Escape Sequences 로 구성된다.  
*리터럴 변수 참조는 변수의 값을 사용하지 않고 변수 그대로를 문자열로 사용하는 것을 말한다.*









