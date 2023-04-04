# maven wrapper(mvnw)

- 별도의 Maven 설치 없이도 Maven을 이용
- Maven 버전에 대한 관리도 해주며 별도로 명시하지 않으면 최신 버전 사용
- mvnw : Maven을 설치하지 않아도 Maven을 실행시킬 수 있도록 해주는 유닉스 셸 스크립트
- mvnw.cmd : 위 스크립트의 배치 버전
- mvnw = maven wrapper의 약자

````
// 특정 버전 명시
mvn -N io.takari:maven:wrapper -Dmaven=3.3.3
````

// 사용
````
./mvnw clean package
````
https://www.baeldung.com/maven-wrapper
