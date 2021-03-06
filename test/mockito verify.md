# mockito verify

- verify(T mock): mock 작업이 한 번 수행 되었는지 검증
- verify(T mock, VerificationMode mode): mock 작업이 mode에 지정된 만큼 수행 되었는지를 검증

- atLeastOnce(): 적어도 한 번 수행했는지 검증
- atLeast(int n): 적어도 n번 수행 했는지 검증
- times(int n): n번 수행 되었는지 검증
- atMost(int n): 최대한 n번 수행 되었는지 검증
- never(): 수행되지 않았는지 검증

- thenAnswer(Answer<?> answer): Answer 인터페이스를 구현, 원하는 작업을 수행
- thenReturn(T value): 지정 값 리턴
- thenReturn(T value, T... values): 지정 값을 순차적으로 리턴
- thenThrow(java.lang.Throwable... throwables): 예외를 야기시키는 Throwable 객체를 지정

- inOrderObj.verify(T mock): 메소드 호출 순서 검증

````
public class Foo
{
    public void foo(){
        Bar bar = new Bar();
        bar.someMethod();
    }
}
````
````
verify(bar, times(1)).someMethod();
````
->
````
public class Foo {
  private BarFactory barFactory;

  public Foo(BarFactory factory) {
    this.barFactory = factory;
  }

  public void foo() {
    Bar bar = this.barFactory.createBar();
    bar.someMethod();
  }
}
````
````
@Test
public void testDoFoo() {
  Bar bar = mock(Bar.class);
  BarFactory myFactory = new BarFactory() {
    public Bar createBar() { return bar;}
  };

  Foo foo = new Foo(myFactory);
  foo.foo();

  verify(bar, times(1)).someMethod();
}
````
https://stackoverflow.com/questions/9841623/mockito-how-to-verify-method-was-called-on-an-object-created-within-a-method