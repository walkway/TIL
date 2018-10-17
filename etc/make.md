# make

- Makefile이 있는 디렉토리에서 make 명령어 실행 -> 컴파일
- 파일 관리 유틸리티
- 명령어 자동화
- 프로그램 종속 구조 파악

### Makefile
- 목적파일(Target) : 명령어가 수행되어 나온 결과를 저장할 파일 
- 의존성(Dependency) : 목적파일을 만들기 위해 필요한 재료 
- 명령어(Command) : 실행 되어야 할 명령어들 
- 매크로(macro) : 코드를 단순화 시키기 위한 방법
````
<Target>: <Depend> ?...[[;] <Command>]
<Tab><Command>
````