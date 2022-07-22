# BeanPostProcessor

- BeanPostProcessor 구현한 bean은 다른 bean보다 먼저 생성
- bean 생성 -> BeanPostProcessor 처리
- BeanFactoryPostProcessor -> postProcessBeanFactory 메소드 -> BeanPostProcessor
- BeanPostProcessor 
````
Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException;
Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException;
````
- postProcessBeforeInitialization: Initialization 전에 처리
- postProcessAfterInitialization: Initialization 후에 처리
````
@Component
public class TestBean implements InitializingBean {
  private String name;

  @Override
  public void afterPropertiesSet() throws Exception {
    System.out.println(name);
  }

  public void setName(String name) {
    this.name = name;
  }

  public String getName() {
    return name;
  }
}
````
````
@Configuration
public class BeanPostProcessorTest implements BeanPostProcessor {

  @Override
  public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
    if(bean instanceof TestBean){
      TestBean bean = (TestBean) bean;
      bean.setName("test");
    }
    return bean;
  }

  @Override
  public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
    return bean;
  }
}
````
- postProcessBeforeInitialization -> afterPropertiesSet() -> postProcessAfterInitialization

### InitializingBean interface
- afterPropertiesSet
- spring framework에서 제공하는 초기화 메소드
- 객체를 생성 -> 프로퍼티 초기화 -> 컨테이너 관련 설정을 완료 -> 호출

### DependsOn
- bean 등록되는 순서 @DependsOn을 통해 지정
- TestBeanClass1 >TestBeanClass2 > TestBeanClass3
````
@Component("TestBeanClass1")
  public class TestBeanClass1 {

}
@Component("TestBeanClass2")
@DependsOn(value = "TestBeanClass1")
public class TestBeanClass2 {

}

@Component("TestBeanClass3")
@DependsOn(value = "TestBeanClass2")
public class TestBeanClass3 {

}
````
