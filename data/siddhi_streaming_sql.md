# Siddhi Streaming SQL
- Each Siddhi Application is an isolated processing unit that allows you to deploy and execute queries independent of other Siddhi applications in the system.
- Stream: 고유하게 식별 가능한 이름으로 시간 순서대로 정렬 된 논리적 인 일련의 이벤트 및 해당 스키마를 정의하는 특정 데이터 유형으로 정의 된 속성 세트이다.
- Event: 이벤트는 하나의 스트림에만 연관되며 해당 스트림의 모든 이벤트는 특정 유형 (또는 동일한 스키마)이 지정된 동일한 속성 세트를 갖는다.
- Table: 정의 된 스키마와 함께 저장된 데이터의 구조화 된 표현. In-Memory, RDBMs, MongoDB, 등의 액세스 및 런타임에 조작 할 수 있다.
- Query: 기존 스트림, 테이블을 결합하여 스트리밍 방식으로 이벤트를 처리하고, 출력 스트림이나 테이블에 이벤트를 생성한다. 쿼리는 하나 이상의 입력 스트림과 0 또는 하나의 테이블을 사용한다. 이벤트를 스트리밍 방식으로 처리하고 출력 이벤트를 스트림 또는 테이블에서 처리할 수 있다..?
- Source: 외부 소스로부터의 데이터를 스트림에 전달한다.
- Sink: 미리 정의된 데이터 포맷으로 도착한 이벤트를 처리한다.
- Input Handler: 이벤트를 스트림에 프로그램 방식으로 주입한다.
- Stream/Query Callback: 스트림 및 쿼리의 출력 이벤트를 프로그래밍 방식으로 사용한다.

# Stream
````
define stream <stream name> (<attribute name> <attribute type>, <attribute name> <attribute type>, ... );
````

# Source
- 여러 전송 및 다양한 데이터 형식을 통해 이벤트를 수신하고 처리하기 위해, 스트림으로 전달한다.
- siddhi가 외부 시스템에서 이벤트를 consume할 수 있도록 한다.
````
@source(type='source_type', static.option.key1='static_option_value1', static.option.keyN='static_option_valueN',
    @map(type='map_type', static.option_key1='static_option_value1', static.option.keyN='static_option_valueN',
        @attributes( attributeN='attribute_mapping_N', attribute1='attribute_mapping_1')
    )
)
define stream StreamName (attribute1 Type1, attributeN TypeN);
````

# Windows
- Windows를 사용하면 계산을 위해 입력 스트림의 특정 기준을 기반으로 이벤트의 하위 집합을 캡처 할 수 있다.
- 각 입력 스트림은 최대 하나의 창만 가질 수 있다.
- 처리 시간, 이벤트 수 등을 기반으로 스트림 내의 이벤트 서브 세트를 작성합니다. 창은 슬라이딩 또는 텀블링 (배치) 방식으로 작동 할 수 있다.
- #window접두사 창을 사용하기 위해서는 다음 관련 스트림에 삽입해야 한다.
````
from <input stream>#window.<window name>(<parameter>, <parameter>, ... )
select <attribute name>, <attribute name>, ...
insert <event type> into <output stream>
````