- find_package(<PackageName> REQUIRED)
cmake가 현재 설치된 라이브러리를 찾아서 추가해 준다.
찾는 기준은 다음과 같다.
cmake -DCMAKE_PREFIX_PATH=/opt/boost .. 로 받은 경로
<PackageName>_DIR, <PackageName>_ROOT 변수에 설정된 경로
CMake 내장 경로 => /usr/local, /usr, /lib, /lib64, /opt, C:\Program Files, C:\Program Files (x86)
/usr/share/cmake/Modules/Find<PackageName>.cmake 파일을 찾아 실행 -> 이 파일 내부에서 라이브러리를 찾을 추가 경로 설정
패키지 구성 파일 -> 일부 라이브러리는 cmake 구성 파일을 제공함. cmake는 이 파일을 찾아 실행함.   

- CMAKE_PREFIX
find_package
