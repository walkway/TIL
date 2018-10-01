# mybatis - spring boot

````
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.3.2</version>
</dependency>
````
- DataSource 감지
- SqlSeessionFactoryBean을 사용해서 데이터 소스를 전달하는 SqlSessionFactory의 객체를 생성, 등록
- SqlSessionFactory을 이용해서 SqlSessionTemplate의 객체를 생성, 등록
- 매퍼 자동 스캔
- 스캔된 것을 SqlSessionTemplate에 연결
- Spring Context에 등록, bean 주입

### sample 1
````
drop table if exists city;
drop table if exists hotel;

create table city (id int primary key auto_increment, name varchar, state varchar, country varchar);
create table hotel (city int, name varchar, address varchar, zip varchar);

insert into city (name, state, country) values ('San Francisco', 'CA', 'US');
insert into hotel(city, name, address, zip) values (1, 'Conrad Treasury Place', 'William & George Streets', '4001')
````
````
@Data
public class City implements Serializable {
	private static final long serialVersionUID = 1L;
	private Long id;
	private String name;
	private String state;
	private String country;
}
````
````
@Mapper
public interface CityMapper {
    @Select("SELECT * FROM CITY WHERE state = #{state}")
    City findByState(@Param("state") String state);
}
````
````
@SpringBootApplication
public class SampleMybatisApplication implements CommandLineRunner {

    private final CityMapper cityMapper;

    public SampleMybatisApplication(CityMapper cityMapper) {
        this.cityMapper = cityMapper;
    }

    public static void main(String[] args) {
        SpringApplication.run(SampleMybatisApplication.class, args);
    }

    @Override
    public void run(String... args) throws Exception {
        System.out.println(this.cityMapper.findByState("CA"));
    }
}
````
- http://www.mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/