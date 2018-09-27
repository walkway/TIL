# @Primary, @Lazy

````
public class BeanClass {
}
````
### bean 등록
````
@Bean
public BeanClass beanClass(){
    System.out.println("bean");
    return new BeanClass();
}
````
````
@Test
public void beanTest(){
    System.out.println("1");
    BeanClass bean = applicationContext.getBean(BeanClass.class);
    System.out.println(bean);
}
// bean 1
````

### Lazy
````
@Bean
@Lazy
public BeanClass beanClass(){
    System.out.println("1");
    return new BeanClass();
}
````
````
@Test
public void lazyTest(){
    System.out.println("1");
    BeanClass bean = applicationContext.getBean(BeanClass.class);
    System.out.println(bean);
}
// 1 bean
// getBean 호출 시, bean 등록
````

### Primary
````
@Bean
@Primary
public BeanClass primaryClass(){
    BeanClass beanClass = new BeanClass();
    System.out.println("primary");
    System.out.println(beanClass);
    return beanClass;
}

@Bean
public BeanClass beanClass(){
    BeanClass beanClass = new BeanClass();
    System.out.println("bean");
    System.out.println(beanClass);
    return beanClass;
}
````
````
@Test
public void primaryTest(){
    BeanClass bean = applicationContext.getBean(BeanClass.class);
    System.out.println(bean);
}
//@Primary bean 먼저 등록
````