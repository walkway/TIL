# verify

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