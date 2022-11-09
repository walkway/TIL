# mysql query

### ORDER BY FIELD, FIND_IN_SET
- 원하는 순서로 
````
SELECT * FROM foo where id in (2,3,1) ORDER BY FIELD(id,2,3,1);  
SELECT * FROM foo where id in (2,3,1) ORDER BY FIND_IN_SET(id,"2,3,1");  
````
- ORDER BY가 없으면 기본적으로 해당 쿼리에서 사용되고 있는 index ASC 정렬
- SELECT 필드에 따라 index 타는게 달라질 수 있음. 순서 정렬이 필요하면 명시적으로 ORDER BY 추가

https://stackoverflow.com/questions/1631723/maintaining-order-in-mysql-in-query
