<权慑天下>（qstx520@163.com）
密级：普通






Spring
V1.0







文 档 编 号		保 密 等 级	
作       者	周权威	最后修改日期	
审  核   人		最后审批日期	
批  准   人		最后批准日期	
							
							
							
							
 
修订历史记录

日期	版本	说明	作者
2018-08-06	V1.0	新建	周权威

 
目录
1	概述	4
1.1	特点	4
1.1.1	轻量级	4
1.1.2	依赖注入	4
1.1.3	面向切面编程	4
1.1.4	容器	4
1.1.5	框架（基盘）	4
1.1.6	一站式	4
1.2	产品	4
2	开发流程	5
2.1	导入JAR包	5
2.2	配置文件	5
2.3	获取容器对象	5
2.4	获取JAVEBEAN	5
3	IOC&DI概述	6
3.1	IOC	6
3.1.1	ApplicationContext	6
3.1.2	获取Bean	6
3.2	DI	7
3.3	配置BEAN	7
4	依赖注入的方式	7
4.1	属性注入	7
4.1.1	Set方法	7
4.1.2	IOC配置文件	8
4.2	构造器注入	9
4.2.1	按照顺序进行注入	9
4.2.2	按照类型进行注入	10
4.3	工厂方法注入	10
4.4	属性注入的细节	10
4.4.1	字面值	10
4.4.2	引用其他Bean	11
4.4.3	NULL值和级联属性	14
5	集合属性	15
5.1	LIST	15
5.1.1	普通集合元素	15
5.1.2	String集合元素	22
5.2	MAP	23
5.3	使用UTILITY SCHEME定义集合	25
5.4	使用P命名空间	26
6	自动装配	27
6.1	BYTYPE	27
6.2	BYNAME	27
6.3	CONSTRUCTOR	28
6.4	缺点	28
7	BEAN之间的关系	28
7.1	继承	28
7.2	依赖	29
8	BEAN的作用域	29
9	使用外部属性文件	30
9.1	创建属性文件	30
9.2	引用外部属性文件	30
9.3	告知IOC容器引用的属性文件	31
10	SPEL	31
10.1	通过BEAN的ID对BEAN进行引用	31
10.2	调用方法以及引用对象中的属性	32
10.3	计算表达式的值	32


1	概述
Spring 是一个开源框架。
Spring为简化企业级应用开发而生，使用Spring可以使简单的JavaBean实现以前只有EJB才能实现的功能。
Spring是一个IOC（DI）和AOP容器框架。
1.1	特点
1.1.1	轻量级
Spring是非侵入性的-基于Spring开发的应用中的对象可以不依赖于Spring的API。
1.1.2	依赖注入
DI，dependency injection、IOC。
1.1.3	面向切面编程
AOP，aspect oriented programming。
1.1.4	容器
Spring是一个容器，因为它包含并管理应用对象的生命周期。
1.1.5	框架（基盘）
Spring实现了使用简单的组建配置组合成一个复杂的应用。在Spring中可以使用XML和Java注解组合这些对象。
1.1.6	一站式
在IOC和AOP的基础上可以整合各种企业应用的开源框架和优秀的第三方类库（实际上Spring自身也提供了展现层的Spring MVC和持久层的Spring JDBC）。
1.2	产品
 
2	开发流程
2.1	导入jar包
把以下jar包导入到工程的classpath下：
 
2.2	配置文件
创建spring容器的配置文件，所有的javabean都需要在spring容器的配置文件中体现。
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
									xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
						<bean id="person1" class="com.spring.beans.Person">
									<property name="name" value="张三"></property>
									<property name="age" value="12"></property>
						</bean>
</beans> 
其中com.spring.beans.Person为Java类，person1为javabean，name、age为类属性。
2.3	获取容器对象
ApplicationContext ac = new ClassPathXmlApplicationContext("spring.xml");
其中spring.xml为xml文件。
2.4	获取JaveBean
Person person1 = (Person) ac.getBean("person1");
其中PersonJava类，person1为javabean实体。
3	IOC&DI概述
3.1	IOC
IOC，Inversion of Control，其思想是反转资源获取的方向，传统的资源查找方式要求组件向容器发起请求查找资源。作为回应，容器适时的返回资源。
Person p = new Person();
而应用了IOC之后，则是容器主动将资源推送给它所管理的组件，组件所要做的仅是选择一种合适的方式来接受资源，这种行为也称为查找的被动形式。
Person p = ac.getBean(“a”);
在Spring IOC容器读取Bean配置、创建Bean实例之前，必须对它进行实例化。只有在容器实例化后，才可以从IOC容器里获取Bean实例并使用。如果想获取对象，必须要有一个IOC容器。
Spring提供了两种类型的IOC容器实现。
BeanFactory:IOC容器的基本实现。
ApplicationContext:提供了更多的高级特性，是BeanFactory的子接口。
BeanFactory是Spring框架的基础设施，面向Spring本身。
ApplicationContext面向使用Spring框架的开发者，几乎所有的应用场合都直接使用ApplicationContext而非底层的BeanFactory。
无论使用何种方式，配置文件是相同的。
3.1.1	ApplicationContext
 ConfigurableApplicationContext扩展于ApplicationContext，新增加两个主要方法：refresh()和close()，让ApplicationContext具有启动、刷新和关闭上下文的能力。
