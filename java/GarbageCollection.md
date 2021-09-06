# Garbage Collection

- JVM에서 Heap 영역에 남아서 더 이상 사용되지 않는 인스턴스들을 가비지라고 한다.
- 메모리가 부족해질 경우 JVM은 이 가비지들을 삭제하여 추가로 사용할 수 있는 메모리 공간을 만든다. 이 과정을 가비지 콜렉팅이라고 한다.
- Weak generational hypothesis
  - 대부분의 객체는 금방 접근 불가능(unreachable) 상태가 된다.
  - 오래된 객체에서 젊은 객체로의 참조는 아주 적게 존재한다.
- Heap내에서 객체의 수명을 관리하기 위해 Young, Old 구역으로 나뉜다.
- Young 영역은 다시 Eden, 두 개의 Survivor 영역으로 나뉜다.
- Eden에는 처음 생성된 객체가 위치하게 된다. 그러다 Eden 영역이 꽉 차면 Minor GC 가 발생하게 되면, 이 영역에 위치하는 객체 중 참조되지 않는 객체는 메모리에서 제거되며, 살아남은 객체들은 Survivor 영역 두 군데 중 한 군데로 이동하게 된다. Minor GC 가 발동할 때마다, Survivor 영역에 있던 객체들은 다른 Survivor 영역으로 이동한다. 즉 최초에 Survivor 1 영역에 있던 객체는, Minor GC가 발동하면 Survivor 2 영역으로 이동하게 된다.
- 각 객체는 Minor GC에서 살아남은 횟수를 기록하는 age bit 를 가지고 있으며, 이 age bit는 Minor GC가 발생할 때마다 하나씩 증가한다. age bit값이 MaxTenuringThreshold 라는 설정값을 초과하게 되는 경우, Old Generation 영역으로 객체가 이동하게 된다.
- Stop The World
- Stop-the-world는 GC 실행을 위해 JVM이 애플리케이션 실행을 멈주는 것이다. GC가 실행 될 때는, GC를 실행하는 쓰레드를 제외한 모든 스레드들이 작업을 멈춘다. GC 작업이 완료한 이후에야 중단했던 작업을 다시 시작한다. 대개의 경우 GC 튜닝이란 이 stop-the-world 시간을 줄이는 것을 말한다.
- Reachability: 어떤 객체에 유효한 참조가 있으면 'reachable', 없으면 'unreachable'로 구별하고 'unreachable'객체를 가비지로 간주한다.
- 기본적으로 new로 할당되는 메모리들은 모두 Strong Reference를 가지기 때문에, 캐시와 같은 것을 만든다고 할 때 메모리 누수에 조심해야 한다. 캐시의 키가 원래 데이터에서 삭제가 된다면 캐시 내부의 키와 값은 더 이상 의미가 없는 데이터, 즉 가비지가 된다. 그럼에도 GC는 삭제된 캐시의 키를 가비지로 인식하지 못한다. 이는 캐시에 넣어준 데이터가 Strong Refrence로 독자적인 Reachablity를 가지기 때문이다. 따라서 캐시에 데이터를 넣어 줄 때, 원래 데이터에 Weak Reference를 넣어준다면 이러한 문제를 방지 할 수 있다. Weak Reference는 new로 할당된 객체의 유효 참조를 인위적으로 설정 할 수 있게 해주기 때문에, 원래의 데이터가 삭제되면 이 객체에 Weak Refrence가 걸려있는 객체들은 모두 가비지로 인식된다.