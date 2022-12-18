# Chain of Responsibility Pattern

- 명령 객체와 일련의 처리 객체를 포함하는 디자인 패턴이다.
- 각각의 처리 객체는 명령 객체를 처리할 수 있는 연산의 집합이고, 체인 안의 처리 객체가 핸들할 수 없는 명령은 다음 처리 객체로 넘겨진다. 이 작동방식은 새로운 처리 객체부터 체인의 끝까지 다시 반복된다.

````
public class Chain {
    Processor chain;

    public Chain() {
        buildChain();
    }

    private void buildChain() {
        chain = new NegativeProcessor(new ZeroProcessor(new PositiveProcessor(null)));
    }

    public void process(Number request) {
        chain.process(request);
    }

}

abstract class Processor {
    private Processor nextProcessor;

    public Processor(Processor nextProcessor) {
        this.nextProcessor = nextProcessor;
    }

    ;

    public void process(Number request) {
        if (nextProcessor != null)
            nextProcessor.process(request);
    }

    ;
}

class Number {
    private int number;

    public Number(int number) {
        this.number = number;
    }

    public int getNumber() {
        return number;
    }

}

class NegativeProcessor extends Processor {
    public NegativeProcessor(Processor nextProcessor) {
        super(nextProcessor);

    }

    public void process(Number request) {
        if (request.getNumber() < 0) {
            System.out.println("NegativeProcessor : " + request.getNumber());
        } else {
            super.process(request);
        }
    }
}

class ZeroProcessor extends Processor {
    public ZeroProcessor(Processor nextProcessor) {
        super(nextProcessor);
    }

    public void process(Number request) {
        if (request.getNumber() == 0) {
            System.out.println("ZeroProcessor : " + request.getNumber());
        } else {
            super.process(request);
        }
    }
}

class PositiveProcessor extends Processor {

    public PositiveProcessor(Processor nextProcessor) {
        super(nextProcessor);
    }

    public void process(Number request) {
        if (request.getNumber() > 0) {
            System.out.println("PositiveProcessor : " + request.getNumber());
        } else {
            super.process(request);
        }
    }
}

class TestChain {
    public static void main(String[] args) {
        Chain chain = new Chain();

        //Calling chain of responsibility 
        chain.process(new Number(90));
        chain.process(new Number(-50));
        chain.process(new Number(0));
        chain.process(new Number(91));
    }
} 
````

````
PositiveProcessor : 90
NegativeProcessor : -50
ZeroProcessor : 0
PositiveProcessor : 91
````

https://www.geeksforgeeks.org/chain-responsibility-design-pattern/