ApplicationContext的主要实现类：
ClassPathXmlApplicationContext：从类路径下加载配置文件；
FileSystemXmlApplicationContext:从文件系统中加载配置文件。
ApplicationContext在初始化上下文时就实例化所有单例的Bean。
WebApplicationContext是专门为WEB应用而准备的，它允许从相对于WEB根目录的路径中完成初始化工作。
3.1.2	获取Bean
 调用ApplicationContext的getBean()方法。
 
3.2	DI
DI，Dependency Injection，IOC的另一种表述方式：即组件以一些预先定义好的方式（例如：setter方法）接受来自如容器的资源注入，相对于IOC而言，这种表述更直接。
3.3	配置bean
配置形式：基于XML文件的方式、基于注解的方式。
Bean配置方式：通过全类名（反射）、通过工厂方法（静态工厂方法&实例工厂方法）、FactoryBean。
<!-- 通过全类名的方式来配置bean-->
<bean class="com.spring.beans.Dog">
    <property name="type" value="哈士奇"></property>
</bean>
Id:Bean的名称。
在id没有指定，Spring自动将全路径类名作为Bean的名字，id可以指定多个名字，名字之间可用逗号、分号、或空格分隔。
4	依赖注入的方式
4.1	属性注入
4.1.1	Set方法
Set方法实现。
定义Set方法：
	public void setType(String type) {
    		this.type = type;
       }

xml配置信息：
<bean id="dog1" class="com.spring.beans.Dog">
</bean>

主方法调用：
ApplicationContext ac = new ClassPathXmlApplicationContext("spring.xml");
/**
 * 获取bean的三种常用方法
 * 1、Object getBean(String) String:id的值
 * 2、getBean(Class)
 * 3、getBean(String,Class)
 */
Dog dog1 = (Dog) ac.getBean("dog1");
dog1.setType("哈士奇");
System.out.println(dog1);
4.1.2	IOC配置文件
4.1.2.1	Property节点
IOC的配置文件当中，通过
<property name=”属性名称” value=”属性的值”></property>节点完成注入。
XML配置信息：
<bean id="dog1" class="com.spring.beans.Dog">
         <property name="type" value="哈士奇"></property>
       </bean>
主方法调用：
ApplicationContext ac = new ClassPathXmlApplicationContext("spring.xml");
Dog dog1 = (Dog) ac.getBean("dog1");
System.out.println(dog1);
4.1.2.2	Property子节点
<property name=”属性名称”>
		<value></value>
<property>
XML配置信息：
<bean id="dog1" class="com.spring.beans.Dog">
          <property name="type">
              <value>哈士奇</value>
          </property>
       </bean>
主方法调用：
ApplicationContext ac = new ClassPathXmlApplicationContext("spring.xml");
Dog dog1 = (Dog) ac.getBean("dog1");
System.out.println(dog1);
4.2	构造器注入
4.2.1	按照顺序进行注入
构造方法如下:
public Car(String type, String factory, double price) {
            this.type = type;
            this.factory = factory;
            this.price = price;
       }
XML配置文件如下：
<bean id="car1" class="com.spring.beans.Car">
    <!--按照顺序赋值参数-->
    <constructor-arg value="宝马"></constructor-arg>
    <constructor-arg value="长春一汽"></constructor-arg>
    <constructor-arg value="300000"></constructor-arg>
</bean>
<bean id="car2" class="com.spring.beans.Car">
    <!--按照顺序赋值参数-->
    <constructor-arg value="奥迪" index="0"></constructor-arg>
    <constructor-arg value="长春一汽" index="1"></constructor-arg>
    <constructor-arg value="800000" index="2"></constructor-arg>
