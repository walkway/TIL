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
<title>Hello Millky</title>
</head>
<body>
	<h1>${hello}</h1>
</body>
</html>
````