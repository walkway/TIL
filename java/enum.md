# enum

- 서로 연관된 상수들의 집합
````
public class Shoes{
	public String name;
	public int size;
	public enum Type{
		WALKING, RUNNING, TRACKING, HIKING
	}
}
````
````
enum Type{
	WALKING, RUNNING, TRACKING, HIKING
}

public class Shoes{
	public String name;
	public int size;
	public Type type;

	public static void main(String[] args){
		for(Type type : Type.values()){
			System.out.println(type);
		}
	}
}
````
````
enum Type {
    WALKING("워킹화"), RUNNING("러닝화"), TRACKING("트래킹화"), HIKING("등산화")
     
    final private String name;
     
    private Type(Stirng name) { //enum에서 생성자 같은 역할
        this.name = name;
    }
    public String getName() { // 문자를 받아오는 함수
        return name;
    }
}
````
https://opentutorials.org/module/1226/8025
https://opentutorials.org/module/516/6091