</bean>
主方法调用：
ApplicationContext ac = new ClassPathXmlApplicationContext("spring.xml");
Car car1 = (Car) ac.getBean("car1");
System.out.println(car1);

Car car2 = (Car) ac.getBean("car2");
System.out.println(car2);
4.2.2	按照类型进行注入
构造方法如下:
public Car(String type, String factory, double price) {
            this.type = type;
            this.factory = factory;
            this.price = price;
       }
XML配置文件如下：
<bean id="car3" class="com.spring.beans.Car">
    <!--按照顺序赋值参数-->
    <constructor-arg value="马自达" type="java.lang.String"></constructor-arg>
    <constructor-arg value="长安马自达" type="java.lang.String"></constructor-arg>
    <constructor-arg value="500000" type="double"></constructor-arg>
</bean>
主方法调用：
ApplicationContext ac = new ClassPathXmlApplicationContext("spring.xml");
Car car3 = (Car) ac.getBean("car3");
System.out.println(car3);
4.3	工厂方法注入
4.4	属性注入的细节
4.4.1	字面值
字面值：可用字符串表示的值，可以通过<value>元素标签或value属性进行注入。
基本数据类型及其封装类、String等类型可以采取字面值注入的方式。
若字面值中包含特殊字符，可以使用<![CDATA[]]>把字面值包裹起来。不能通过value属性方式进行注入
XML配置信息：
<bean id="car4" class="com.spring.beans.Car">
    <property name="type">
        <value><![CDATA[<奥迪>]]></value>
    </property>
    <property name="factory" value="长春一汽"></property>
    <property name="price" value="800000"></property>
</bean>
 			     主方法调用：
ApplicationContext ac = new ClassPathXmlApplicationContext("spring.xml");
Car car4 = (Car) ac.getBean("car4");
System.out.println(car4);
4.4.2	引用其他Bean
组成应用程序的Bean经常需要相互协作以完成应用程序的功能，要使Bean能够相互访问，就必须在Bean配置文件中指定Bean的引用。
在Bean的配置文件中，可以通过<ref>元素或ref属性为Bean的属性或构造器参数指定对Bean的引用。也可以在属性或构造器里包含Bean的声明，这样的Bean成为内部Bean。
4.4.2.1	ref元素
XML配置信息：
<bean id="person1" class="com.spring.beans.Person">
    <property name="name" value="权慑天下"></property>
    <property name="age" value="26"></property>
    <property name="car" ref="car1"></property>
</bean>
 主方法调用：
ApplicationContext ac = new ClassPathXmlApplicationContext("spring.xml");
Person person1 = (Person) ac.getBean("person1");
System.out.println(person1);
4.4.2.2	ref属性
XML配置信息：
<bean id="person2" class="com.spring.beans.Person">
    <property name="name" value="权慑天下"></property>
    <property name="age" value="26"></property>
    <property name="car">
        <ref bean="car2"></ref>
    </property>
</bean>
 主方法调用：
ApplicationContext ac = new ClassPathXmlApplicationContext("spring.xml");
Person person2 = (Person) ac.getBean("person2");
System.out.println(person2);
4.4.2.3	构造器注入
 构造方法：
public Person(String name, int age, Car car) {
    this.name = name;
    this.age = age;
    this.car = car;
}
XML配置文件信息：
<bean id="person3" class="com.spring.beans.Person">
    <constructor-arg name="name" value="张三"></constructor-arg>
    <constructor-arg name="age" value="26"></constructor-arg>
    <constructor-arg name="car" ref="car1"></constructor-arg>
</bean>

<bean id="person4" class="com.spring.beans.Person">
    <constructor-arg name="name" value="王五"></constructor-arg>
    <constructor-arg name="age" value="20"></constructor-arg>
    <constructor-arg name="car">
        <ref bean="car2"></ref>
    </constructor-arg>
</bean>
主方法调用：
ApplicationContext ac = new ClassPathXmlApplicationContext("spring.xml");
Person person3 = (Person) ac.getBean("person3");
System.out.println(person3);

Person person4 = (Person) ac.getBean("person4");
System.out.println(person4);
4.4.2.4	内部Bean
当Bean实例仅仅给一个特定的属性使用时，可以将其声明为内部Bean，内部Bean声明直接包含在<property>或<constructor-arg>元素里，不需要设置任何Id或name属性，内部Bean不能使用在任何其他地方。
构造方法：
public Person(String name, int age, Car car) {
    this.name = name;
    this.age = age;
    this.car = car;
}
XML配置文件信息：
<bean id="person5" class="com.spring.beans.Person">
    <property name="name" value="李四"></property>
    <property name="age" value="22"></property>
    <property name="car">
        <bean class="com.spring.beans.Car">
            <property name="type" value="比亚迪"></property>
            <property name="factory" value="长春一汽"></property>
            <property name="price" value="100000"></property>
        </bean>
    </property>
