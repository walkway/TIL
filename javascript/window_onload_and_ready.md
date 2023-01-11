# window onload, ready

### window.onload
- 문서의 모든 콘텐츠(images, script, css, etc)가 로드된 후 발생하는 이벤트
- DOM standard 이벤트, html의 로딩이 끝난 후에 시작
- 화면에 필요한 모든 요소들이 웹 브라우저 메모리에 올려진 다음 실행
- 동일한 문서에 onload는 하나만 존재

### $(document).ready()
- DOM이 로드된 시점에 호출
- 외부 리소스 및 이미지 로딩과 상관없이 DOM만 로드되면 실행
- 중복 사용해도 순서대로 모두 실행

### example
````
The ready event occurs after the HTML document has been loaded, while the onload event occurs later, when all content (e.g. images) also has been loaded.
The onload event is a standard event in the DOM, while the ready event is specific to jQuery. The purpose of the ready event is that it should occur as early as possible after the document has loaded, so that code that adds functionality to the elements in the page doesn't have to wait for all content to load.
````

````
<script type="text/javascript">
window.onload = function() {
	console.log("window onload 1");
}
window.onload = function() {
	console.log("window onload 2");
} 
$(document).ready(function() {
	console.log("document ready 1")
	});
$(document).ready(function() { 
	console.log("document ready 2")
	});
</script>
````
````
document ready 1
document ready 2
window onload 1
````
