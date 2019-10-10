![](F:\opt\githubproject\powerttt\daily-summary\2019年10月10日\Bean生命周期.png)

```java
public class User implements InitializingBean, DisposableBean {

    private String name;
    public User() {
        System.out.println("调用Bean的函数(constructor)");
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        System.out.println("调用Bean的函数(setName/setAttribute)");
        this.name = name;
    }

    /**
     * 构造后
     */
    @PostConstruct
    public void postConstruct(){
        System.out.println("构造后");
    }

    /**
     * MainConfig中的@Bean 的 initMethod
     */
    public void initMethod(){
        System.out.println("调用Bean的函数（initMethod）");
    }
    /**
     * 属性设置后
     * Initializing接口方法中的afterPropertiesSet
     */
    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("调用Bean的函数(afterPropertiesSet)  属性设置后");
    }

    /**
     * 销毁前
     */
    @PreDestroy
    public void preDestroy(){
        System.out.println("销毁前 some thing");
    }

    @Override
    public void destroy() throws Exception {
        System.out.println("销毁  some thing");

    }
    /**
     * MainConfig中的@Bean 的 initMethod
     */
    public void destroyMethod(){
        System.out.println("调用Bean的函数(destroyMethod)");
    }

}
```

```java
@Component
public class CustomBeanPostProcessor implements BeanPostProcessor {
    /**
     * 将此BeanPostProcessor应用于给定的新bean实例<i>之前</ i>任何bean
     * @param bean
     * @param beanName
     * @return
     * @throws BeansException
     */
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        if (bean.getClass()==User.class){
            System.out.println("调用postProcessBeforeInitialization...");
        }
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        if (bean.getClass()==User.class){
            System.out.println("调用postProcessAfterInitialization...");
        }
        return bean;
    }
}
```

```java
@SpringBootApplication
public class MainConfig {

    public static void main(String[] args) {
        SpringApplication.run(MainConfig.class);
    }

    @Bean
    public User user(){
        return new User();
    }

}
```

