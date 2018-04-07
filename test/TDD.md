# TDD
- 테스트 주도 개발(Test-driven development)
- 짧은 개발 사이클을 반복하는 소프트웨어 개발 프로세스 중 하나
- 1. arrange 2. act 3. asset
- red-green
- assert 범위가 SRP를 만족하는지 체크
- act는 한가지 역할만 하는지 체크
- 어떤 레벨로 테스트 할것인지 고민
- 처음 생각한대로 코드를 작성하고, 리팩토링을 반복
- @before : 초기화
- @after : 끝
````
assertThat(T actual, Matcher<? super T> matcher)
````
- BDD는 Behavior-Driven Development