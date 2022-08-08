# Remove Duplicates from an Array

### Set
`````
const duplicationArray = [1, 2, 3, 1, 2];
const set = new Set(duplicationArray);
const uniqueArray = [...set];
document.writeln(Array.isArray(uniqueArray));
document.writeln(uniqueArray);
````

### indexOf(), filter()
- indexOf(): 배열에서 특정 값이 처음으로 나타나는 index 반환
- filter(): 특정 조건에 부합하는 배열의 모든 값을 배열 형태 반환
````
const duplicationArray = [1, 2, 3, 1, 2];
const uniqueArray = duplicationArray.filter((element, index) => {
    return duplicationArray.indexOf(element) === index;
});

document.writeln(Array.isArray(uniqueArray));
document.writeln(uniqueArray);
````

### forEach(), includes()
- forEach(): 배열을 순회하면서, 배열의 원소들로 주어진 callback 함수 실행
- include(): 배열에 특정 값이 포함되는지 여부 검사
````
const duplicationArray = [1, 2, 3, 1, 2];

let uniqueArray = [];
duplicationArray.forEach((element) => {
    if (!uniqueArray.includes(element)) {
        uniqueArray.push(element);
    }
});

document.writeln(Array.isArray(uniqueArray));
document.writeln(uniqueArray);
````
