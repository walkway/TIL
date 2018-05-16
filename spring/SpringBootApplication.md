# @SpringBootApplication

- spring boot 프로젝트를 만들면, base package에 @SpringBootApplication 어노테이션을 포함한 Application class 생긴다.
- @SpringBootApplication은 base package의 sub-packages를 스캔한다.
- com.project 안에 Application class가 있으면, com.project.*. 를 스캔한다.
- override @SpringBootApplication
- package a, package b
````
@SpringBootApplication(scanBasePackages = "a")
````
````
@SpringBootApplication(scanBasePackages = {"a", "b"})
````

