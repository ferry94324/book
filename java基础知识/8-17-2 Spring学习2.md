# Spring 学习2

### 1、什么是Spring框架？Spring框架有哪些主要模块？

Spring框架是一个为Java应用程序的开发提供了综合、广泛的基础性支持的Java平台。

Spring帮助开发者解决了开发中基础性的问题，使得开发人员可以专注于应用程序的开发。

Spring框架本身亦是按照设计模式精心打造，这使得我们可以在开发环境中安心的集成Spring框架，不必担心Spring是如何在后台进行工作的。

Spring框架至今已集成了20多个模块。这些模块主要被分如下图所示的核心容器、数据访问/集成,、Web、AOP（面向切面编程）、工具、消息和测试模块。![](/assets/Spring1.png)2、使用Spring框架能带来哪些好处？



下面列举了一些使用Spring框架带来的主要好处：

* Dependency Injection\(DI\) 方法使得构造器和JavaBean properties文件中的依赖关系一目了然。
* 与EJB容器相比较，IoC容器更加趋向于轻量级。这样一来IoC容器在有限的内存和CPU资源的情况下进行应用程序的开发和发布就变得十分有利。
* Spring并没有闭门造车，Spring利用了已有的技术比如ORM框架、logging框架、J2EE、Quartz和JDK Timer，以及其他视图技术。
* Spring框架是按照模块的形式来组织的。由包和类的编号就可以看出其所属的模块，开发者仅仅需要选用他们需要的模块即可。
* 要测试一项用Spring开发的应用程序十分简单，因为测试相关的环境代码都已经囊括在框架中了。更加简单的是，利用JavaBean形式的POJO类，可以很方便的利用依赖注入来写入测试数据。
* Spring的Web框架亦是一个精心设计的Web MVC框架，为开发者们在web框架的选择上提供了一个除了主流框架比如Struts、过度设计的、不流行web框架的以外的有力选项。
* Spring提供了一个便捷的事务管理接口，适用于小型的本地事物处理（比如在单DB的环境下）和复杂的共同事物处理（比如利用JTA的复杂DB环境）。

### 3、什么是控制反转\(IOC\)？什么是依赖注入？

控制反转是应用于软件工程领域中的，在运行时被装配器对象来绑定耦合对象的一种编程技巧，对象之间耦合关系在编译时通常是未知的。在传统的编程方式中，业务逻辑的流程是由应用程序中的早已被设定好关联关系的对象来决定的。在使用控制反转的情况下，业务逻辑的流程是由对象关系图来决定的，该对象关系图由装配器负责实例化，这种实现方式还可以将对象之间的关联关系的定义抽象化。而绑定的过程是通过“依赖注入”实现的。

控制反转是一种以给予应用程序中目标组件更多控制为目的设计范式，并在我们的实际工作中起到了有效的作用。

依赖注入是在编译阶段尚未知所需的功能是来自哪个的类的情况下，将其他对象所依赖的功能对象实例化的模式。这就需要一种机制用来激活相应的组件以提供特定的功能，所以依赖注入是控制反转的基础。否则如果在组件不受框架控制的情况下，框架又怎么知道要创建哪个组件？

