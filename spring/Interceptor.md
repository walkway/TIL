# Interceptor
- 특정 URI로 요청시 Controller로 가는 요청을 가로채는 역할
- HandlerInterceptor
- Handler를 실행하기전(preHandle), Handler를 실행한 후(postHandle), view를 렌더링한 후(afterCompletion)
- ex) log 처리
````
public class TestInterceptor extends HandlerInterceptorAdapter {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
		log.info("Interceptor > preHandle");
		return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        log.info("Interceptor > postHandle");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object object, Exception arg3) throws Exception {
        log.info("Interceptor > afterCompletion" );
    }
}
````
````
@Configuration
public class WebMvcConfig extends WebMvcConfigurerAdapter {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new TestInterceptor())
                .addPathPatterns("/*")
                .excludePathPatterns("/**")
                .excludePathPatterns("/user/**");
    }
}
````

### Filter와 차이
- Filter: Filter는 DispatcherServlet 실행 전 호출, Web Application 등록
- Interceptor: DispatcherServlet이 실행된 후 호출, Spring의 Context 등록