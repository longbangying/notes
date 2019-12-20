# spring 
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

## spring 中的事务传播行为
- REQUIRED    默认值，支持当前事务，如果当前没有事务，则新建一个事务；
- SUPPORTS    支持当前事务，如果当前没有事务，则以非事务方式运行；
- MANDATORY    支持当前事务，如果当前没有事务，则抛出异常；
- REQUIRES_NEW    新建事务，如果当前存在事务，就把当前事务挂起；
- PROPAGATION_NOT_SUPPORTED    以非事务方式运行，如果当前存在事务，则挂起；
- PROPAGATION_NEVER    以非事务方式运行，如果当前存在事务，则抛出异常；
- PROPAGATION_NESTED    如果存在事务，则在嵌套事务内运行，如果不存在事务，则执行与REQUIRED 类似的操作；

## spring AOP，IOC实现原理
### AOP
   AOP(面向切面)是一种编程范式，提供从另一个角度考虑程序接口以完善面向对象编程(OOP);AOP能够将那些无业务无关，却为业务模块所共用的逻辑，例如日志管理，事务处理，权限控制等都封装起来，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的可操作性和可维护性；
### AOP的优点：
- 降低模块的耦合度
- 使系统容易拓展
- 提高代码复用性
### AOP基本概念
- 连接点(JoinPoint) 需要在程序中插入横切关注点的点，连接点可能是在类初始化、方法调用，字段调用或异常处理等等，spring只支持方法调用；
- 切入点(PointCut) 一组相关连接点的集合
- 通知(Advice) 在连接点执行的行为，提供了在AOP中需要在切入点所选择的连接点处进行拓展现有行为的手段，包括前置曾倩(before advice)、后置增强(after advice)、环绕增强(around advice);
- 切面(Aspect) 通知和切入点的结合；
- 织入(Weaving) 织入是一个过程，将切面应用到目标对象从而创建出AOP代理对象的过程；
- 代理(Proxy) 通过代理的方式来对目标对象应用切面，AOP代理可以用JDK动态代理(只能代理接口)或Cglib代理实现；
- 目标对象(Target) 需要织入关注点的对象，即要被代理的对象；

### JDK动态代理简单实现
#### 目标对象
```java
public class Dog implements Animal {
    @Override
    public void cry() {
        System.out.println("旺旺旺....");
    }
}
```
#### 代理对象
```java
public class DogProxy implements InvocationHandler {
    private Object targetObject;
    public DogProxy(Object targetObject) {
        this.targetObject = targetObject;
    }
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("前置通知............");
        method.invoke(targetObject,args);
        System.out.println("后置通知............");
        return null;
    }
```
#### 运行
```java
public static void main(String[] args) {
        Dog dog = new Dog();
        Animal animal = (Animal)           Proxy.newProxyInstance(dog.getClass().getClassLoader(),Dog.class.getInterfaces(),new DogProxy(dog));
        animal.cry();
    }
```






