# SOLID

## SRP 단일 책임 원칙 (Single responsibility principle)
- 한 클래스는 하나의 책임만 가져야 한다.
- 클래스가 여러 책임을 갖게 되면 그 클래스는 각 책임마다 변경되는 이유가 발생하기 때문에 클래스가 한 개의 이유로만 변경되려면 클래스는 한 개의 책임만을 가져야한다.
- "클래스를 변경하는 이유는 단 한 개여야 한다."

## OCP	
- 개방-폐쇄 원칙 (Open/closed principle)
- 소프트웨어 요소는 확장에는 열려 있으나 변경에는 닫혀 있어야 한다.
- 기능을 변경하거나 확장할 수 있으면서 그 기능을 사용하는 코드는 수정하지 않는다.

## LSP	
- 리스코프 치환 원칙 (Liskov substitution principle)
- 프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 한다.
- 상위 타입의 객체를 하위 타입의 객체로 치환해도 상위 타입을 사용하는 프로그램은 정상적으로 동작해야 한다.

## ISP	
- 인터페이스 분리 원칙 (Interface segregation principle)
- 특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나보다 낫다.
- 인터페이스는 그 인터페이스를 사용하는 클라이언트를 기준으로 분리해야 한다.

## DIP	
- 의존관계 역전 원칙 (Dependency inversion principle)
- 추상화에 의존해야지, 구체화에 의존하면 안된다.
- 고수준 모듈은 저수준 모듈의 구현에 의존해서는 안 된다. 저수준 모듈이 고수준 모듈에서 정의한 추상 타입에 의존해야 한다.
- 변화하기 쉬운 것에 의존하기보다는, 변화하지 않는 것에 의존한다.

### example
````
// anti

class AbcPay {

    String payment() {
        return "Abc";
    }
    
}

public class PayService {

    private AbcPay pay;
    
    public void PayService(final AbcPay pay) {
        this.pay = pay;
    }

    public String payment() {
        return pay.payment();
    }
}
````

````
//DIP

public interface Pay {
    String payment();
}

class AbcPay implements Pay {
    @Override
    public String payment() {
        return "abc";
    }
}

public class DefPay implements Pay {
    @Override
    public String payment() {
        return "def";
    }
}

public class PayService {
    private Pay pay;
    
    public void PayService(final Pay pay) {
        this.pay = pay;
    }

    public String payment() {
        return pay.payment();
    }
}
````
