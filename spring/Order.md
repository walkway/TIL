# @Order

- 같은 타입의 빈이 컬렉션(List등)에 Autowired 될 때 순서를 지정한다.
- 낮은 숫자가 우선순위가 높다.
````
@Component
@Order(1)
public class Excellent implements Rating {
 
    @Override
    public int getRating() {
        return 1;
    }
}
 
@Component
@Order(2)
public class Good implements Rating {
 
    @Override
    public int getRating() {
        return 2;
    }
}
 
@Component
@Order(Ordered.LOWEST_PRECEDENCE)
public class Average implements Rating {
 
    @Override
    public int getRating() {
        return 3;
    }
}
````
````
public class RatingRetrieverUnitTest { 
     
    @Autowired
    private List<Rating> ratings;
     
    @Test
    public void givenOrder_whenInjected_thenByOrderValue() {
        assertThat(ratings.get(0).getRating(), is(equalTo(1)));
        assertThat(ratings.get(1).getRating(), is(equalTo(2)));
        assertThat(ratings.get(2).getRating(), is(equalTo(3)));
    }
}
````
https://www.baeldung.com/spring-order