</bean>
主方法调用：
ApplicationContext ac = new ClassPathXmlApplicationContext("spring.xml");
Person person5 = (Person) ac.getBean("person5");
System.out.println(person5);
4.4.3	NULL值和级联属性
4.4.3.1	NULL值
可以使用专用的<null/>元素标签为Bean的字符串或其他对象类型的属性注入null值。
XML配置信息：
<bean id="person8" class="com.spring.beans.Person">
    <property name="name" value="李四"></property>
    <property name="age" value="22"></property>
    <property name="car">
        <!--显示的声明所引用的是一个空值-->
        <null></null>
    </property>
</bean>
主方法调用：
ApplicationContext ac = new ClassPathXmlApplicationContext("spring.xml");
Person person8 = (Person) ac.getBean("person8");
System.out.println(person8);
4.4.3.2	级联属性
和Struts、Hibernate等框架一样，Spring支持级联属性的配置。
XML配置文件：
<bean id="dog2" class="com.spring.beans.Dog">
</bean>
<bean id="person9" class="com.spring.beans.Person">
    <property name="name" value="李四"></property>
    <property name="age" value="22"></property>
    <property name="dog" ref="dog2"></property>
    <property name="dog.type" value="小白"></property>
</bean>
主方法调用：
ApplicationContext ac = new ClassPathXmlApplicationContext("spring.xml");
Person person9 = (Person) ac.getBean("person9");
System.out.println(person9);
5	集合属性
在Spring中可以通过一组内置的xml标签（例如：<list>、<set>或<map>）来配置集合属性。
5.1	List
配置java.util.List类型的属性，需要指定<list>标签，在标签里包含一些元素，这些标签可以通过<value>指定简单的常量值，通过<ref>指定对其他Bean的引用。通过<bean>指定内置Bean定义，通过<null/>指定空元素，甚至可以内嵌其他集合。
数组的定义和List一样，都使用<list>。
配置java.util.set需要使用<set>标签，定义元素的方法与List一样。
5.1.1	普通集合元素
XML配置信息：
Person类：
package com.spring.beans;
import java.util.List;
public class Person {
    private String name;
    private int age;
    private List<Car> car;
    public Person(){}
    public Person(String name, int age, List<Car> car) {
        this.name = name;
        this.age = age;
        this.car = car;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public List<Car> getCar() {
        return car;
    }
    public void setCar(List<Car> car) {
        this.car = car;
    }
    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", car=" + car +
                '}';
    }
}
XML配置：
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="person1" class="com.spring.beans.Person">
        <property name="name" value="权慑天下"></property>
        <property name="age" value="26"></property>
        <property name="car" ref="car1"></property>
    </bean>
    <bean id="person2" class="com.spring.beans.Person">
        <property name="name" value="权慑天下"></property>
        <property name="age" value="26"></property>
        <property name="car">
            <ref bean="car2"></ref>
        </property>
    </bean>
    <bean id="person3" class="com.spring.beans.Person">
        <constructor-arg name="name" value="张三"></constructor-arg>
        <constructor-arg name="age" value="26"></constructor-arg>
        <constructor-arg name="car" ref="car1"></constructor-arg>
    </bean>
    <bean id="person4" class="com.spring.beans.Person">
        <constructor-arg name="name" value="王五"></constructor-arg>
        <constructor-arg name="age" value="20"></constructor-arg>
        <constructor-arg name="car">
            <ref bean="car2"></ref>
        </constructor-arg>
    </bean>
    <bean id="person5" class="com.spring.beans.Person">
        <property name="name" value="李四"></property>
        <property name="age" value="22"></property>
        <property name="car">
            <bean class="com.spring.beans.Car">
                <property name="type" value="比亚迪"></property>
                <property name="factory" value="长春一汽"></property>
                <property name="price" value="100000"></property>
            </bean>
        </property>
    </bean>
    <bean id="person6" class="com.spring.beans.Person">
        <property name="name" value="小朱"></property>
        <property name="age" value="20"></property>
        <property name="car">
            <list>
                <bean class="com.spring.beans.Car">
                    <property name="type" value="宝马"></property>
                    <property name="factory" value="长春一汽"></property>
                    <property name="price" value="200000"></property>
                </bean>
                <bean class="com.spring.beans.Car">
                    <property name="type" value="保时捷"></property>
                    <property name="factory" value="长春一汽"></property>
                    <property name="price" value="100000"></property>
                </bean>
            </list>
        </property>
    </bean>

    <bean id="person7" class="com.spring.beans.Person">
        <property name="name" value="小朱"></property>
        <property name="age" value="20"></property>
        <property name="car">
            <list>
                <ref bean="car1"></ref>
                <ref bean="car2"></ref>
                <ref bean="car3"></ref>
                <ref bean="car4"></ref>
            </list>
        </property>
    </bean>

    <!-- 通过全类名的方式来配置bean-->
    <bean id="dog1" class="com.spring.beans.Dog">
        <constructor-arg value="藏獒"></constructor-arg>
    </bean>

    <bean id="car1" class="com.spring.beans.Car">
        <!--按照顺序赋值参数-->
        <constructor-arg value="宝马"></constructor-arg>
        <constructor-arg value="长春一汽"></constructor-arg>
        <constructor-arg value="300000"></constructor-arg>
    </bean>
    <bean id="car2" class="com.spring.beans.Car">
        <!--按照顺序赋值参数-->
        <constructor-arg value="奥迪" index="0"></constructor-arg>
        <constructor-arg value="长春一汽" index="1"></constructor-arg>
        <constructor-arg value="800000" index="2"></constructor-arg>
    </bean>
    <bean id="car3" class="com.spring.beans.Car">
        <!--按照顺序赋值参数-->
        <constructor-arg value="马自达" type="java.lang.String"></constructor-arg>
        <constructor-arg value="长安马自达" type="java.lang.String"></constructor-arg>
        <constructor-arg value="500000" type="double"></constructor-arg>
    </bean>
    <bean id="car4" class="com.spring.beans.Car">
        <property name="type">
            <value><![CDATA[<奥迪>]]></value>
        </property>
        <property name="factory" value="长春一汽"></property>
        <property name="price" value="800000"></property>
    </bean>
