# TDD
- 테스트 주도 개발(Test-driven development)
- 짧은 개발 사이클을 반복하는 소프트웨어 개발 프로세스 중 하나
- 1. arrange 2. act 3. asset
- red-green, refactor -> blue
- assert 범위가 SRP를 만족하는지 체크
- act는 한가지 역할만 하는지 체크
- 어떤 레벨로 테스트 할것인지 고민
- 처음 생각한대로 코드를 작성하고, 리팩토링을 반복
- @before : 초기화
- @after : 끝
````
assertThat(T actual, Matcher<? super T> matcher)
````
- BDD는 Behavior-Driven Development
- tip
 	- most simple
 	- defenerate test case first
 	- 너무 많이 생각하지 말고
- debugging time 줄여준다.
- decoupling
- test code -> production code
````
public class MovieTest {

    private Movie movie;

    @Before
    public void setUp() throws Exception {
        movie = new Movie();
    }

    @Test
    public void should_return_0_when_just_created(){
        assertThat(movie.averageRating(), is(0));
    }
    @Test
    public void should_return_1_when_1_was_rated(){
        movie.rate(1);
        assertThat(movie.averageRating(), is(1));
    }
    @Test
    public void should_return_4_when_3_and_5_were_rated(){
        movie.rate(3);
        movie.rate(5);
        assertThat(movie.averageRating(), is(4));
    }

}
````
````
public class Movie {
    private int sumOfRate = 0;
    private int countOfRate = 0;

    public Integer averageRating() {
        return countOfRate == 0 ? 0 : sumOfRate / countOfRate;
    }

    public void rate(int rate) {
        this.sumOfRate += rate;
        countOfRate ++;

    }
}
````
````
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.net.Socket;

import static org.hamcrest.Matchers.is;
import static org.junit.Assert.assertThat;

class TyrantMap {
    public static final int OPERATION_PREFIX = 0xC8;
    public static final int OPERATION_PUT = 0x10;
    public static final int OPERATION_GET = 0x30;
    private Socket socket;
    private DataOutputStream writer;
    private DataInputStream reader;

    public void open() throws IOException {
        socket = new Socket("localhost", 1978);
        writer = new DataOutputStream(socket.getOutputStream());
        reader = new DataInputStream(socket.getInputStream());
    }

    public void put(String key, String value) throws IOException {
        writeHeader(OPERATION_PUT);
        writeKeyValue(key, value);
        verifyStatus();
    }
    public byte[] get(String key) throws IOException {
        writeHeader(OPERATION_GET);
        writeKey(key, key.getBytes());
        verifyStatus();
        return readResults();
    }

    private byte[] readResults() throws IOException {
        int length = reader.readInt();
        byte[] results = new byte[length];
        reader.read(results);
        return results;
    }

    private void writeKey(String key, byte[] bytes) throws IOException {
        writer.writeInt(key.length());
        writer.write(bytes);
    }

    public void close() throws IOException {
        socket.close();
    }
    private void verifyStatus() throws IOException {
        int status = reader.read();
        assertThat(status, is(0));
    }

    private void writeKeyValue(String key, String value) throws IOException {
        writer.writeInt(key.length());
        writeKey(value, key.getBytes());
        writer.write(value.getBytes());
    }

    private void writeHeader(int operationCode) throws IOException {
        writer.write(OPERATION_PREFIX);
        writer.write(operationCode);
    }
}
````
````
import org.junit.Test;
import java.io.IOException;
import static org.hamcrest.Matchers.is;

public class TyrantTest {
	//Tyrant t = new Tyrant();
	//t.put("key", "value");
	//assertThat(t.get("key"), is("value"));
    @Test
    public void getRetrievesWhatWasPut() throws IOException {
        TyrantMap tyrantMap = new TyrantMap();
        tyrantMap.open();
        tyrantMap.put("key", "value");
        assertThat(tyrantMap.get("key", is("value".getBytes());
        tyrantMap.close();
    }
}
````