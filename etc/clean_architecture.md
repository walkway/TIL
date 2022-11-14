# Clean Architecture

### 관심사 분리
- 프레임워크 독립적: 아키텍처는 소프트웨어 라이브러리 존재에 의존하지 않는다. 
- 테스트 용이함: 비즈니스 규칙은 UI, 데이터베이스, 웹 서버, 기타 외부 요인없이 테스트 가능해야 한다.
- UI 독립적: 시스템의 나머지 부분을 변경할 필요 없이 UI를 쉽게 변경할 수 있다.
- 데이터베이스 독립적:  비즈니스 규칙은 데이터베이스에 의존하지 않는다. (Oracle 또는 SQL Server를 Mongo, BigTable, CouchDB 또는 다른 것으로 교체)
- 외부 기능 독립적: 실제로 비즈니스 규칙은 외부 세계에 대해 모른다.

### Dependency 규칙
- 동심원은 소프트웨어의 각각의 영역이다.
- 내부(Policy) -> 외부(Mechanism) 외부 원으로 향할 수도록 고수준 소프트웨어이다.
- 의존성은 내부 원을 향한다. 내부 원은 외부 원에 대해 알지 못한다.
  - 외부 원에서 선언한 이름을 내부 원에서 참조하면 안된다.
  - 외부 원에서 사용하는 데이터 포맷은 내부 원에서 사용하지 않는다.
- 내부로 이동함에 따라 추상화 수준은 높아진다. 외부 원은 저수준의 구체적인 상세 정보를 담는다. 
- 내부 원으로 이동해가면서 소프트웨어는 추상화 되고, 고수준의 정책을 캡슐화한다. 가장 내부에 있는 원이 가장 일반적이다.

### Entities
- Enterprise wide 비즈니스 규칙을 캡슐화한다.
- 기능이 있는 객체, 데이터 구조 함수 집합이다.
- 외부에서 무언가 변경될 때, 변경될 가능성이 가장 적은 계층이다.
  - ex) 보안 변경에 의해 영향을 받는 개체로 예상하지 않고, 운영상 변경이 Entity 계층에 영향을 주면 안된다.
- Entity는 다른 계층을 알 수 없고, 어떤 것에도 의존하지 않는다.

### Use Cases
- 비즈니스 고유 규칙
- Entity로 부터의 혹은 Entity에서의 데이터 흐름을 조합한다.
- 계층의 변경이 Entity에 영향을 주지 않는다. 데이터베이스, UI 또는 공통의 프레임워크의 변경으로부터 영향 받지 않는다.
- Use Case 의 상세가 바뀐다면 이 계층의 코드는 영향을 받는다.
````
    Gather Info for New Loan
    
    Input:  Name, Address, Birthdate, etc.
    Output: Same info + credit score
    
    Rules:
      1. Validate name
      2. Validate address, etc.
      3. Get credit score
      4. If credit score < 500 activate Denial
      5. Else create Customer (entity) and activate Loan Estimation
````

### Interface Adapters
- Use Case와 Entity에 용이한 형식으로부터 데이터베이스나 웹 등 외부의 기능에 용이한 형식으로 데이터를 변환한다. (Presenters, Views, Controllers)
- 모델이 Controllers에서 Use Case로 전달되고,Use Case에서 Presenters, Views로 되돌아가는 단순한 데이터 구조일 가능성이 높다.
- 이 계층에서 데이터는, Entity나 Use Case에 용이한 형에서, 사용하고 있는 프레임워크에 용이한 형으로 변환된다. 

### Frameworks and Drivers / Infrastructure
- 가장 외부 계층은 데이터베이스나 웹 프레임워크 등 일반적으로 프레임워크나 도구로 구성된다. 
- 이 계층에는 내부의 원과 통신할 연결 코드 이외에는 별다른 코드를 작성하지 않는다.

https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html
https://pusher.com/tutorials/clean-architecture-introduction/
