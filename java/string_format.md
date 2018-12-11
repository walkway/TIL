# String format

````
%[argument_index$][flags][width]conversion
````
````
String.format("%s, %s", "hello", "world")
````
- 1000 단위 (,)
````
//10,000
String.format("%,d", 10000);
````
- 자리수
````
//0010
String.format("%04d", 10);
````
https://www.novixys.com/blog/java-string-format-examples/
https://docs.oracle.com/javase/7/docs/api/java/util/Formatter.html