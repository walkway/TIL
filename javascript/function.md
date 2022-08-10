### every
- 배열의 모든 요소가 callbackFunction 에서 true를 리턴해야 true
````
arr.every(function(currentValue, index, array), thisValue))
````

### some
- 배열의 요소 중 하나라도 callbackFunction에서 true를 리턴하면 true
````
arr.some(function(currentValue, index, array), thisValue))
````

````
let result = [10, 20, 30, 40].every(element => element % 2 === 0);
console.log(result);

result = [10, 20, 30, 40].some((element, index) => element % 2 === 0);
console.log(result);

result = [10, 20, 30, 40].some((element, index, arrow) => element % 2 === 0);
console.log(result);
````
````
true
true
true
````
