# @DataJpaTest

````
@Target(value=TYPE)
@Retention(value=RUNTIME)
@Documented
@Inherited @BootstrapWith(value=org.springframework.boot.test.autoconfigure.orm.jpa.DataJpaTestContextBootstrapper.class)
@ExtendWith(value=org.springframework.test.context.junit.jupiter.SpringExtension.class)
@OverrideAutoConfiguration(enabled=false)
@TypeExcludeFilters(value=DataJpaTypeExcludeFilter.class)
@Transactional
@AutoConfigureCache
@AutoConfigureDataJpa
@AutoConfigureTestDatabase
@AutoConfigureTestEntityManager
@ImportAutoConfiguration
public @interface DataJpaTest
````
- 내장형 데이터베이스를 사용하여 실제 데이터베이스를 사용하지 않고 테스트 데이터베이스 사용
- 메모리상에 내부 데이터베이스를 생성하고 @Entity 클래스들을 등록하고 JPA Repository 설정
- 각 테스트마다 테스트가 완료되면 관련한 설정 롤백
- @AutoConfigureTestDatabase: replace=AutoConfigureTestDatabase.Replace 디폴트로 설정되어, 설정해놓은 DB가 아닌 in-memory DB를 활용해서 테스트가 실행
EmbeddedDatabaseConnection 클래스를 보면 H2, DERBY, HSQL, HSQLDB 중 사용 가능한 in-memory DB에 자동으로 컨넥션을 설정
replace=AutoConfigureTestDatabase.NONE으로 값을 덮어 씌우면 설정해놓은 DB를 테스트에 사용
````
- Data JPA tests may also inject a TestEntityManager bean which provides an alternative to the standard JPA EntityManager specifically designed for tests. If you want to use TestEntityManager outside of @DataJpaTests you can also use the @AutoConfigureTestEntityManager annotation. A JdbcTemplate is also available if you need that.
````
import org.junit.*;
import org.junit.runner.*;
import org.springframework.boot.test.autoconfigure.orm.jpa.*;

import static org.assertj.core.api.Assertions.*;

@RunWith(SpringRunner.class)
@DataJpaTest
public class ExampleRepositoryTests {

    @Autowired
    private TestEntityManager entityManager;

    @Autowired
    private UserRepository repository;

    @Test
    public void testExample() throws Exception {
        this.entityManager.persist(new User("sboot", "1234"));
        User user = this.repository.findByUsername("sboot");
        assertThat(user.getUsername()).isEqualTo("sboot");
        assertThat(user.getVin()).isEqualTo("1234");
    }

}
````

# @TestPropertySource
- @TestPropertySource 어노테이션을 통해 다른 설정보다 더 높은 우선순위를 가지는 설정 sourace를 정의
````
@RunWith(SpringRunner.class)
@SpringBootTest(
  SpringBootTest.WebEnvironment.MOCK,
  classes = Application.class)
@AutoConfigureMockMvc
@TestPropertySource(
  locations = "classpath:application-integrationtest.properties")
public class EmployeeRestControllerIntegrationTest {

    @Autowired
    private MockMvc mvc;

    @Autowired
    private EmployeeRepository repository;

    // write test cases here
}
````
````
spring.datasource.url = jdbc:h2:mem:test
spring.jpa.properties.hibernate.dialect = org.hibernate.dialect.H2Dialect
````
