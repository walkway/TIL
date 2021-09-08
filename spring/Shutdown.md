# Shutdown Spring Boot Application

## Shutdown Endpoint
````
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
````

application.properties
````
management.endpoints.web.exposure.include=*
management.endpoint.shutdown.enabled=true
endpoints.shutdown.enabled=true
````
API call
````
curl -X POST localhost:port/actuator/shutdown
````

## Close Application Context
````
ConfigurableApplicationContext ctx = new 
  SpringApplicationBuilder(Application.class).web(WebApplicationType.NONE).run();
System.out.println("Spring Boot application started");
ctx.getBean(TerminateBean.class);
ctx.close();
````
````
public class TerminateBean {

    @PreDestroy
    public void onDestroy() throws Exception {
        System.out.println("Spring Container is destroyed!");
    }
}
````
````
@Configuration
public class ShutdownConfig {

    @Bean
    public TerminateBean getTerminateBean() {
        return new TerminateBean();
    }
}
````

## Close the Current Application Context
````
@RestController
public class ShutdownController implements ApplicationContextAware {
    
    private ApplicationContext context;
    
    @PostMapping("/shutdownContext")
    public void shutdownContext() {
        ((ConfigurableApplicationContext) context).close();
    }

    @Override
    public void setApplicationContext(ApplicationContext ctx) throws BeansException {
        this.context = ctx;
        
    }
}
````
````
curl -X POST localhost:port/shutdownContext
````
https://www.baeldung.com/spring-boot-shutdown