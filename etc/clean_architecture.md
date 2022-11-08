# Clean Architecture

### Entity
- 앱의 핵심 기능에 중요한 비즈니스 규칙의 집합
- OOP (객체 지향 프로그래밍) 에서 Entity에 대한 규칙은 한 클래스의 메서드로 그룹화
- Entity는 다른 계층을 알 수 없음. 어떤 것에도 의존하지 않음.

### Use cases
- 특정 상황에 대한 비즈니스 규칙
- 앱의 동작을 결정
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
- Use case는 Entity 계층에 의존적이지만, 그 위의 계층에 대해서는 아무것도 모름, 접속 장치/DB가 어떤 것인지도 상관하지 않음

### Adapters
- Interface Adapter
- Adapter는 domain과 infrastructure 사이 변환기
- ex) UI에서 입력받은 정보를 가져와 use case와 entity에 맞춰 repackage. 그런 다음 use case와 entity로 부터 받아온 데이터를 UI에 표시하거나 DB에 저장하기 위해 repackage

### Infrastructure
- UI, DB, 프레임워크, 장치 등 모든 I/O 구성 요소가 이동하는 곳, 가장 많은 변화
- 변화 가능성이 높기 때문에, domain 계층들로부터 가능한 멀리 떨어져있음

https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html
https://pusher.com/tutorials/clean-architecture-introduction/
