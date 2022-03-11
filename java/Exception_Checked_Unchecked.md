# Checked, Unchecked Exception

## Checked Exception
- 반드시 예외 처리해야 함
  - throw 키워드를 사용해서 선언적으로 예외를 던지거나
  - try-catch 예외를 직접 처리
- Transaction rollback (x)
- RuntimeException 상속하지 않음
- ex) IOException, SQLException
- https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Exception.html

## Unchecked Exception
- 명시적으로 예외 처리하지 않아도 됨
- Transaction rollback (o)
- RuntimeException 상속
- ex) NullPointerException, IllegalArgumentException
- https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/RuntimeException.html

## Error
- 라이브러리 비 호환성, 무한 재귀 또는 메모리 누수와 같은 심각하고 일반적으로 복구 할 수없는 상태
- Error를 상속하는 클래스들은 UnCheckedException 형태로 동작
- ex) OutOfMemoryError, StackOverflowError 
- https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Error.html 

## @Transactional
spring에서 @Transactional 붙은 클래스는 프록시를 생성하고, 프록시는 트랜잭션 로직을 메서드 앞 뒤에 넣음
spring에서 @Transactional을 사용한 Checked Exception rollback되지 않음
````
// 예외 rollback 지정
@Transactional(rollbackFor = Exception.class)

@Transactional(rollbackFro = {RuntimeException.class, Exception.class})


// 특정 예외 > rollback 되지 않도록 함
@Transactional(noRollbackFor={IgnoreRollbackException.class})
````

### example
1. rollback(o) - unchecked exception
````
@Transactional
public void createProduct() {  
    Product prod = new Product();
    productRepository.save(prod);
    System.out.println("Product inserted.");
    throw new RuntimeException();
}
````

2. rollback(x) - checked exception
````
@Transactional
public void createProduct() throws Exception {  
    Product prod = new Product();
    productRepository.save(prod);
    System.out.println("Product inserted.");
    throw new SQLException();
}
````

3. rollback(o) - checked exception, rollbackFor 설정
````
@Transactional(rollbackFor = SQLException.class)
public void createProduct() throws Exception {
    Product prod = new Product();
    productRepository.save(prod);
    System.out.println("Product inserted.");
    throw new SQLException();
}
````

4. rollback(x) - catch exception
````
@Transactional
public void createProduct() {
    try {
        Product prod = new Product();
        productRepository.save(prod);
        System.out.println("Product inserted.");
        throw new RuntimeException();
    }catch (Exception e){
        System.out.println("Here we catch the exception.");
    }
}
````

5. rollback(o) - createProduct(), createOrder() / nested transaction
createOrder()에서 RuntimeException가 발생했지만, 예외를 처리하지 않기 때문에 createProduct() 트랜잭션도 rollback
````
//ProductController.java
@Transactional
public void createProduct() {
    Product prod = new Product();
    prod.setTitle("Create Product");
    productRepository.save(prod);
    orderController.createOrder();
}

//OrderController.java
@Transactional
public void createOrder() {
    System.out.println("------ createOrder ------");
    Order order = new Order();
    order.setTitle("Create Order");
    orderRepository.save(order);  
    throw new RuntimeException("Create Order RuntimeException");
}
````

6. rollback(o) - product written / rollback(o) - order record  / nested transaction
Propagation 설정에 따라 트랜잭션을 시작하고 종료한다. 기본적으로 REQUIRED 설정된다. 활성 트랜잭션이 있는 경우 Spring은 새 트랜잭션을 생성하는 대신 이를 사용한다.  
외부 메서드에서 예외를 포착해도, 트랜잭션은 롤백된다.

````
// Transaction silently rolled back because it has been marked as rollback-only

//ProductController.java
@Transactional
public void createProduct() {
    Product prod = new Product();
    prod.setTitle("Create Product");
    productRepository.save(prod);
    try {
        orderController.createOrder();
    } catch (RuntimeException e){
        System.out.println("Handle " + e.getMessage());
    }
}

//OrderController.java
@Transactional
public void createOrder() {
    Order order = new Order();
    order.setTitle("Create Order");
    orderRepository.save(order);  
    throw new RuntimeException("Create Order RuntimeException");
}
````

7. rollback(x) - product written / rollback(o) - order record  / nested transaction REQUIRES_NEW, catch exception
````
@Transactional
public void createProduct() {
    Product prod = new Product();
    prod.setTitle("Create Product");
    productRepository.save(prod);
    try {
        orderController.createOrder();
    } catch (RuntimeException e){
        System.out.println("Handle " + e.getMessage());
    }
}

@Transactional(propagation=Propagation.REQUIRES_NEW)
public void createOrder() {
    Order order = new Order();
    order.setTitle("Create Order");
    orderRepository.save(order);  
    throw new RuntimeException("Create Order RuntimeException");
}
````

8. rollback(o) - product written / rollback(o) - order record  / nested transaction REQUIRES_NEW
````
@Transactional
public void createProduct() {
    Product prod = new Product();
    prod.setTitle("Create Product");
    productRepository.save(prod);
    orderController.createOrder();
}
@Transactional(propagation=Propagation.REQUIRES_NEW)
public void createOrder() {
    Order order = new Order();
    order.setTitle("Create Order");
    orderRepository.save(order);  
    throw new RuntimeException("Create Order RuntimeException");
}
````

https://medium.com/geekculture/spring-transactional-rollback-handling-741fcad043c6

### handling-exceptions 
https://www.baeldung.com/java-exceptions#handling-exceptions 