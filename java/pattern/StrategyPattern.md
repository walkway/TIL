# strategy pattern

- 행위 패턴에 해당되는 전략 패턴은 전략을 쉽게 바꿀 수 있도록 해주는 디자인 패턴
- 어떤 목적을 달성하기 위해 일을 수행하는 방식, 비즈니스 규칙, 문제를 해결하는 알고리즘
- Context: Strategy의 메소드를 호출해서 사용하는 클래스
- Strategy: 전략을 사용하기 위한 인터페이스
- ConcreteStrategyA, B, C: Strategy 인터페이스를 실제 구현하는 클래스

````
public interface SoundStrategy {
    void cry();
}

public class CryStrategy implements SoundStrategy {
    @Override
    public void cry() {
        System.out.println("cry");
    }
}

public class NoCryStrategy implements SoundStrategy {
    @Override
    public void cry() {
        System.out.println("-");
    }
}
````
````
public class CryingContext {

    private SoundStrategy soundStrategy;

    public void cry() {
        soundStrategy.cry();
    }

    public void setSoundStrategy(SoundStrategy soundStrategy) {
        this.soundStrategy = soundStrategy;
    }
}

public class Person extends CryingContext {

    public Person() {
        System.out.println("사람");
    }

}

public class Duck extends CryingContext {

    public Duck() {
        System.out.println("오리");
    }

}
````
````
CryingContext person = new Person();
CryingContext duck = new Duck();

person.setSoundStrategy(new NoCryStrategy());
duck.setSoundStrategy(new CryStrategy());

person.cry();
duck.cry();

person.setSoundStrategy(new CryStrategy());
person.cry();
````
