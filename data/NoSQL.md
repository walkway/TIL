# NoSQL

## 관계형 데이터 설계와 NoSQL의 차이점
- RDBMS에서는 데이터를 유연하게 쿼리할 수 있지만, 쿼리 비용이 상대적으로 높으며 트래픽이 많은 상황에서는 확장성이 떨어진다.
- NoSQL 데이터베이스에서는 몇 가지 방법으로 데이터를 효율적으로 쿼리할 수 있지만, 그 외에는 쿼리 비용이 높고 속도가 느리다.
- RDBMS의 경우, 세부적인 구현이나 성능을 걱정하지 않고 유연성을 목적으로 설계한다. 일반적으로 쿼리 최적화가 스키마 설계에 영향을 미치지 않지만, 정규화가 중요하다.
- NoSQL의 경우, 가장 중요하고 범용적인 쿼리를 가능한 빠르고 저렴하게 수행할 수 있도록 스키마를 설계한다. 사용자의 데이터 구조는 사용자 비즈니스 사용 사례의 특정 요구 사항에 적합하도록 만든다.
- NoSQL 데이터베이스들은 "궁극적인 일관성" 개념을 제공함으로써 데이터베이스의 변경사항이 모든 노드에 "궁극적으로"(일반적으로 밀리초 내) 전파되므로 데이터에 대한 모든 쿼리들이 즉각 업데이트된 데이터를 반환하지 않을 수 있고 정확하지 않은 데이터를 읽는 결과가 발생할 수 있다. 스테일 리드(stale read)

## NoSQL 데이터베이스의 유형
### 문서 데이터베이스
- JSON(JavaScript Object Notation) 객체와 비슷한 문서에 데이터를 저장한다.
- 각 문서에는 필드와 값의 쌍이 포함되어 있다. 일반적으로 값은 문자열, 숫자, 불리언(boolean), 어레이, 객체 등 유형이 다양할 수 있으며, 구조는 개발자가 코드에서 사용하고 있는 객체에 맞춰 조정된다다. 다양한 필드 값 유형과 강력한 쿼리 언어 덕분에 문서 데이터베이스는 다양한 사용 사례에 적합하며, 범용 데이터베이스로도 사용할 수 있다. 
- 대량의 데이터를 수용하도록 수평 스케일아웃이 가능하다. 

### 키-값 데이터베이스
- 각 항목에 키와 값이 포함되어 있는 보다 간단한 유형의 데이터베이스이다.
- 값은 보통 키를 참조하는 방식으로만 검색이 가능하다. 키-값 데이터베이스는 대량의 데이터를 저장해야 하지만 검색을 위해 복잡한 쿼리를 수행할 필요가 없는 사용 사례에 적합하다.

### 와이드 컬럼 스토어
- 테이블, 행 및 동적 열에 데이터를 저장한다. 와이드 컬럼 스토어는 각 행이 동일한 열을 가질 필요가 없다는 점에서 관계형 데이터베이스에 대해 뛰어난 유연성을 제공한다.
- 와이드 컬럼 스토어는 대량의 데이터를 저장해야 할 때 적합하며, 쿼리 패턴을 예측할 수 있다. 와이드 컬럼 스토어는 보통 사물인터넷 데이터와 사용자 프로필 데이터를 저장하는 데 사용된다.

### 그래프 데이터베이스
- 노드와 에지에 데이터를 저장한다. 노드에는 일반적으로 사람, 장소 및 사물에 대한 정보가 저장되고, 에지에는 노드 간의 관계에 대한 정보가 저장된다. 
- 소셜 네트워크, 사기 탐지, 권장 엔진 같은 패턴을 찾아보기 위해 관계를 상세히 검토해야 하는 사용 사례에 적합하다.
