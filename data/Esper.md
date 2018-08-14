### Esper

- Esper is a component for complex event processing (CEP) and event series analysis.
- 실시간으로 발생하는 이벤트를 분석하고 처리하기 위한 컴포넌트
- EPL(Event Processing Language): 실시간으로 발생되는 이벤트에 대해 쿼리를 실행
- In-Memory 기반 시스템
- Lightweight & Embedda
- Esper 구성: Input Adapter, CEP Engine, EPL, Output Adapter
- 윈도우: 윈도우는 일정 시간 동안 또는 일정 개수의 이벤트를 메모리에 보관하고, EPL에서는 이 윈도우에 보관된 이벤트 목록을 대상으로 결과를 뽑아냄.
 - 시간 윈도우
 - 시간 배치 윈도우
 - 길이 윈도우
 - 길이 배치 윈도우
- ex)
````
select first(*) as tick1, last(*) as tick2 
from StockTick.win:time(5 seconds) 
group by code 
having first(*) != last(*) and (last(cost) - first(cost)) / first(cost) >= 0.05
````
````
StockTick 이벤트를 최근 5초 기준으로
code를 이용해서 그룹핑 하고,
그룹에서 최근 5초 이내 첫 번째와 마지막 이벤트가 다르고, 첫 번째 이벤트의 cost보다 마지막 이벤트의 cost가 5% 이상 값이 크면,
그룹의 첫 번째 이벤트와 마지막 이벤트를 선택한다
````
- http://javacan.tistory.com/entry/Esper-Beginner-1-Quick-Start