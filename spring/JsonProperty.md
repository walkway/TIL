# @JsonProperty

- getter/setter의 이름을 property 와 다른 이름을 사용할 수 있도록 설정

````
public class Form {

private boolean isActive;

public boolean isActive() {
    return isActive;
}
````
- boolean 타입을 가져오지 못하는 문제
- Jackson determines the property name from either the getter or setter
- 해결 방법
````
public boolean getIsActive() {
    return isActive;
}

public void setIsActive(boolean isActive) {
    this.isActive = isActive;
}
````
or
````
@JsonProperty
private boolean isActive;
````