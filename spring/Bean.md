# @Bean
- @ComponentScan: Bean 찾기
- @Autowired 생성자 주입
- @Autowired 어노테이션으로 주입할 객체를 생성
- Bean 객체에 이름을 설정: name
- @Bean(name="test", autowire = Autowire.BY_NAME)
- name 없는 경우, default name = 메소드명
- @Configuration 안에 @Bean
- @Component, @Service, @Repository, @Controller: 자동으로 스프링빈 등록
- @SpringBootApplication: @Configuration, @EnableAutoConfiguration, @ComponentScan 을 사용하는 것
- @Controller: Presentation Layer에서 Contoller를 명시
- @Service: Business Layer에서 Service를 명시
- @Repository: Persistence Layer에서 DAO를 명시
- @Component: 그 외에 자동으로 스캔해서 등록하고 싶은 것들을 위해 사용

### @Bean과 @Component 차이
- @Bean: 컨트롤이 불가능한 외부 라이브러리들을 Bean으로 등록하고 싶은 경우
- @Component: 컨트롤 가능한 경우