在[Java](http://lib.csdn.net/base/javase)中依然注入有以下三种实现方式：

1. 构造器注入
2. Setter方法注入
3. 接口注入

### 4、请解释下Spring框架中的IoC？

Spring中的 `org.springframework.beans` 包和 `org.springframework.context包构成了Spring框架IoC容器的基础。`

BeanFactory 接口提供了一个先进的配置机制，使得任何类型的对象的配置成为可能。`ApplicationContex接口对BeanFactory`（是一个子接口）进行了扩展，在BeanFactory的基础上添加了其他功能，比如与[Spring的AOP](http://howtodoinjava.com/category/frameworks/java-spring-tutorials/spring-aop/)更容易集成，也提供了处理[message resource的机制](http://howtodoinjava.com/2015/02/10/spring-mvc-internationalization-i18n-and-localization-i10n-example/)（用于国际化）、事件传播以及应用层的特别配置，比如针对Web应用的WebApplicationContext。

`org.springframework.beans.factory.BeanFactory` 是Spring IoC容器的具体实现，用来包装和管理前面提到的各种bean。BeanFactory接口是Spring IoC 容器的核心接口。

IOC:把对象的创建、初始化、销毁交给spring来管理，而不是由开发者控制，实现控制反转。

### 5、BeanFactory和ApplicationContext有什么区别？

BeanFactory 可以理解为含有bean集合的工厂类。BeanFactory 包含了种bean的定义，以便在接收到客户端请求时将对应的bean实例化。

BeanFactory还能在实例化对象的时生成协作类之间的关系。此举将bean自身与bean客户端的配置中解放出来。BeanFactory还包含了bean生命周期的控制，调用客户端的初始化方法（initialization methods）和销毁方法（destruction methods）。

从表面上看，application context如同bean factory一样具有bean定义、bean关联关系的设置，根据请求分发bean的功能。但applicationcontext在此基础上还提供了其他的功能。

1. 提供了支持国际化的文本消息
2. 统一的资源文件读取方式
3. 已在监听器中注册的bean的事件

以下是三种较常见的 ApplicationContext 实现方式：

1、ClassPathXmlApplicationContext：从classpath的XML配置文件中读取上下文，并生成上下文定义。应用程序上下文从程序环境变量中取得。

```
派生到我的代码片
ApplicationContext context = new ClassPathXmlApplicationContext(“bean.xml”);    
```

2、FileSystemXmlApplicationContext ：由文件系统中的XML配置文件读取上下文。

```
 在CODE上查看代码片派生到我的代码片
ApplicationContext context = new FileSystemXmlApplicationContext(“bean.xml”); 
```

3、XmlWebApplicationContext：由Web应用的XML文件读取上下文。

### 6、Spring有几种配置方式？

将Spring配置到应用开发中有以下三种方式：

1. 基于XML的配置
2. 基于注解的配置
3. 基于Java的配置

### 7、如何用基于XML配置的方式配置Spring？

在Spring框架中，依赖和服务需要在专门的配置文件来实现，我常用的XML格式的配置文件。这些配置文件的格式通常用&lt;beans&gt;开头，然后一系列的bean定义和专门的应用配置选项组成。

SpringXML配置的主要目的时候是使所有的Spring组件都可以用xml文件的形式来进行配置。这意味着不会出现其他的Spring配置类型（比如声明的方式或基于[Java ](http://lib.csdn.net/base/java)Class的配置方式）

Spring的XML配置方式是使用被Spring命名空间的所支持的一系列的XML标签来实现的。Spring有以下主要的命名空间：context、beans、jdbc、tx、aop、mvc和aso。

```
<beans>    
    <!-- JSON Support -->    
    <bean name="viewResolver" class="org.springframework.web.servlet.view.BeanNameViewResolver"/>    
    <bean name="jsonTemplate" class="org.springframework.web.servlet.view.json.MappingJackson2JsonView"/>    
    <bean id="restTemplate" class="org.springframework.web.client.RestTemplate"/>    
</beans>   
```

下面这个web.xml仅仅配置了DispatcherServlet，这件最简单的配置便能满足应用程序配置运行时组件的需求。

```
<web-app>    
    <display-name>Archetype Created Web Application</display-name>    
    <servlet>    
        <servlet-name>spring</servlet-name>    
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>    
        <load-on-startup>1</load-on-startup>    
    </servlet>    
    <servlet-mapping>    
        <servlet-name>spring</servlet-name>    
        <url-pattern>/</url-pattern>    
    </servlet-mapping>    
</web-app> 
```

### 8、如何用基于Java配置的方式配置Spring？

Spring对Java配置的支持是由@Configuration注解和@Bean注解来实现的。由@Bean注解的方法将会实例化、配置和初始化一个新对象，这个对象将由Spring的IoC容器来管理。@Bean声明所起到的作用与&lt;bean/&gt; 元素类似。被@Configuration所注解的类则表示这个类的主要目的是作为bean定义的资源。被@Configuration声明的类可以通过在同一个类的内部调用@bean方法来设置嵌入bean的依赖关系。

最简单的@Configuration 声明类请参考下面的代码：

```
@Configuration    
public class AppConfig{    
    @Bean    
    public MyService myService() {    
        return new MyServiceImpl();    
    }    
}
```

对于上面的@Beans配置文件相同的XML配置文件如下：

```
<beans>    
    <bean id="myService" class="com.somnus.services.MyServiceImpl"/>    
</beans>  
```

上述配置方式的实例化方式如下：利用AnnotationConfigApplicationContext 类进行实例化

```
public static void main(String[] args) {    
    ApplicationContext ctx = new AnnotationConfigApplicationContext(AppConfig.class);    
    MyService myService = ctx.getBean(MyService.class);    
    myService.doStuff();    
} 
```

要使用组件组建扫描，仅需用@Configuration进行注解即可：

```
@Configuration    
@ComponentScan(basePackages = "com.somnus")    
public class AppConfig  {    
    ...    
}  
```

在上面的例子中，com.acme包首先会被扫到，然后再容器内查找被@Component 声明的类，找到后将这些类按照Sring bean定义进行注册。

如果你要在你的web应用开发中选用上述的配置的方式的话，需要用AnnotationConfigWebApplicationContext 类来读取配置文件，可以用来配置Spring的Servlet监听器ContextLoaderListener或者Spring MVC的DispatcherServlet。

```
<web-app>    
    <!-- Configure ContextLoaderListener to use AnnotationConfigWebApplicationContext    
        instead of the default XmlWebApplicationContext -->    
    <context-param>    
        <param-name>contextClass</param-name>    
        <param-value>    
            org.springframework.web.context.support.AnnotationConfigWebApplicationContext    
        </param-value>    
    </context-param>    
     
    <!-- Configuration locations must consist of one or more comma- or space-delimited    
        fully-qualified @Configuration classes. Fully-qualified packages may also be    
        specified for component-scanning -->    
    <context-param>    
        <param-name>contextConfigLocation</param-name>    
        <param-value>com.howtodoinjava.AppConfig</param-value>    
    </context-param>    
     
    <!-- Bootstrap the root application context as usual using ContextLoaderListener -->    
    <listener>    
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>    
    </listener>    
     
    <!-- Declare a Spring MVC DispatcherServlet as usual -->    
    <servlet>    
        <servlet-name>dispatcher</servlet-name>    
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>    
        <!-- Configure DispatcherServlet to use AnnotationConfigWebApplicationContext    
            instead of the default XmlWebApplicationContext -->    
        <init-param>    
            <param-name>contextClass</param-name>    
            <param-value>    
                org.springframework.web.context.support.AnnotationConfigWebApplicationContext    
            </param-value>    
        </init-param>    
        <!-- Again, config locations must consist of one or more comma- or space-delimited    
            and fully-qualified @Configuration classes -->    
        <init-param>    
            <param-name>contextConfigLocation</param-name>    
            <param-value>com.howtodoinjava.web.MvcConfig</param-value>    
        </init-param>    
    </servlet>    
     
    <!-- map all requests for /app/* to the dispatcher servlet -->    
    <servlet-mapping>    
        <servlet-name>dispatcher</servlet-name>    
        <url-pattern>/app/*</url-pattern>    
    </servlet-mapping>    
</web-app    
```

### 9、怎样用注解的方式配置Spring？

Spring在2.5版本以后开始支持用注解的方式来配置依赖注入。可以用注解的方式来替代XML方式的bean描述，可以将bean描述转移到组件类的内部，只需要在相关类上、方法上或者字段声明上使用注解即可。注解注入将会被容器在XML注入之前被处理，所以后者会覆盖掉前者对于同一个属性的处理结果。

注解装配在Spring中是默认关闭的。所以需要在Spring文件中配置一下才能使用基于注解的装配模式。如果你想要在你的应用程序中使用关于注解的方法的话，请参考如下的配置。

```
<beans>    
   <context:annotation-config/>    
   <!-- bean definitions go here -->    
</beans> 
```

在 &lt;context:annotation-config/&gt;标签配置完成以后，就可以用注解的方式在Spring中向属性、方法和构造方法中自动装配变量。

下面是几种比较重要的注解类型：

1. @Required：该注解应用于设值方法。
2. @Autowired：该注解应用于有值设值方法、非设值方法、构造方法和变量。
3. @Qualifier：该注解和@Autowired注解搭配使用，用于消除特定bean自动装配的歧义。
4. JSR-250 Annotations： Spring支持基于JSR-250 注解的以下注解，@Resource、@PostConstruct 和 @PreDestroy。



