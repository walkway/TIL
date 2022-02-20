# Decorator pattern

- 데코레이터 패턴(Decorator pattern)이란 주어진 상황 및 용도에 따라 어떤 객체에 책임을 덧붙이는 패턴으로, 기능 확장이 필요할 때 서브클래싱 대신 쓸 수 있는 유연한 대안이 될 수 있다.
- 객체의 결합을 통해 기능을 동적으로 유연하게 확장
- 기본 기능에 추가할 수 있는 기능의 종류가 많은 경우에 각 추가 기능을 Decorator 클래스로 정의 한 후 필요한 Decorator 객체를 조합함으로써 추가 기능의 조합을 설계
````
public abstract class Beverage {
    String description = "no title";
 
    public abstract int cost();
 
    public String getDescription() {
        return description;
    }
    
}
````
````
public abstract class CondimentDecorator extends Beverage {
    
    public abstract String getDescription();
}
````
````
public class Americano extends Beverage {
 
    public Americano() {
        super();
        description = "아메리카노";
    }
 
    @Override
    public int cost() {
        return 4000;
    }
 
}
````
````
public class CaffeLatte extends Beverage {
 
    public CaffeLatte() {
        super();
        description = "카페라떼";
    }
 
    @Override
    public int cost() {
        return 5000;
    }
 
}
````
````
public class Cream extends CondimentDecorator {
 
    Beverage beverage;
    
        
    public Cream(Beverage beverage) {
        super();
        this.beverage = beverage;
    }
 
    @Override
    public String getDescription() {
        return beverage.getDescription() + ", 크림";
    }
 
    @Override
    public int cost() {
        // TODO Auto-generated method stub
        return beverage.cost() + 500;
    }
 
}
````
````
public class Shot extends CondimentDecorator {
    
    Beverage beverage;
 
    public Shot(Beverage beverage) {
        super();
        this.beverage = beverage;
    }
 
    @Override
    public String getDescription() {
        // TODO Auto-generated method stub
        return beverage.getDescription() + ", 샷";
    }
 
    @Override
    public int cost() {
        // TODO Auto-generated method stub
        return beverage.cost() + 400;
    }
 
}
````
````
public class Customer {
 
    public static void main(String[] args) {
        
        Beverage beverage = new Americano();
        beverage = new Shot(beverage);
        beverage = new Shot(beverage);
        
        System.out.println("메뉴 : " + beverage.getDescription());
        System.out.println("가격 : " + beverage.cost());
        // 메뉴 : 아메리카노, 샷, 샷
        // 가격 : 4800
        
    }
}
````