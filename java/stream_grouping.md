# stream grouping

````
static <T,K> Collector<T,?,Map<K,List<T>>> 
  groupingBy(Function<? super T,? extends K> classifier)
````
````
Map<BlogPostType, List<BlogPost>> postsPerType = posts.stream()
  													  .collect(groupingBy(BlogPost::getType));
````
````
List<String> items =Arrays.asList("apple", "apple", "banana", "apple", "orange", "banana", "papaya");

Map<String, Long> result = items.stream()
								.collect(Collectors.groupingBy
											(Function.identity(), Collectors.counting()));
````
````
papaya=1, orange=1, banana=2, apple=3
````
https://www.baeldung.com/java-groupingby-collector
https://www.mkyong.com/java8/java-8-collectors-groupingby-and-mapping-example/