# Factory Pattern

- 객체를 만드는 공장(Factory 객체)

````
public abstract class Test {
    public abstract String getName();

    @Override
    public String toString() {
        return "product name : " + getName();
    }
}
````
````
public class A extends Test {
    private String name;

    public A (String name) {
        this.name = name;
    }

    @Override
    public String getName() {
        return this.name;
    }

    public void toStrig () {
        System.out.println(this.name);
    }
}
````
````
public class B extends Test {
    private String name;

    public B (String name) {
        this.name = name;
    }

    @Override
    public String getName() {
        return this.name;
    }

    public void toStrig () {
        System.out.println(this.name);
    }
}
````
````
public class Factory {
    public static Product getProduct(String type, String name) {
        if ( "a".equals(type) )
            return new A(name);
        else if ( "b".equals(type) )
            return new B(name);
        return null;
    }
}
````
````
public class main {
    public static void main(String[] args) {
        Test t1 = Factory.getProduct("a", "A");
        Test t2 = Factory.getProduct("b", "B");
    }
}
````
