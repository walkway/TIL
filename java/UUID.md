# UUID
- 범용 고유 식별자(universally unique identifier, UUID)는 소프트웨어 구축에 쓰이는 식별자 표준으로, 개방 소프트웨어 재단(OSF)이 분산 컴퓨팅 환경(DCE)의 일부로 표준화하였다.
````
import java.util.UUID;

public class UUIDTest {
    public static void main(String[] args) {
        UUID uuid1 = UUID.randomUUID();
        UUID uuid2 = UUID.randomUUID();
        System.out.println("UUID One: " + uuid1.toString());
        System.out.println("UUID Two: " + uuid2.toString());
    }
}
````