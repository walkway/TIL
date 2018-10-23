# Login Request Authentication Test

````
@RunWith(SpringJUnit4ClassRunner.class) 
@ContextConfiguration 
@WebAppConfiguration 
public class CustomLoginRequestBuilderAuthenticationTests { 
 
    @Autowired 
    private WebApplicationContext context; 
 
    @Autowired 
    private Filter springSecurityFilterChain; 
 
    private MockMvc mvc; 
 
    @Before 
    public void setup() { 
        mvc = MockMvcBuilders 
                .webAppContextSetup(context) 
                .addFilters(springSecurityFilterChain) 
                .build(); 
    } 
 
    @Test 
    public void authenticationSuccess() throws Exception { 
        mvc 
            .perform(login()) 
            .andExpect(status().isMovedTemporarily()) 
            .andExpect(redirectedUrl("/")) 
            .andExpect(authenticated().withUsername("user")); 
    } 
 
    @Test 
    public void authenticationFailed() throws Exception { 
        mvc 
            .perform(login().user("notfound").password("invalid")) 
            .andExpect(status().isMovedTemporarily()) 
            .andExpect(redirectedUrl("/authenticate?error")) 
            .andExpect(unauthenticated()); 
    } 
 
    static FormLoginRequestBuilder login() { 
        return SecurityMockMvcRequestBuilders 
                .formLogin("/authenticate") 
                    .userParameter("user") 
                    .passwordParam("pass"); 
    } 
 
    @Configuration 
    @EnableWebMvcSecurity 
    @EnableWebMvc 
    static class Config extends WebSecurityConfigurerAdapter { 
 
        @Override 
        protected void configure(HttpSecurity http) throws Exception { 
            http 
                .authorizeRequests() 
                    .anyRequest().authenticated() 
                    .and() 
                .formLogin() 
                    .usernameParameter("user") 
                    .passwordParameter("pass") 
                    .loginPage("/authenticate"); 
        } 
 
        @Autowired 
        public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception { 
            auth 
                .inMemoryAuthentication() 
                    .withUser("user").password("password").roles("USER"); 
        } 
    } 
}
````
https://www.programcreek.com/java-api-examples/index.php?source_dir=spring-security-test-blog-master/src/test/java/org/springframework/security/test/web/servlet/showcase/login/CustomLoginRequestBuilderAuthenticationTests.java
 