# Open Feign

- writing java http clients easier
````
interface GitHub {
  @RequestLine("GET /repos/{owner}/{repo}/contributors")
  List<Contributor> contributors(@Param("owner") String owner, @Param("repo") String repo);
}

static class Contributor {
  String login;
  int contributions;
}

public static void main(String... args) {
  GitHub github = Feign.builder()
                       .decoder(new GsonDecoder())
                       .target(GitHub.class, "https://api.github.com");

  // Fetch and print a list of the contributors to this library.
  List<Contributor> contributors = github.contributors("OpenFeign", "feign");
  for (Contributor contributor : contributors) {
    System.out.println(contributor.login + " (" + contributor.contributions + ")");
  }
}
````
- Jackson: json api
````
GitHub github = Feign.builder()
                     .encoder(new JacksonEncoder())
                     .decoder(new JacksonDecoder())
                     .target(GitHub.class, "https://api.github.com");
````
- Response, String, byte[], void -> non-default decoder

### Custom error
````
Feign.builder().errorDecoder(new StashErrorDecoder())
		.target(TestApi.class, url);
````
````
public class TestErrorDecoder implements ErrorDecoder {

    @Override
    public Exception decode(String methodKey, Response response) {
        if (response.status() >= 400 && response.status() <= 499) {
            return new TestClientException(
                    response.status(),
                    response.reason()
            );
        }
        if (response.status() >= 500 && response.status() <= 599) {
            return new TestClientException(
                    response.status(),
                    response.reason()
            );
        }
        return errorStatus(methodKey, response);
    }
}
````

### Header
- configuration
````
public class HeaderConfiguration {

    @Bean
    public RequestInterceptor requestInterceptor() {
        return requestTemplate -> requestTemplate.header("header key", "value");
    }
}
````
- annotation
````
@GetMapping(value = "/status/", headers = "key=value")
void status(@PathVariable("status") int status);
````
````
@GetMapping(value = "/status/")
void status(@RequestHeader("key") String headers, @PathVariable("status") int status);
````

- https://github.com/OpenFeign/feign
- https://github.com/OpenFeign/feign/wiki/Custom-error-handling

### timeout
application.yml
````
feign:
  client:
    config:
      default:
        connectTimeout: 60000
        readTimeout: 10000
````
````
feign:
  client:
    config:
      FooClient:
        connectTimeout: 10000
        readTimeout: 20000
````
- configuration default
````
feign.httpclient.connection-timeout: 2000
feign.httpclient.ok-http.read-timeout: 60s
````

### @SpringQueryMap
annotate a POJO or Map parameter
````
// Params.java
public class Params {
    private String param1;
    private String param2;

    // [Getters and setters omitted for brevity]
}

@FeignClient("demo")
public interface DemoTemplate {

    @GetMapping(path = "/demo")
    String demoEndpoint(@SpringQueryMap Params params);
}
````

-  https://docs.spring.io/spring-cloud-openfeign/docs/current/reference/html/appendix.html
