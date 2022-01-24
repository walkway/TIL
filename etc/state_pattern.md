# State Pattern

- 객체가 특정 상태에 따라 행위를 달리하는 상황에서, 자신이 직접 상태를 체크하여 상태에 따라 행위를 호출하지 않고, 상태를 객체화 하여 상태가 행동을 할 수 있도록 위임하는 패턴
- 객체의 특정 상태를 클래스로 선언하고, 클래스에서는 해당 상태에서 할 수 있는 행위들을 메서드 정의
- 각 상태 클래스들을 인터페이스로 캡슐화 하여, 클라이언트에서 인터페이스를 호출하는 방식
````
public interface PowerState {
    public void powerPush();
}
````
````
public class On implements PowerState{
    public void powerPush(){
        System.out.println("off");
    }
}


public class Off implements PowerState {
    public void powerPush(){
        System.out.println("saving");
    }
}

public class Saving implements PowerState {
    public void powerPush(){
        System.out.println("on");
    }
}
````
````
public class Laptop {
    private PowerState powerState;

    public Laptop(){
        this.powerState = new Off();
    }

    public void setPowerState(PowerState powerState){
        this.powerState = powerState;
    }

    public void powerPush(){
        powerState.powerPush();
    }
}
````
````
public class Client {
    public static void main(String args[]){
        Laptop laptop = new Laptop();
        On on = new On();
        Off off = new Off();
        Saving saving = new Saving();

        laptop.powerPush();
        laptop.setPowerState(on);
        laptop.powerPush();
        laptop.setPowerState(saving);
        laptop.powerPush();
        laptop.setPowerState(off);
        laptop.powerPush();
        laptop.setPowerState(on);
        laptop.powerPush();
    }
}
````
