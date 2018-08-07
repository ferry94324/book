## 2018-8-7

---

**1.下列修饰符中，能够使得某个成员变量只能被它所在包访问到和它的子类访问到的是（）**

**【解析**】：![](/assets/关键字范围.png)所以满足能被包访问和子类访问的修饰符只有protected。

**2.以下关于abstract 关键字的说法，正确的是（B）**

* **abstract 可以与final 并列修饰同一个类。**
* **abstract 类中不可以有private的成员。**
* **abstract 类中必须全部是abstract方法。**
* **abstract 方法必须在abstract类或接口中。**

【**解析**】：含有abstract修饰符的class即为抽象类，abstract 类不能创建的实例对象。含有abstract方法的类必须定义为abstract class，abstract class类中的方法不必是抽象的。abstract class类中定义抽象方法必须在具体\(Concrete\)子类中实现，所以，不能有抽象构造方法或抽象静态方法。如果的子类没有实现抽象父类中的所有抽象方法，那么子类也必须定义为abstract类型。



