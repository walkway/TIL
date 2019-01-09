# javascript debug

### debugging
- 디버그(debug), 디버깅(debugging)은 컴퓨터 프로그램의 정확성이나 논리적인 오류(버그)를 검출하여 제거하는 과정을 뜻한다.
- Debugging tactics can involve interactive debugging, control flow analysis, unit testing, integration testing, log file analysis, monitoring at the application or system level, memory dumps, and profiling.

### chrome dev tools
- Elements: dom 확인, css 수정
- Console: log, script command
- Sources: break point
  - break point 코드 일시 중지 -> 단계별 코드 실행 -> 버그 확인 -> 내용 수정 -> 적용

- break point type
````
Line-of-code: 정확한 코드 영역
Conditional line-of-code: 정확한 코드 영역 + 조건	
DOM: 특정 DOM 노드 또는 그 자식을 변경하거나 제거하는 코드
XHR: XHR URL에 문자열 패턴이 포함 된 경우
Event listener:	이벤트 발생 후 실행되는 코드
Exception: catch 또는 catch되지 않은 예외를 throw하는 코드
Function: 특정 함수가 호출
````

- example: click 이벤트에 break point 활성화
````
Sources > Event Listener Breakpoints > Mouse > click
````
- example: 변수 타입 확인
````
Sources > Watch
typeof sum
````

##### 참고
- https://www.youtube.com/watch?v=fJxFZO8OEEs&list=PLNYkxOF6rcIBDSojZWBv4QJNoT4GNYzQD
- https://developers.google.com/web/tools/chrome-devtools/javascript/?hl=ko