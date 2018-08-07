## 2018-8-7

---

**1.下列修饰符中，能够使得某个成员变量只能被它所在包访问到和它的子类访问到的是（）**

**【解析**】：![](/assets/关键字范围.png)所以满足能被包访问和子类访问的修饰符只有protected。

**2.以下关于abstract 关键字的说法，正确的是（D）**

* **abstract 可以与final 并列修饰同一个类。**
* **abstract 类中不可以有private的成员。**
* **abstract 类中必须全部是abstract方法。**
* **abstract 方法必须在abstract类或接口中。**

【**解析**】：含有abstract修饰符的class即为抽象类，abstract 类不能创建的实例对象。含有abstract方法的类必须定义为abstract class，abstract class类中的方法不必是抽象的。abstract class类中定义抽象方法必须在具体\(Concrete\)子类中实现，所以，不能有抽象构造方法或抽象静态方法。如果的子类没有实现抽象父类中的所有抽象方法，那么子类也必须定义为abstract类型。

**3.Java的跨平台特性是指它的源代码可以在多个平台运行**。

【**解析**】：语言跨平台是编译后的文件跨平台，而不是源程序跨平台。Java源代码首先经过编译器生成字节码，即class文件，该class文件与平台无关，而class文件经过解释执行之后翻译成最终的机器码，这是平台相关的。

**4.关于JAVA堆，下面说法错误的是（C）**

* **所有类的实例和数组都是在堆上分配内存的**
* **堆内存由存活和死亡的对象，空闲碎片区组成**
* **数组是分配在栈中的**
* **对象所占的堆内存是由自动内存管理系统回收**

【**解析**】：所以这句话其实应该改为，数组的引用存在栈内存中，而数组对象保存在堆里面。

**5.非抽象类实现接口后，必须实现接口中的所有抽象方法，除了abstract外，方法头必须完全一致（错误）**

【**解析**】：实际上这道题考查的是**两同两小一大**原则：

方法名相同，参数类型相同

子类返回类型小于等于父类方法返回类型，

子类抛出异常小于等于父类方法抛出异常，

子类访问权限大于等于父类方法访问权限。

**6.SimpleDateFormat对象是线程不安全的**

【**解析**】SimpleDateFormat是线程不安全的类，一般不要定义为static变量，如果定义为static，必须加锁，或者使用DateUtils工具类。SimpleDateFormat类内部有一个Calendar对象引用,它用来储存和这个SimpleDateFormat相关的日期信息,例如sdf.parse\(dateStr\),sdf.format\(date\) 诸如此类的方法参数传入的日期相关String,Date等等, 都是交由Calendar引用来储存的.这样就会导致一个问题,如果你的SimpleDateFormat是个static的, 那么多个thread 之间就会共享这个SimpleDateFormat, 同时也是共享这个Calendar引用。单例、多线程、又有成员变量（这个变量在方法中是可以修改的），这个场景是不是很像servlet，在高并发的情况下，容易出现幻读成员变量的现象，故说SimpleDateFormat是线程不安全的对象。

**7.关于访问权限说法正确 的是 ？ \( \)**

* **外部类前面可以修饰public,protected和private**
* **成员内部类前面可以修饰public,protected和private**
* **局部内部类前面可以修饰public,protected和private**
* **以上说法都不正确**

【**解析**】（ 1 ）对于外部类而言，它也可以使用访问控制符修饰，但外部类只能有两种访问控制级别： public 和默认。因为外部类没有处于任何类的内部，也就没有其所在类的内部、所在类的子类两个范围，因此 private 和 protected 访问控制符对外部类没有意义。

（ 2 ）内部类的上一级程序单元是外部类，它具有 4 个作用域：同一个类（ private ）、同一个包（ protected ）和任何位置（ public ）。

（ 3 ） 因为局部成员的作用域是所在方法，其他程序单元永远不可能访问另一个方法中的局部变量，所以所有的局部成员都不能使用访问控制修饰符修饰。

**8.以下声明合法的是**

* **default  String  s**
* **public  final  static  native  int  w\( \)**
* **abstract  double  d**
* **abstract  final  double  hyperbolicCosine\( \)**

【**解析**】

A：java的访问权限有public、protected、private和default的，default不能修饰变量

C：普通变量不能用abstract修饰，abstract一般修饰方法和类

D：被定义为abstract的类需要被子类继承，但是被修饰为final的类是不能被继承和改写的

**9.存根（Stub）与以下哪种技术有关-动态链接**

【**解析**】

存根类是一个类，它实现了一个接口，它的作用是：如果一个接口有很多方法，如果要实现这个接口，就要实现所有的方法。但是一个类从业务来说，可能只需要其中一两个方法。如果直接去实现这个接口，除了实现所需的方法，还要实现其他所有的无关方法。而如果通过继承存根类就实现接口，就免去了这种麻烦。

RMI 采用stubs 和 skeletons 来进行远程对象\(remote object\)的通讯。stub 充当远程对象的客户端代理，有着和远程对象相同的远程接口，远程对象的调用实际是通过调用该对象的客户端代理对象stub来完成的。

每个远程对象都包含一个代理对象stub，当运行在本地Java虚拟机上的程序调用运行在远程Java虚拟机上的对象方法时，它首先在本地创建该对象的代理对象stub, 然后调用代理对象上匹配的方法。每一个远程对象同时也包含一个skeleton对象，skeleton运行在远程对象所在的虚拟机上，接受来自stub对象的调用。这种方式符合等到程序要运行时将目标文件动态进行链接的思想。

**10. 接口中方法的 定义**

* **public void main\(String \[\] args\);**
* **private int getSum\(\);**
* **boolean setFlag\(Boolean \[\] test\);**
* **public float get\(int x\);**

【**解析**】：

java程序的入口必须是static类型的，接口中不允许有static类型的方法。A项没有static修饰符，可以作为普通的方法。而且接口中的方法必须是public的。想想借口就是为了让别人实现的，相当于标准，标准不允许别人使用是不合理的，所以接口中的方法必须是public。C项中，接口中的方法默认是public的。D项属于正常的方法。所以答案是：ACD

