- 概念：控制反转，把创建对象和对象之间的调用过程交给Spring进行管理，是面向对象的一种，可以设计原则，可以降低代码之间的耦合，最常用的叫依赖注入(ID)

- 底层原理：
     >- xml解析、工厂模式、反射<br /> 
     >- 耦合度逐渐降低<br />
     >- 原始模式---->工厂模式----->IOC模式
![Image text](https://gitee.com/songhe1122/java-framework/raw/master/%E5%9B%BE%E7%89%87/1655805598765-6012cb90-244b-4199-ad0d-fbde6be57123.png)
# IOC接口：<br />
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
>> 基于xml方式创建对象<br />
>> 基于xml方式注入属性<br />
#### xml方式注入
##### 依赖注入
>- 概念：是spring框架核心IOC的具体体现，就是注入属性<br /> 
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
##### xml注入其他数据类型
**Bean依赖注入数据类型**<br />  
- 注入数据的三种类型<br />    
>- 普通数据类型<br />   
>- 引用数据类型<br />   
>- 集合数据类型

**字面量**：
   null值<br />
```
<property name="address" >   
     <null/>
</property> 
``` 
**属性值包含特殊符号**<br />
>- 1、把<>进行转意 &lt;&gt
>- 2、把特殊符号内容写到CDATA

**外部bean**<br /> 
```
<!--外部注入Service和dao对象创建-->
<bean id="service" class="com.bai.Service.UserService">
<!--注入UserDao的对象  
    name属性值，类里面属性名称 
    ref属性，创建UserDao对象bean标签id值 
-->
<property name="userDao" ref="dao"></property>
</bean>
<bean id="dao" class="com.bai.Dao.UserDaoimpl"></bean>
```
**注意：接口和抽象类无法使用<bean>标签进行xml配置**<br />
**内部bean和级联赋值**<br />
- （1）一对多：部门跟员工<br />
```
  <!--内部bean，一对多-->
<bean id="emp" class="com.bai.Emp">
    <!--设置普通属性-->
    <property name="ename" value="lucy"></property>
<property name="gender" value="女"></property>
<!--设置对象类型属性-->
    <property name="dept" >
        <bean id="dept" class="com.bai.Dept">
            <property name="dname" value="安保部"></property>
        </bean>
    </property>
</bean>

<!--级联赋值-->
<bean id="emp" class="com.bai.Emp">
    <property name="ename" value="lucy"></property>
    <property name="gender" value="女"></property>
    <!--级联赋值-->
    <property name="dept" ref="dept"></property>
</bean>
<bean id="dept" class="com.bai.Dept">
    <property name="dname" value="财务部"></property>
</bean>

<!--级联赋值-->(emp中生成dept的get方法)
  <bean id="emp" class="com.bai.Emp">
      <property name="ename" value="lucy"></property>
      <property name="gender" value="女"></property>
    <!--级联赋值-->
      <property name="dept" ref="dept"></property>
 <property name="dept.dnaem" value="技术部"></property>
  </bean>
```
**注入集合**<br />
```
<!--注入集合类型-->
<!--提取list集合属性注入-->
<util:list id="booklist">
    <value>西游记</value>
    <value>水浒传</value>
    <value>三国</value>
</util:list>
<!--提取list集合注入-->
<bean id="book" class="com.bai.gather.Book">
    <property name="list" ref="booklist"></property>
</bean>

<!--创建多个course对象-->
<bean id="cour" class="com.bai.gather.course">
    <property name="cname" value="Spring"></property>
</bean>
<bean id="cour2" class="com.bai.gather.course">
    <property name="cname" value="MyBist"></property>
</bean>
<bean id="array" class="com.bai.gather.array">
    <!--数组类型的注入-->
    <property name="courses">
        <list>
            <value>语文</value>
            <value>数学</value>
        </list>
    </property>
<!--注入list集合类型，值是对象-->
<property name="courseList">
    <list>
        <ref bean="cour"></ref>
        <ref bean="cour2"></ref>
    </list>
</property>
    <!--map集合类型的注入-->
    <property name="map">
        <map>
            <entry key="JAVA" value="java"></entry>
            <entry key="PHP" value="php"></entry>
        </map>
    </property>
    <!--set类型注入-->
    <property name="sets">
        <set>
            <value>MySQL</value>
            <value>Resid</value>
        </set>
    </property>
</bean>
```
##### FacoryBean
Spring里有两种bean，一种是普通的bean，另一种是工厂bean(FacoryBean)<br />
**普通bean：**<br />
- 在spring配置文件中，定义的类型就是返回的类型<br />
**工厂bean：**<br />
- 在配置文件定义bean类型可以和返回类型不一样<br />
```
public class bean implements FactoryBean<course> {
    //定义返回bean的对象
    @Override
    public course getObject() throws Exception {
        course course = new course();
        course.setCname("a,b,c");
        return course;
    }
    @Override
    public Class<?> getObjectType() {
        return null;
    }
    @Override
    public boolean isSingleton() {
        return false;
    }
}
```
#### xml创建对象
##### 配置文件
- 1、Bean标签的基本配置<br />   
- 用于配置对象交由spring来创建<br />   
- 默认情况下调用的是类的无参构造函数，如果没有无参构造函数则不能创建成功<br />
- 基本属性：<br />   
- id:Bean实例在spring容器中的唯一构造表示<br />   
- class：Bean的全限定名称<br />
![Image text](https://gitee.com/songhe1122/java-framework/raw/master/%E5%9B%BE%E7%89%87/1655807109545-94b796c2-999b-417f-8845-145f4abdf0cb.png)

**Bean标签范围配置**<br />
     scop:指对象的作用范围。
***取值范围*** <br />           
     singleton   默认值，单例的 (地址值一样,对象只有一个，加载配置文件时创建Bean) <br /> 
实例化时机：当Spring核心文件被加载时，实例化配置的Bean实例<br />             
**Bean的生命周期**<br />
- 创建：当应用加载，创建容器时，对象被创建<br />
- 运行：只要容器在，对象就一直或者<br />
- 销毁：当应用卸载，销毁容器时，对象被销毁<br />
prototype  多例的  (地址值不一样，对象由多个，调用getBean()时实例化Bean)<br />              
- **Bean的生命周期**<br />     
创建：当使用对象是，创建新的对象实例<br />  
运行：只要对象在使用中，就一直活着<br />   
销毁：当对象长期不使用，就被java的垃圾回收器回收了<br />
**singletion和prototype的区别：**<br />
singletion是单例的，prototype是多实例<br /> 
设置scope值是singletion时，加载Spring配置文件时就会创建单实例对象<br />
设置scope值是prototype时，不是在加载Spring配置文件时创建，在调用getBean()时创建<br /> 
-------------------------------**基本不用**------------------------------------------------------<br />  
request    Web项目中，spring创建一个Bean对象，将对象存入request域中<br />
session    Web项目中，spring创建一个Bean对象，将对象存入session域中<br />
global session  Web项目，应用在Portlet环境，如果没有该环境，相当于session<br />

**生命周期**
通过构造器创建bean实例(无参构造)<br />   
为bean的属性设置值和对其他bean引用(调用set方法）<br />  
init-method:指定类中的初始化方法名称<br />   
bean可以使用了(对象获取到了)<br />     
destroy-method:指定类中销毁方法名称<br />
**bean的后置处理器，bean的生命周期有七步**<br /> 
通过构造器创建bean实例(无参构造)<br />    
为bean的属性设置值和对其他bean引用(调用set方法）<br />    
把bean的实例传递给bean后置处理器(postProcessBeforeInitialization)<br />  
init-method:指定类中的初始化方法名称<br />    
把bean的实例传递给bean后置处理器(postProcessAfterInitialization)<br /> 
bean可以使用了(对象获取到了)<br />     
destroy-method:指定类中销毁方法名称<br />
**bean自动装配**<br />
- 概念：根据指定的装配规则(属性名称或属性类型)，Spring自动将匹配的属性值进行注入<br />
```
<!--自动装配
  bean标签属性autowire,配置自动装配
  autowire属性常用两个值：
              byName 根据属性名称进行注入，注入值bean得id值要和属性名称一致
              byType 根据属性类型进行注入,相同类型的bean只能有一个(单例)
-->
<bean id="emp" class="com.bai.autowire.Emp" autowire="byType">
 <!--手动装配-->
 <!--<property name="dept" ref="dept"></property>-->
</bean>
<bean id="dept" class="com.bai.autowire.Dept"></bean>
```
**Bean实例化三种方式**
- 无参构造方法实例化<br />
- 工厂静态方法实例化<br />  
```
<bean id="userDao" class="com.bai.factory.StaticFactory" factory-method="getUserDao"></bean>
```
- 工厂实例方法实例化
``` 
<!--获取工厂的对象-->
           <bean id="factory" class="com.bai.factory.DynamicFactory" ></bean>
        <!--通过工厂获取Bean的对象-->
<bean id="userDao" factory-bean="factory" factory-method="getUserDao"></bean>
```
##### 引入其他配置文件(分模块开发)
- spring配置内容非常多，可以将部分配置拆解到其他配置文件，spring主配置文件通过import标签进行加载
![Image text](https://gitee.com/songhe1122/java-framework/raw/master/%E5%9B%BE%E7%89%87/1655807109545-94b796c2-999b-417f-8845-145f4abdf0cb.png)
```
<import resource="applicationContext-user.xml"/>
<!--配置连接池-->
<bean id="durid" class="com.alibaba.druid.pool.DruidDataSource">
    <property name="driverClassName" value="${driverClassName}"></property>
    <property name="url" value="${url}"></property>
    <property name="username" value="${userName}"></property>
    <property name="password" value="${password}"></property>
</bean>
<!--引入外部属性文件-->
<context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>
```