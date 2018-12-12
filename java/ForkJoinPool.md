#ForkJoinPool

- 일을 작은 업무로 분배 -> 취합
- Work-Stealing
- 큰 업무를 작은 단위의 업무로 쪼갠다.
- 부모 쓰레드로 부터 처리로직을 복사하여 새로운 쓰레드에서  쪼개진 업무를 수행(Fork) 시킨다.
- 특정 쓰레드에서 더이상 Fork가 일어나지 않고 업무가 완료되면 그 결과를 부모 쓰레드에서 Join하여 값을 취합한다.
- 최초에 ForkJoinPool을 생성한 쓰레드로 값을 리턴하여 작업을 완료한다.
- RecursiveAction
  - 결과를 반환하지 않는 task
  - 특정 작업을 분할하고, 처리를 수행하면서 해당 작업을 내부에서 처리
- RecursiveTask
  - 결과를 반환하는 task를 fork, join으로 처리