</beans>
主方法调用：
package com.spring.beans;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Main {
    public static void main(String[] args) {
        ApplicationContext ac = new ClassPathXmlApplicationContext("spring.xml");

        System.out.println("****************************1****************************\n");
        Person person1 = (Person) ac.getBean("person1");
        System.out.println(person1);

        System.out.println("****************************2****************************\n");
        Person person2 = (Person) ac.getBean("person2");
        System.out.println(person2);

        System.out.println("****************************3****************************\n");
        Person person3 = (Person) ac.getBean("person3");
        System.out.println(person3);

        System.out.println("****************************4****************************\n");
        Person person4 = (Person) ac.getBean("person4");
        System.out.println(person4);

        System.out.println("****************************5****************************\n");
        Person person5 = (Person) ac.getBean("person5");
        System.out.println(person5);

        System.out.println("****************************6****************************\n");
        Person person6 = (Person) ac.getBean("person6");
        System.out.println(person6);

        System.out.println("****************************7****************************\n");
        Person person7 = (Person) ac.getBean("person7");
        System.out.println(person7);
    }
}
5.1.2	String集合元素
Cat类：
package com.spring.beans;

import java.util.List;

public class Cat {
    private List<String> name;
    public Cat() {
    }
    public Cat(List<String> name) {
        this.name = name;
    }
    public List<String> getName() {
        return name;
    }

    public void setName(List<String> name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Cat{" +
                "name=" + name +
                '}';
    }
}
XML配置信息：
<bean name="cat1" class="com.spring.beans.Cat">
    <property name="name">
        <list>
            <value>小花</value>
            <value>小黄</value>
        </list>
    </property>
</bean>
主方法调用：
package com.spring.beans;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Main {
    public static void main(String[] args) {
        ApplicationContext ac = new ClassPathXmlApplicationContext("spring.xml");
        System.out.println("****************************10****************************\n");
        Cat cat1 = (Cat) ac.getBean("cat1");
        System.out.println(cat1);
    }
}
5.2	Map
配置java.unil.Map 通过<map>标签定义，<map>标签里可以使用多个<entry>作为子标签，每个条目包含一个键和一个值。必须在<key>标签里定义键，因为键和值的类型没有限制，所以可以自由地为它们指定<value>、<ref>、<bean>或<null>元素。
可以将Map的键和值作为<entry>的属性定义：简单常量使用key和value来定义；Bean引用通过key-ref和value-ref属性定义。
使用<props>定义java.util.Properties，该标签使用多个<prop>作为子标签，每个<prop>标签必须定义key属性。
Cat类：
package com.spring.beans;

import java.util.List;
import java.util.Map;

public class Cat {
    private List<String> name;
    private Map<String ,Car> carMap;
    public Cat() {
    }

