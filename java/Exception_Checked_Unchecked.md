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
spring에서 @Transactional을 사용한 Checked Exception rollback되지 않음
````
// 예외 rollback 지정
@Transactional(rollbackFor = Exception.class)

@Transactional(rollbackFro = {RuntimeException.class, Exception.class})


// 특정 예외 > rollback 되지 않도록 함
@Transactional(noRollbackFor={IgnoreRollbackException.class})
````

### handling-exceptions 
https://www.baeldung.com/java-exceptions#handling-exceptions 