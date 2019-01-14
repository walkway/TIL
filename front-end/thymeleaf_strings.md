# thymeleaf string

- 문자 연결
````
<p th:text="'name: ' + ${name}"></p>
````
````
<p th:text="|name: ${ame}|"></p>
````

- 기본 값
````
<p th:text="${name}?: 'unknown'"></p>
````
````
<p th:text="${#strings.defaultString(name, 'unknown')}"></p>
````
- 함수
````
<p th:text="${#strings.indexOf(name,frag)}"></p>
<p th:text="${#strings.substring(name,3,5)}"></p>
<p th:text="${#strings.substringAfter(name,prefix)}"></p>
<p th:text="${#strings.substringBefore(name,suffix)}"></p>
<p th:text="${#strings.replace(name,'las','ler')}"></p>
````