# test private
````
import java.util.List;

public class A {
	private String name;

	public A (String name) {
		this.name = name;
	}
	
	private String privateMethod(int a, String b, List list) {
	  return "name: " + this.name + 
            ", a: " + a + ", b: " + b + ", List: " + list;
	}
}
````
````
public class ATest {
  A a = new A("TEST");

  @Test
  public void test() throws Exception{
  	Method method = A.class.getDeclaredMethod("privateMethod", int.class, String.class, List.class);
  	method.setAccessible(true);
    String result = String.class.cast(method.invoke(a, 1, "string", new ArrayList(Arrays.asList(1,2,3))));
    assertThat(result, CoreMatchers.is("name: TEST, a: 1, b: string, List: [1, 2, 3]"));
  }
}
````