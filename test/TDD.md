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