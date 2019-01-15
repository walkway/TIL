# stream max, min

- max
````
Integer maxNumber = Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9)
                    .max(Comparator.comparing(Integer::valueOf))
                    .get();
````

- min
````
Integer minNumber = Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9)
                    .min(Comparator.comparing(Integer::valueOf))
                    .get();
````
````
class Person {
    String name;
    Integer age;
}
````
````
Person alex = new Person("Alex", 23);
Person john = new Person("John", 40);
Person peter = new Person("Peter", 32);
List<Person> people = Arrays.asList(alex, john, peter);

Person minByAge = people
  .stream()
  .min(Comparator.comparing(Person::getAge))
  .orElseThrow(NoSuchElementException::new);

assertEquals("Alex", alex, minByAge);
````