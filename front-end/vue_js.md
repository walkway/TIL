# vue js

- 사용자 인터페이스 빌드를 위한 오픈 소스 프로그레시브 자바스크립트 프레임워크
- MVVM(Model-View-ViewModel) 패턴
- Vue.js lets you extend HTML with HTML attributes called directives
- Vue.js directives offers functionality to HTML applications
- Vue.js provides built-in directives and user defined directives
````
<div id="app">
  {{ message }}
</div>
````
````
var app = new Vue({
  el: '#app',
  data: {
    message: '안녕하세요 Vue!'
  }
})
````
- binding
````
<div id="app">
{{ message }}
</div>

<script>
var myObject = new Vue({
    el: '#app',
    data: {message: 'Hello Vue!'}
})

function myFunction() {
    myObject.message = "John Doe";
}
</script>
````
- loop
````
<!DOCTYPE html>
<html>
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<body>

<div id="app">
  <ul>
    <li v-for="x in todos">
      {{ x.text }}
    </li>
  </ul>
</div>

<script>
myObject = new Vue({
  el: '#app',
  data: {
    todos: [
      { text: '1' },
      { text: '2' },
      { text: '3' }
    ]
  }
})
</script>
````
````
1
2
3
````
https://kr.vuejs.org/v2/guide/index.html