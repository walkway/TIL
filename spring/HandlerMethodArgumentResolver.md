# HandlerMethodArgumentResolver

- Strategy interface for resolving method parameters into argument values in the context of a given request.
- 주어진 요청을 처리할 때, 메소드 파라미터를 인자 값들에 주입, Controller 에 들어오는 파라미터 가공 or 설정

### Anotation 정의
````
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
public @interface ValidatedMember {

}
````

### HandlerMethodArgumentResolver 정의
````
@Component
public class CustomMethodArgumentResolver implements HandlerMethodArgumentResolver {

    @Override
    public boolean supportsParameter(MethodParameter parameter) {
        // 구현
    }

    @Override
    public Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer, NativeWebRequest webRequest, WebDataBinderFactory binderFactory) throws Exception {
     // 구현
    }

}
````

### ArgumentResolver 설정
````
@Configuration
public class WebConfiguration implements WebMvcConfigurer {

    @Autowired
    private CustomMethodArgumentResolver customMethodArgumentsHandler;

    @Override
    public void addArgumentResolvers(List<HandlerMethodArgumentResolver> resolvers) {
        resolvers.add(customMethodArgumentsHandler);
    }
}
````

### Controller
````
@GetMapping("/test/{id}")
public void requestSignUp(@ValidatedMember Member member) {
    ... 생략
}
````