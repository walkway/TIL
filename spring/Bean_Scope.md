# Bean Scope
````
- singleton – Return a single bean instance per Spring IoC container
- prototype – Return a new bean instance each time when requested
- request – Return a single bean instance per HTTP request. *
- session – Return a single bean instance per HTTP session. *
- globalSession – Return a single bean instance per global HTTP session. *
````
- Spring에서 기본적으로 bean은 singleton으로 생성
````
public class TestBean {
	String message;
	
	public String getMessage() {
		return message;
	}

	public void setMessage(String message) {
		this.message = message;
	}
}
````
````
//singleton A == B
//prototype A != B
TestBean A = (TestBean)context.getBean("testBean");
TestBean B = (TestBean)context.getBean("testBean");
````

https://www.mkyong.com/spring/spring-bean-scopes-examples/
http://www.baeldung.com/spring-bean-scopes