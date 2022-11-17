# ConcurrentModificationException 

- List, Map 등 Iterable 객체를 수노히하면서 요소를 삭제하거나 변경할 때 발생
````
List<String> fruits = new ArrayList<>();
fruits.add("apple");
fruits.add("banana");
fruits.add("kiwi");

for (String item : fruits) {
    if (item.equals("apple")) {
        fruits.remove(item);
    }
}
````
- Collection 객체에 element 추가 제거될때, modCount 변수를 수정하는 동작이 추가되어 있다.
- modCount가 조작되는 모든 메서드 (remove, add.. 등)를 반복문에서 사용하여 Collection에 접근하면 Exception이 발생한다.
- 반복문 내부에서 Collection 길이가 변하게 되어 처음 참조한 count 변수와 같지 않기 때문이다.

### Iterator 순회, 요소 삭제
````
for (Iterator<String> iterator = fruits.iterator(); iterator.hasNext();) {
    String item = iterator.next();
    if (item.equals("apple")) {
        iterator.remove();
    }
}
````
