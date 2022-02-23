# global time zone
- 모든 날짜/시간 값을 UTC 형식으로 저장하고, 필요한 경우 UI에서만 표준 시간대 변환을 처리
- MySQL에 날짜를 저장할 때, 데이터베이스의 날짜가 시스템이나 JVM과 다를 때, 일관되게 설정하는 것이 필요

## Timezone as a URL Param
- MySQL 기본적으로 useLegacyDatetimeCode=true를 사용, 시간대를 선택하려면 이 속성을 false로 설정
- spring boot > application.properties
````
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/test?serverTimezone=UTC&useLegacyDatetimeCode=false
    username: root
    password:
````

## Spring Boot Property
````
spring:
  jpa:
    properties:
      hibernate:  
        jdbc:
          time_zone:UTC
````

## JVM Default Timezone
- Java의 기본 시간대를 업데이트하는 방법, 이 방법은 애플리케이션 전체에 적용되기 때문에 다른 문제를 일으킬 수도 있음
- 예를 들어 다른 데이터베이스에 연결해야 할 수 있고, 어떤 이유로 인해 날짜를 다른 시간대에 저장해야 할 수도 있고
````
@PostConstruct
void started() {
  TimeZone.setDefault(TimeZone.getTimeZone("UTC"));
}
````

https://vladmihalcea.com/how-to-store-date-time-and-timestamps-in-utc-time-zone-with-jdbc-and-hibernate/  
https://vladmihalcea.com/a-beginners-guide-to-java-time-zone-handling/

# formatter
````
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss").withZone(ZoneId.systemDefault());
````
````
DateTimeFormatter formatter = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.SHORT).withZone(ZoneId.systemDefault());
````
````
Instant instant = Instant.now();
String time = formatter.format(instant);
````