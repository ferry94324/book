## 2018-8-8

---

**1.下面的输出结果正确的是**

```
public class Increment
{
    public static void main(String args[])
    {
        int a;
        a = 6;
        System.out.print(a);
        System.out.print(a++);
        System.out.print(a);
    }
}
```

* **666  **
* **667  **
* **677  **
* **676**

【**解析**】：a++可以理解为当访问a之后再对a进行加一操作

**2.下面关于abstract关键字描述错误的是（）**

* **abstract关键字可以修饰类或方法  **
* **final类的方法都不能是abstract，因为final类不能有子类  **
* **abstract类不能实例化  **
* **abstract类的子类必须实现其超类的所有abstract方法**

【**解析**】：

D：含有抽象方法的类\(包括直接定义了抽象方法；继承一个抽象父类，但没有完全实现父类包含的抽象方法；实现一个接口，但没有完全实现接口包含的抽象方法\)只能被定义成抽象类。

A：用于修饰抽象类或者抽象方法

B：final修饰的类不能被继承

C：抽象类不能被实例化，无法使用new关键字调用抽象类的构造器创建抽象类的实例，即使抽象类不包含抽象方法，也不能被实例化。

**3.下列语句正确的是：**

* **形式参数可被字段修饰符修饰  **
* **形式参数不可以是对象  **
* **形式参数为方法被调用时真正被传递的参数  **
* **形式参数可被视为local variable**

【**解析**】：

![](/assets/类.png)

**4.下面有关struts1和struts2的区别，描述错误的是？**

* **Struts1要求Action类继承一个抽象基类。Struts 2 Action类可以实现一个Action接口  **
* **Struts1 Action对象为每一个请求产生一个实例。Struts2 Action是单例模式并且必须是线程安全的  **
* **Struts1 Action 依赖于Servlet API，Struts 2 Action不依赖于容器，允许Action脱离容器单独被测试  **
* **Struts1 整合了JSTL，Struts2可以使用JSTL，但是也支持OGNL**

【**解析**】：

Action 类:  

• Struts1要求Action类继承一个抽象基类。Struts1的一个普遍问题是使用抽象类编程而不是接口，而struts2的Action是接口。  

• Struts 2 Action类可以实现一个Action接口，也可实现其他接口，使可选和定制的服务成为可能。Struts2提供一个ActionSupport基类去 实现 常用的接口。Action接口不是必须的，任何有execute标识的POJO对象都可以用作Struts2的Action对象。 

线程模式:  

• Struts1 Action是单例模式并且必须是线程安全的，因为仅有Action的一个实例来处理所有的请求。单例策略限制了Struts1 Action能作的事，并且要在开发时特别小心。Action资源必须是线程安全的或同步的。 

• Struts2 Action对象为每一个请求产生一个实例，因此没有线程安全问题。（实际上，servlet容器给每个请求产生许多可丢弃的对象，并且不会导致性能和垃圾回收问题） 

Servlet 依赖:  

• Struts1 Action 依赖于Servlet API ,因为当一个Action被调用时HttpServletRequest 和 HttpServletResponse 被传递给execute方法。 

• Struts 2 Action不依赖于容器，允许Action脱离容器单独被测试。如果需要，Struts2 Action仍然可以访问初始的request和response。但是，其他的元素减少或者消除了直接访问HttpServetRequest 和 HttpServletResponse的必要性。 

可测性:  

• 测试Struts1 Action的一个主要问题是execute方法暴露了servlet API（这使得测试要依赖于容器）。一个第三方扩展－－Struts TestCase－－提供了一套Struts1的模拟对象（来进行测试）。 

• Struts 2 Action可以通过初始化、设置属性、调用方法来测试，“依赖注入”支持也使测试更容易。  

捕获输入:  

• Struts1 使用ActionForm对象捕获输入。所有的ActionForm必须继承一个基类。因为其他JavaBean不能用作ActionForm，开发者经常创建多余的类捕获输入。动态Bean（DynaBeans）可以作为创建传统ActionForm的选择，但是，开发者可能是在重新描述\(创建\)已经存 在的JavaBean（仍然会导致有冗余的javabean）。 

• Struts 2直接使用Action属性作为输入属性，消除了对第二个输入对象的需求。输入属性可能是有自己\(子\)属性的rich对象类型。Action属性能够通过 web页面上的taglibs访问。Struts2也支持ActionForm模式。rich对象类型，包括业务对象，能够用作输入/输出对象。这种 ModelDriven 特性简化了taglib对POJO输入对象的引用。 

表达式语言：  

• Struts1 整合了JSTL，因此使用JSTL EL。这种EL有基本对象图遍历，但是对集合和索引属性的支持很弱。  

• Struts2可以使用JSTL，但是也支持一个更强大和灵活的表达式语言－－"Object Graph Notation Language" \(OGNL\). 

