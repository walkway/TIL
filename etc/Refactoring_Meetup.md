# Refactoring Meetup

### Mob Programming
- Pair: 한 대의 컴퓨터로 작업. 키보드를 잡은 사람: navigator, 보조: navigator
- Mob: 한 명의 드라이버, 여러 명의 프로그래머가 한 대의 컴퓨터로 작업. 1:n 확장
- manager, navigator, driver
- 돌아가면서, driver <-> navigator
- manager가 원하는 코드를 만든다.
- 미팅 잡고, 시간을 쓰는 것이 더 아깝다.
- 커뮤니케이션 비용 최소화 -> 작업 효율을 극대화

### Cognitive Complexity
- Cyclomatic Complexity
 - 같은 복잡도지만, 어떤 코드가 가독성이 좋을까.
 - 사람이 더 인식하기 좋은 지표.
 - SonarQube: 신호등 표현, 코드 품질을 평가 sonarcloud.io/

### TDD
- 테스트 코드 작성 -> 테스트 코드를 통과할 수 있는 코드를 작성.
- 모든 팀원이 이해하기 쉽도록 작성, 의존성이 적은 코드 작성.
- web 기반 코딩, 실시간 코딩: cyber-dojo.org
- given, when, then
- 중복제거
- magic number
- inline
- long method: 불완전한 중복, 복잡한 메소드 분리

### Empathy & Sympathy
- https://www.youtube.com/watch?v=W9YSNpkPJI4

- Refactoring Meetup
- http://www.moreagile.net/2014/12/mob-programming.html