# kotlin

## 특징
- Type Inference
  - 할당되는 리터럴 값을 알아서 추론
- OOP, FP (Functional Programing)
  - 객체지향 프로그래밍, 함수형 프로그래밍을 모두 지원
  - 코틀린에서 함수는 일급 객체
  - 고차 함수로 사용
- 모든 타입 클래스 타입
  - 기본형 타입도 클래스로 존재
- Nullable
  - Null 할당이 가능한 타입을 선언할 수 있다.
  - NPE 예외를 컴파일 시점에서 미리 방지할 수 있다
````
// example 
fun main() { 
  print("Hello Kotlin")
}
````
- Boilerplate
  - 자바에서 데이터 캡슐화 원칙에 의해서 멤버 필드에 접근을 위해서 관례적으로 setter getter 메서드를 생성해준다. getter setter 뿐만 아니라 생성자와 같은 필수적인 코드가 들어간다.
  - 코틀린은 언어 차원에서 불필요하고 반복적인 코드들을 제거했다.
````
data class Member(val name: String, val age: Int)
````
- Try Catch
  - try catch 를 강제하는 Checked Exception 은 자바 최초 설계시 만들어졌다. 코틀린에서는 자바에서의 강제적인 예외처리를 제거했다.
- Default Argument
