# IOC

IOC：控制反转，将对象交给容器来进行管理。

DI：依赖注入，将对应属性注入到具体对象中，使用@Autowired，@Resource，populateBean方法完成属性注入。

容器：存储管理对象，使用map结构存储对象，spring中存储对象一般使用三级缓存，singletonObjects存放完整对象，earlySingletonObjects存放半成品对象，singleton存放lambda表达式和对象名称的映射，整个bean的生命周期，从创建到销毁都是由容器控制。



spring中所有的bean都是通过反射生成的，比如constructor，newInstance,整个过程有很多扩展的点，比如有两个非常重要的接口，BeanFactoryPostProcessor,BeanPostProcessor,用来实现扩展功能，其中AOP就是在IOC基础上的扩展实现，是用BeanPostProcessor实现的，IOC中除了创建对象之外，还有一个重点就是填充属性，



# AOP



# Spring Bean生命周期

**背诵生命周期流程图**

bean的生命周期主要包括实例化和初始化两个重要环节，此外还有一些扩展的环节。

1.实例化bean对象，通过反射生成，通过createBeanInstance方法专门生成对象。

2.设置属性，当bean对象创建完成之后，对象的属性都是默认值，通过populateBean方法来完成对象属性填充，注意循环依赖的问题（使用三级缓存解决循环依赖的问题）。

3.向bean对象中设置容器属性，调用invokeAwareMethods方法将容器对象设置到具体的bean对象中。

4.调用BeanPostProcessor中的前置处理方法来进行bean扩展工作，比如ApplicationContextPostProcessor，EmbeddValueResolver等对象。

5.调用invokeInitMethods方法完成初始化方法的调用，在这个方法执行的过程中，需要判断当前的bean对象是否实现了InitializingBean接口，如果实现了则调用afterPropertiesSet方法来最后设置bean对象

6.调用BeanPostProcessor中的后置处理方法来完成bean的后置处理，主要实现的接口名字为AbstractAutoProxyCreator。

7.获取到完整的对象之后，通过getBean方式获取和使用对象。

8.当对象使用完成之后，会销毁对象，主要是判断是否实现了DispoableBean接口，然后去调用destroyMethod方法。



# BeanFactory和FactoryBean区别

BeanFactory和FactoryBean都可以创建对象，不同就是创建的流程和方式不一样

使用BeanFactory时，必须遵守bean的生命周期，相当于流水线式得到创建对象。

而FactoryBean是用户可以自定义创建bean对象的创建流程，不需要按照bean生命周期来创建，在此接口中包含了三个方法，分别是

isSingleton：判断是否单例对象

getObjectType：获取对象的类型

getObject：在此方法中可以自己创建对象，使用new的方式或者使用代理的方式都可以，用户可以按照自己的需要随意创建对象，在很多其他框架中都使用了FactoryBean接口，比如Feign



# ApplicationContext和BeanFactory的区别

BeanFactory是访问Spring容器的根接口，里面只提供了某些基本方法和约束规范，为了满足更多的需求，ApplicationContext实现了此接口，并在此接口基础之上做了一些扩展功能，提供更多的api调用，一般使用ApplicationContext较多



# Spring中设计模式

单例模式：Spring中的bean都是单例的

工厂模式：BeanFactory

模板方法：postProcessorBeanFactory，onRefresh

观察者模式：listener，event，multicast

适配器模式：Adapter

装饰者模式：BeanWrapper

责任链模式：使用aop模式的时候会有一个责任链模式

代理模式：aop动态代理

委托者模式：delegate

建造者模式：builder

策略模式：XmlBeanDefinitionReader,PropertiesBeanDefinitionReader



# 循环依赖



# 事务

