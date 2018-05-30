# lodash

- 일치하는 값 배열
````
_.filter(collection, [predicate=.identity], [thisArg])
````
````
_.filter([4, 5, 6], function(n) {
  return n % 2 == 0;
});
//[4, 6]
````

- 포함 여부
````
_.includes([1, 2, 3], 1);
//true
````
https://lodash.com/