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