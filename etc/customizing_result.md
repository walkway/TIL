# customizing result

````
// message
No converter found capable of converting from type [org.springframework.data.jpa.repository.query.AbstractJpaQuery$TupleConverter$TupleBackedMap] to type

->
JPA queries typically produce their results as instances of a mapped entity. However, queries with aggregation functions normally return the result as Object[].
````
1. constructor - object-oriented
````
public class ItemCount {
    
    private Integer year;
    private Long count;

    public CommentCount(Integer year, Long count) {
        this.year = year;
        this.count = count;
    }
}
````
````
@Repository
public interface ItemRepository extends JpaRepository<Item, Integer> {
    
    @Query("SELECT new ItemCount(i.year, COUNT(i.year)) "
    	+ "FROM Item AS i " 
    	+ "GROUP BY i.year "
    	+ "ORDER BY i.year DESC"
	List<ItemCount> countByYear();
}
````
matches the projected attributes exactly

2. interface-based projection
````
public interface ItemCount {
    Integer getAccessHour());
    Long getAccessCount();
}
````
````
@Repository
public interface CommentRepository extends JpaRepository<Item, Integer> {
    
    @Query(value = "SELECT HOUR(a.created_at) AS accessHour, COUNT(a.created_at) AS accessCount "
    		+ "FROM ACCESS a "
			+ "WHERE created_at >= '2022-03-20 00:00:00' and created_at <= '2022-03-20 24:00:00' " 
			+ "GROUP BY HOUR(accessHour) "
			+ "ORDER BY HOUR(accessCount) ASC", nativeQuery = true)
	List<ItemCount> countTotalByHour();
}
````

https://www.baeldung.com/jpa-queries-custom-result-with-aggregation-functions