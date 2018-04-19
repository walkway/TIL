# Thymeleaf

- Spring boot 지원
- server-side temlate engine
- maven: dependecy 추가
````
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
````
- controller
````
@Controller
@RequestMapping("/index")
public class TestController {
	@RequestMapping(method=RequestMethod.GET)
	public String index(Model model) {
		model.addAttribute("hello", "Hello World");
		return "index";
	}
}
````
- templates/index.html
````
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8"></meta>
</head>
<body>
	<h1 th:text="${hello}"></h1>
</body>
</html>
````
### Thymeleaf Layout Dialect
- 코드 재사용이 가능한 레이아웃, 템플릿 라이브러리
- spring-boot-starter-thymeleaf 의존성 포함
- https://github.com/ultraq/thymeleaf-layout-dialect

- th:replace
	- replace에 정의된 html 파일로 치환
	- templates
	- :: 뒤에 fragment속성의 이름
- layout:fragment
	- 공통 레이아웃에서 선언
	- 컨텐츠 페이지에서 로드
````
<!DOCTYPE html>
<html lang="ko" xmlns="http://www.w3.org/1999/xhtml"
				xmlns:th="http://www.thymeleaf.org">
	<head th:replace="fragments/header :: head"> </head>
	<body th:replace="fragments/body :: body"> </body>
</html>
````
- head
````
<!DOCTYPE html>
<html lang="ko" xmlns="http://www.w3.org/1999/xhtml"
				xmlns:th="http://www.thymeleaf.org" xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout">
<head th:fragment="head">
    <th:block th:include="경로"></th:block>
    <link rel="stylesheet" th:href="@{경로}"/>
</head>
</html>
````