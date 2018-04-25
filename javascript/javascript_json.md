# javascript json

- json 문자열 변환
````
JSON.stringify(value[, replacer[, space]])
````
````
JSON.stringify({ x: 0 });
````
````
'{"x":0}'
````
- json 문자열을 json object로 변환
````
JSON.parse(text [, reviver])  
````
````
var obj = JSON.parse('{ "name":"John", "age":30, "city":"New York"}');
````
````
<p id="demo"></p> 

<script>
	document.getElementById("demo").innerHTML = obj.name + ", " + obj.age; 
</script>
````
https://www.w3schools.com/js/js_json_parse.asp