    public Map<String, Car> getCarMap() {
        return carMap;
    }

    public void setCarMap(Map<String, Car> carMap) {
        this.carMap = carMap;
    }

    public Cat(List<String> name, Map<String, Car> carMap) {
        this.name = name;
        this.carMap = carMap;
    }

    public List<String> getName() {
        return name;
    }

    public void setName(List<String> name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Cat{" +
                "name=" + name +
                ", carMap=" + carMap +
                '}';
    }
}
XML配置信息：
<bean name="cat2" class="com.spring.beans.Cat">
    <property name="name">
        <list>
            <value>花花</value>
            <value>白白</value>
        </list>
    </property>
    <property name="carMap">
        <map>
            <entry key="first" value-ref="car1"></entry>
            <entry key="second" value-ref="car2"></entry>
        </map>
    </property>
</bean>
主方法调用：
package com.spring.beans;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Main {
    public static void main(String[] args) {
        ApplicationContext ac = new ClassPathXmlApplicationContext("spring.xml");
        System.out.println("****************************10****************************\n");
        Cat cat2 = (Cat) ac.getBean("cat2");
        System.out.println(cat2);
    }
}
5.3	使用utility scheme定义集合
使用基本的集合标签定义集合时，不能将集合作为独立的Bean定义，导致其他Bean无法引用该集合，所以无法在不同Bean之间共享集合。 
可以使用util schema里的集合标签定义独立的集合Bean。需要注意的是，必须在<beans>根元素里添加util shema定义。
导入utility工作空间，在<beans>根元素中添加util schema定义如下：
xmlns:util="http://www.springframework.org/schema/util"
xsi中添加：http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.3.xsd
XML配置信息：
<util:list id="cars">
    <ref bean="car1"></ref>
    <ref bean="car2"></ref>
</util:list>

<bean id="person10" class="com.spring.beans.Person">
    <property name="name" value="权慑天下"></property>
    <property name="age" value="26"></property>
    <property name="car" ref="cars"></property>
</bean>
主方法调用：
ApplicationContext ac = new ClassPathXmlApplicationContext("spring.xml");
System.out.println("****************************12****************************\n");
Person person10 = (Person) ac.getBean("person10");
System.out.println(person10);
5.4	使用p命名空间
为了简化XML文件的配置，越来越多的XML文件采用属性而非子元素配置信息。
Spring从2.5版本开始引入了一个新的p命名空间，可以通过<bean>元素属性的方式配置<bean>的属性。使用p命名空间后，基于XML的配置方式将进一步简化。
导入P命名空间：
xmlns:p="http://www.springframework.org/schema/p"
XML文件配置：
<!--p命令空间可以通过属性的方式来进行字段的注入-->
<bean id="person11" class="com.spring.beans.Person" p:name="小周" p:age="20" p:car-ref="cars" p:dog-ref="dog1"></bean>
主方法调用：
ApplicationContext ac = new ClassPathXmlApplicationContext("spring.xml");
System.out.println("****************************12****************************\n");
Person person11 = (Person) ac.getBean("person11");
System.out.println(person11);
6	自动装配
Spring IOC容器可以自动装配Bean。需要做的仅仅是在<bean>的autowire属性指定自动装载的模式。
6.1	byType
根据类型自动装配:若IOC容器中有多个与目标Bean类型一致的Bean，在这种情况下，Spring将无法判断哪个Bean最合适该属性，所以不能执行自动装配。
XML配置信息：
<!--bean的自动装配，可以让一个bean对象自动的引用其他的bean
    bytype:按照类型进行装配
        缺点：如果在IOC容器中存在多个同类型的bean时，出现异常
    byName:按照名称进行装配,要求bean属性名称必须与引用的bean的id值保持一致-->
<bean id="car" class="com.spring.beans.Car" p:type="宝马"></bean>
<bean id="person1" class="com.spring.beans.Person" autowire="byType"></bean>
6.2	byName
根据名称自动装配：必须将目标Bean的名称和属性名设置的完全相同。
XML配置信息：
<!--bean的自动装配，可以让一个bean对象自动的引用其他的bean
    bytype:按照类型进行装配
        缺点：如果在IOC容器中存在多个同类型的bean时，出现异常
    byName:按照名称进行装配,要求bean属性名称必须与引用的bean的id值保持一致-->
<bean id="car" class="com.spring.beans.Car" p:type="宝马"></bean>
<bean id="person1" class="com.spring.beans.Person" autowire="byName"></bean>
6.3	constructor
通过构造器自动装配：当Bean中存在多个构造器时，此种自动装配方式将会很复杂。（不推荐使用）
6.4	缺点
在Bean配置文件里设置autowire属性进行自动装配将会装配Bean的所有属性。然而，若只希望装配个别属性时，autowire属性就不够灵活了。
Autowire属性要么根据类型自动装配，要么根据名称自动装配，不能两者兼而有之。
一般情况下，在实际的项目中很少使用自动装配功能，因为和自动装配功能所带来的好处比起来，明确清晰的配置文档更有说服力一些。
7	bean之间的关系
7.1	继承
Spring允许继承bean的配置，被继承的bean称为父bean，继承这个父bean的bean称为子bean。
子bean从父bean中继承配置，包括bean的属性配置。
子bean也可以覆盖父bean继承过来的配置。
子bean可以作为配置模板，也可以作为bean实例。若只想把父bean作为模板，可以设置<bean>的abstract属性为true，这样Spring将不会实例化这个bean。
并不是<bean>元素里的所有属性都被继承，比如：autowire，abstract等。
也可以忽略父bean的class元素，让子bean指定自己的类，而共享相同的属性配置，但此时abstract必须设为true。
XML配置信息：
<bean id="car" class="com.spring.beans.Car" p:type="宝马"></bean>
<bean id="person1" class="com.spring.beans.Person" p:name="小李" p:car-ref="car"></bean>
<!--继承-->
<bean id="person2" parent="person1" p:name="小王"></bean>
抽象类：
<!--抽象类-->
<!--1、指定父类class元素-->
<bean id="abstractPerson" abstract="true" class="com.spring.beans.Person" p:name="Tom" p:car-ref="car"></bean>
<bean id="person3" parent="abstractPerson"></bean>
<!--2、不指定父类class元素，子类bean指定-->
<bean id="abstractPerson1" abstract="true" p:name="Davie" p:car-ref="car"></bean>
<bean id="person4" parent="abstractPerson1" class="com.spring.beans.Person"></bean>
7.2	依赖
Spring允许用户通过depends-on属性设定bean前置依赖的bean，前置依赖的bean会在本bean实例化之前创建好。
如果前置依赖多个bean，则可以通过逗号，空格的方式配置bean的名称。
XML配置信息：
<!--依赖-->
<bean id="car1" class="com.spring.beans.Car" p:type="宝马"></bean>
<bean id="person5" class="com.spring.beans.Person" p:name="权慑天下" p:car-ref="car1" depends-on="car1"></bean>
8	bean的作用域
在Spring中，可以在<bean>元素的scope属性里设置bean的作用域。默认情况下，Spring只为每个在IOC容器里声明的bean创建唯一一个实例，整个IOC容器范围内都能共享该实例。该作用域被称为singleton，它是所有bean的默认作用域。
 
实例：
XML配置信息：
<!--作用域-->
<bean id="car2" class="com.spring.beans.Car" p:type="玛莎拉蒂" scope="prototype"></bean>
主方法调用：
Car car21 = (Car) ac.getBean("car2");
Car car22 = (Car) ac.getBean("car2");
System.out.println(car21==car22);
9	使用外部属性文件
在配置文件里配置bean时，有时需要在bean的配置里混入系统部署的细节信息（例如：文件路径，数据源配置信息等）。而这些部署细节实际上需要和bean配置相分离。
Spring提供了一个PropertyPlaceholderConfigurer的BeanFactory后置处理器，这个处理器允许用户将bean配置的部分内容外移到属性文件中，可以在bean配置文件里使用形式为${var}的变量，PropertyPlaceholderConfigurer从属性文件里加载属性，并使用这些属性来替换变量。
Spring还允许在属性文件中使用${propName}，以实现属性之前的相互作用。 
9.1	创建属性文件
在类路径下创建外部属性文件，*.properties key=value。
  
9.2	引用外部属性文件
配置文件中引用外部属性文件的内容，通过key值来引用。${key}
XML配置信息如下：
<bean id="person6" class="com.spring.beans.Person" p:name="${personName}" p:age="${personAge}" p:car-ref="car"></bean>
9.3	告知IOC容器引用的属性文件
配置文件中引用外部属性文件的内容，通过key值来引用。${key}
1、在xml文件配置信息中增加如下头文件：
xmlns:context="http://www.springframework.org/schema/context"
xsi:schemaLocation="http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd"
2、XML配置信息：
<!--引用外部属性文件，完成属性值与bean的配置分离，解耦
    1、创建属性文件：*.properties key=value
    2、配置文件中引用外部属性文件的内容，通过key值来引用。${key}
    3、告知IOC容器引用的属性文件
-->
<context:property-placeholder location="classpath:person.properties"></context:property-placeholder>
<bean id="person6" class="com.spring.beans.Person" p:name="${personName}" p:age="${personAge}" p:car-ref="car"></bean>
10	SpEL
Spring表达式语言（简称SpEL）:是一个支持运行时查询和操作对象图的强大的表达式语言。
语法类似于EL：SpEL使用#{…}作为定界符，所有在大括号中的字符都将被认为是SpEL，SpEL为bean的属性进行动态赋值提供了便利。
通过SpEL可以实现：通过bean的id对bean进行引用，调用方法以及引用对象中的属性，计算表达式的值，正则表达式的匹配。
10.1	通过bean的id对bean进行引用
通过bean的id对bean进行引用，ref属性、ref节点。
XML配置信息：
<bean id="car1" class="com.spring.beans.Car" p:type="宝马"></bean>
<bean id="person7" class="com.spring.beans.Person">
    <property name="name" value="张三"></property>
    <property name="age" value="28"></property>
    <property name="car" value="#{car1}"></property>
</bean>
10.2	调用方法以及引用对象中的属性
调用方法以及引用对象中的属性，XML配置信息如下：
1、引用对象中的属性
<bean id="person7" class="com.spring.beans.Person">
    <property name="name" value="张三"></property>
    <property name="age" value="28"></property>
    <property name="car" value="#{car1}"></property>
</bean>
<bean id="person8" class="com.spring.beans.Person">
    <property name="name" value="王五"></property>
    <property name="age" value="23"></property>
    <property name="car" value="#{person7.car}"></property>
</bean>
2、调用方法
<bean id="person7" class="com.spring.beans.Person">
    <property name="name" value="tom"></property>
    <property name="age" value="28"></property>
    <property name="car" value="#{car1}"></property>
</bean>
<bean id="person8" class="com.spring.beans.Person">
    <property name="name" value="#{person7.name.toUpperCase()}"></property>
    <property name="age" value="23"></property>
    <property name="car" value="#{person7.car}"></property>
</bean>
10.3	计算表达式的值
计算表达式，包括算数运算、关系运算、逻辑运算，XML配置信息如下：
<bean id="person7" class="com.spring.beans.Person">
    <property name="name" value="tom"></property>
    <property name="age" value="28"></property>
    <property name="car" value="#{car1}"></property>
</bean>
<bean id="person8" class="com.spring.beans.Person">
    <property name="name" value="#{person7.name.toUpperCase()}"></property>
    <property name="age" value="#{person7.age+20}"></property>
    <property name="car" value="#{person7.car}"></property>
</bean>
注意逻辑运算符号：and，or，not，正则表达式：matches。
11	bean的生命周期
SpringIOC容器可以管理bean的生命周期，Spring允许在bean生命周期的特定点执行定制的任务。
SpringIOC容器对bean的生命周期进行管理的过程：
通过构造器或工厂方法创建bean的实例
为bean的属性设置值和对其他bean的引用
调用bean的初始化方法
bean可以使用了。
当容器关闭时，调用bean的销毁方法
在bean的声明里设置init-method和destroy-method属性，为bean指定初始化和销毁方法。
12	创建bean后置处理器
Bean后置处理器允许在调用初始化方法前后对Bean进行额外的处理。
Bean后置处理器对IOC容器里的所有Bean实例逐一处理，而非单一实例。其典型应用是：检查Bean属性的正确性或根据特定的标准更改Bean的属性。
对Bean后置处理器而言，需要实现Interface BeanPostProcessor接口。在初始化方法被调用前后，Spring将把每个Bean实例分别传递给上述接口的以下两个方法：
 
SpringIOC容器对Bean的生命周期进行管理的过程：
通过构造器或工厂方法创建Bean实例，为Bean的属性设置值和对其他Bean的引用，将Bean实例传递给Bean后置处理器的postProcessBeforeInitialization方法，调用Bean的初始化方法，将Bean实例传递给Bean后置处理器的postProcessAfterInitialization方法，Bean可以使用了，当容器关闭时，调用Bean的销毁方法。
