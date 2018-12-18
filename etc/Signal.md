# Signal
- 프로세스에게 어떤 Event의 발생을 알리기 위해 전달 되는 소프트웨어 인터럽트
- Hadware Exception(ex 1/0)
- Software Condition(Alram)
- 단말기에서 발생하는 사용자 입력
- System call(kill)

````
SIGABRT	6	종료 (코어 덤프)	프로세스 중단 신호
SIGALRM	14	종료	알람 클럭
SIGBUS	없음	종료 (코어 덤프)	정의되지 않은 메모리 오브젝트의 일부분에 접근.
SIGCHLD	없음	무시	차일드 프로세스 종료, 중단, 계속
SIGCONT	없음	계속	정지하지 않으면 계속 실행.
SIGFPE	없음	종료 (코어 덤프)	오류가 있는 산술 조작.
SIGHUP	1	종료	행업(Hangup).
SIGILL	없음	종료 (코어 덤프)	유효하지 않은 명령.
SIGINT	2	종료	터미널 인터럽트 신호.
SIGKILL	9	종료	킬 (신호를 잡거나 무시할 수 없음).
SIGPIPE	없음	종료	신호를 읽는 사용자가 없는 상태에서 파이프에 기록.
SIGPOLL	없음	종료	폴링 가능한 이벤트.
SIGPROF	없음	종료	프로파일링 타이머 시간 초과.
SIGQUIT	3	종료 (코어 덤프)	터미널 종료 신호.
SIGSEGV	없음	종료 (코어 덤프)	잘못된 메모리 참조.
SIGSTOP	없음	정지	실행 정지 (신호를 잡거나 무시할 수 없음)
SIGSYS	없음	종료 (코어 덤프)	불량 시스템 호출.
SIGTERM	15	종료	종료 신호.
SIGTRAP	없음	종료 (코어 덤프)	트레이스/브레이크포인트 트랩.
SIGTSTP	없음	정지	터미널 정지 신호.
SIGTTIN	없음	정지	백그라운드 프로세스 읽기 시도.
SIGTTOU	없음	정지	백그라운드 프로세스 쓰기 시도.
SIGUSR1	없음	종료	사용자 정의 신호 1.
SIGUSR2	없음	종료	사용자 정의 신호 2.
SIGURG	없음	무시	높은 대역의 데이터를 소켓에서 이용 가능.
SIGVTALRM	없음	종료	가상 타이머 시간 초과.
SIGXCPU	없음	종료 (코어 덤프)	CPU 시간 제한 초과.
SIGXFSZ	없음	종료 (코어 덤프)	파일 크기 제한 초과.
````
