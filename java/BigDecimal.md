# BigDecimal

````
BigDecimal num1 = new BigDecimal("100.100");
BigDecimal num2 = new BigDecimal("200.100");
````
- 더하기
````
num1.add(num2);
````
- 빼기
````
num1.subtract(num2);
````
- 곱하기
````
num1.multiply(num2);
````
- 나누기
````
num1.divide(num2, BigDecimal.ROUND_UP); //올림
num1.divide(num2, BigDecimal.ROUND_DOWN); //버림
num1.divide(num2, BigDecimal.ROUND_HALF_UP); //반올림
num1.divide(num2, BigDecimal.ROUND_HALF_DOWN); //반내림 

num1.divide(num2, 2, BigDecimal.ROUND_UP); //소수 2째자리까지 표시, 올림
````