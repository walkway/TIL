# TaskManager Memory

## Total Memory

- Flink JVM 프로세스의 전체 프로세스 메모리는 Flink 애플리케이션에서 소비하는 메모리(총 Flink 메모리)와 JVM이 프로세스를 실행하는 데 사용하는 메모리로 구성된다.
- 총 Flink 메모리 소비에는 JVM Heap, 관리 메모리 (Flink에서 관리) 및 기타 직접(또는 기본) 메모리 사용이 포함된다.
- Flink에서 메모리를 설정하는 또다른 방법은 작업 힙과 관리되는 메모리를 명시적으로 지정할 수 있다.

## Task(연산자) Heap Memory
- 사용자 코드에 대해 특정 양의 JVM Heap을 사용할 수 있도록 보장하려면 Task Heap Memory를 명시적으로(taskmanager.memory.task.heap.size) 설정할 수 있다.
- JVM Heap 크기에 추가되고 사용자 코드를 실행하는 Flink의 운영자 전용

## Managed Memory
- Flink에서 관리하며 기본 메모리(Off-Heap)로 할당된다.
- 명시적으로 구성(taskmanager.memory.managed.size)
- 또는 전체 Flink 메모리에서 (taskmanager.memory.managed.fraction) 일부로 계산
- 가중치(taskmanager.memory.managed.consumer-weights)

## Off-Heap Memory
- 사용자 코드에 의해 할당된 Off-Heap Memory는 (taskmanager.memory.task.off-heap.size)으로 설정 할 수 있다.
- 프레임워크 Off-Heap Memory는 Flink 프레임워크에 더 많은 메모리가 필요하다고 확신하는 경우에만 값을 변경한다.

https://nightlies.apache.org/flink/flink-docs-master/docs/deployment/memory/mem_setup_tm/
