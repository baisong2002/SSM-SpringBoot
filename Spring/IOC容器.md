- 概念：控制反转，把创建对象和对象之间的调用过程交给Spring进行管理，是面向对象的一种，可以设计原则，可以降低代码之间的耦合，最常用的叫依赖注入(ID)

- 底层原理：
     >- xml解析、工厂模式、反射<br /> 
     >- 耦合度逐渐降低<br />
     >- 原始模式---->工厂模式----->IOC模式
![Image text](https://gitee.com/songhe1122/java-framework/raw/master/%E5%9B%BE%E7%89%87/1655805598765-6012cb90-244b-4199-ad0d-fbde6be57123.png)
## IOC接口：<br />
- IOC思想基于IOC容器完成，IOC容器底层就是对象工厂<br /> 
- Spring提供IOCA容器实现的两种方式(两个接口)<br /> 
   >>- (1)BeanFactoy:<br />
     >>>IOC容器基本实现，是Spring内部使用的接口，不提供开发人员使用<br /> 
        加载配置文件时，不会创建对象，在获取(使用)对象才去创建对象<br />     
   >>- (2)ApplicationContext：<br/>
        BeanFactory接口的子接口，提供更强大的功能，一般由开发人员使用<br /> 
        加载配置文件时，会创建配置文件对象<br />
   >>- ApplicationContext实现类：<br />
   >>>- FileSystemXmlApplicationContext ---->盘路径<br /> 
   >>>- ClassPathXmlApplicationContext  ---->类路径

### IOC相关的API
**ApplicationContext继承体系**
![Image text](https://gitee.com/songhe1122/java-framework/raw/master/%E5%9B%BE%E7%89%87/1655348011387-a37229e4-9a26-4a6b-bd3b-90789738b063.png)
**ApplicationContext的实现类**
 - ClassPathXmlApplicationContext   -----> 是从类的根路径下加载配置文件(推荐使用）<br />
 - FileSystemXmlApplicationContext   ---->  是从磁盘路径上加载配置文件，配置文件可以在磁盘任意位置<br />
 
 - AnnotationConfigApplicationContext  ---->  使用注解配置容器对象时，需要使用此类来创建spring容器，用来读注解<br />
**getBean()方法的使用<br />**
  id方式(某一类型的Bean有多个)<br />
  根据类型获取(某一类型的Bean只有一个)<br />
## Bean管理
概念：指的是两个操作：<br />
>> Spring创建对象<br /> 
>> Spring属性注入<br />
### xml方式
基于xml方式创建对象<br />
基于xml方式注入属性<br />
#### xml方式注入
**依赖注入**
>- 概念：是spring框架核心IOC的具体体现<br /> 
>- 在编写程序时，通过控制反转，把对象的创建交给Spring，代码中不可能出现没有依赖的情况，IOC解耦只能降低依赖关系，不会让消除

- 依赖注入的方法：<br /> 
**构造方法**<br />
>- 有参数构造注入属性<br />
```
<bean id="orders" class="com.bai.test.Orders"><br /> 
<constructor-arg name="oname" value="abc"></constructor-arg><br /> 
<constructor-arg name="address" value="中国"></constructor-arg><br /></bean><br />
```
**此时spring调用的是有参构造方法，不是无参构造方法**<br />
**set方法**<br /> 
>- 方式一：<br /> 
```<property name="userDao" ref="userDao"></property>```
    
>- 方式二：<br /> 
>>- 引入p命名空间<br />
```xmlns:p="http://www.springframework.org/schema/p"```
>>- 在注入文件中注入<br />
```<bean id="Service" class="com.bai.impl.UserServiceimpl" p:userDao-ref="userDao"></bean>```
![Image text](https://gitee.com/songhe1122/java-framework/raw/master/%E5%9B%BE%E7%89%87/1655347914646-270c2797-e742-496f-8ff2-f9a1aea7c57c.png)