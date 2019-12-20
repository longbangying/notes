# spring 知识体系
## 1.spring bean 的作用范围及生命周期
### spring的bean有五个作用范围spring的bean有五个作用范围
- singleton   在spring IOC 容器中仅存在一个实例，以单例的方式存在，默认值；
- prototype   每次向容器获取Bean的时候（调用getBean()），都会New一个实例；
- request     每一次http请求都会创建一个Bean,仅适用于WebApplicationContext环境；
- session     同一个HttpSession中共享一个Bean,不同的Session用不同的Bean,仅适用于WebApplicationContext环境；
- globalsession 一般用于Protlet应用环境,同样也仅适用于WebApplicationContext环境；
![spring bean的五个作用域](https://img-blog.csdn.net/20160417164310654?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center "spring bean的五个作用域")
### spring bean 的生命周期
- spring 对bean进行实例化，默认是单例；
- spring 对bean进行依赖注入；
- 如果Bean实现了BeanNameAWare接口，则会调用bean的setBeanName(String beanName)方法把bean名称传给bean;
- 如果Bean实现了BeanFactoryAware接口，则会调用bean的setBeanFactory(BeanFactory beanfactory)方法，把BeanFactory实例传给bean；
- 如果Bean实现了ApplicationContextAware接口，则会调用bean的setApplicationContext(ApplicationContext context)方法，把ApplicationContext实例传给bean;
- 如果Bean实现了BeanPostProcessor接口，则会调用beanProcessBeforeInitialization(Object o,String beanName)方法;
- 如果Bean实现了InitializingBean接口，则会调用afterPropertiesSet()方法，类似的，如果Bean使用了init-method属性声明了初始化方法，也会被调用；
- 如果Bean实现了BeanPostProcessor接口，则会调用beanProcessAfterInitialiation(Object o,String beanName)方法；
- 此时Bean已经准备就绪，可以使用了，如果是单例bean,就一直驻留在应用上下文中，直到应用上下文被销毁；
- 如果Bean实现了DisPosableBean接口，在应用上下文被销毁的时候，spring 会调用 destroy()方法，同样的，如果使用destroy-method属性指定了销毁方法，也会被调用；
![spring bean的生命周期](https://img-blog.csdn.net/20160417164808359?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center "spring bean的生命周期")


