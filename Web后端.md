# Java

  ## 基础语法

### 面向对像编程

#### 继承

Person类

>```java
>class  Person
>{
>    private String m_name;
>    private int m_Age;
>    public  void setName(String name)
>    {
>        this.m_name=name;
>    }
>    public String getName()
>    {
>        return this.m_name;
>    }
>    public  void setAge(int age)
>    {
>        this.m_Age=age;
>    }
>    public int  getAge()
>    {
>        return this.m_Age;
>    }
>}
>```
>
>

 现在需要创建一个学生类 属性有 name,age score ;而学生类相对于Person只多了score 这个时候就可以用到继承。

继承是面向对象编程中非常强大的一种机制，它首先可以复用代码。当我们让`Student`从`Person`继承时，`Student`就获得了`Person`的所有功能，我们只需要为`Student`编写新增的功能。

Java使用`extends`关键字来实现继承：

>```java
>class Student extends Person
>{
>    //不需要重复 name ，age，字段和方法；只需要定义新增的
>    private int m_Score;
>    public void setScore(int score)
>    {
>        this.m_Score=score;
>    }
>    public int getScore()
>    {
>        return this.m_Score;
>    }
>```

*** 注意：子类自动获得了父类的所有字段，严禁定义与父类重名的字段！***

我们把`Person`称为超类（super class），父类（parent class），基类（base class），把`Student`称为子类（subclass），扩展类（extended class）。

 java只允许一个calss继承自一个类，因此一个类有且只有一个父类

Protected  

  			子类无法访问父类的private字段或者private方法 

 我们可以用关键字protected  protect关键字把字段和方法的访问权限控制在继承树内部，一个`protected`字段和方法可以被其子类，以及子类的子类所访问，

**super**

​    super关键字表示父类（超类)。子类引用父类的字段时可以用  super.filename  实际上也可以用 this.filename代替

**阻止继承**

正常情况下，只要某个class没有`final`修饰符，那么任何类都可以从该class继承。

允许使用`sealed`修饰class，并通过`permits`明确写出能够从该class继承的子类名称。

定义一个`Shape`类：

>```
>public sealed class Shape permits Rect, Circle, Triangle {
>    ...
>}
>```

上述`Shape`类就是一个`sealed`类，它只允许指定的3个类继承它。如果写：

>```
>public final class Rect extends Shape {...}
>```

是没问题的，因为`Rect`出现在`Shape`的`permits`列表中。但是，如果定义一个`Ellipse`就会报错：

> ```
> public final class Ellipse extends Shape {...}
> ```

原因是`Ellipse`并未出现在`Shape`的`permits`列表中。这种`sealed`类主要用于一些框架，防止继承被滥用。

**要使用sealed关键字必须使用参数`--enable-preview`和`--source 15`。**

**向上转型**

向上转型实际上是把一个子类型安全地变为更加抽象的父类型：

```
Student s = new Student();
Person p = s; // upcasting, ok
```

注意到继承树是`Student > Person `，所以，可以把`Student`类型转型为`Person`。

为了避免向下转型出错，Java提供了`instanceof`操作符，可以先判断一个实例究竟是不是某种类型：

利用`instanceof`，在向下转型前可以先判断：

```
Person p = new Student();
if (p instanceof Student) {
    // 只有判断成功才会向下转型:
    Student s = (Student) p; // 一定会成功
}
```

#### 多态

​	在继承关系中，子类如果定义了一个与父类方法签名完全相同的方法，被称为覆写（Override）。

例如，在`Person`类中，我们定义了`run()`方法：

```
class Person {
    public void run() {
        System.out.println("Person.run");
    }
}
```

在子类`Student`中，覆写这个`run()`方法：

```
class Student extends Person {
    @Override
    public void run() {
        System.out.println("Student.run");
    }
}
```

但是`@Override`不是必需的。

在上一节中，我们已经知道，引用变量的声明类型可能与其实际类型不符，例如：

```
Person p = new Student();
```

现在，我们考虑一种情况，如果子类覆写了父类的方法：

~~~public class Main {
    public static void main(String[] args) {
        Person p = new Student();
        p.run(); // 应该打印Person.run还是Student.run?
    }
}

class Person {
    public void run() {
        System.out.println("Person.run");
    }
}

class Student extends Person {
    @Override
    public void run() {
        System.out.println("Student.run");
    }
}
~~~

Java的实例方法调用是基于运行时的实际类型的动态调用，而非变量的声明类型。

这个非常重要的特性在面向对象编程中称之为多态。它的英文拼写非常复杂：Polymorphic。

**多态**

多态是指，针对某个类型的方法调用，其真正执行的方法取决于运行时期实际类型的方法。例如：

多态的访问 ： 编译看左边，运行看右边  。但是两边都得有

**多态的好处与弊端**

* 多态的好处 ：提高了程序的扩展性

 			具体体现： 定义方法时，使用父类型作为参数，在将来使用时，使用具体的子类型参与操作

* 多态的弊端 ：不能使用子类的特有功能

**多态的转型**

* 向上转型 

​				从子到父

​				父类引用指向子类对象

* 向下转型

  ​		  从父到子

  ​			父类引用转为子类对像


#### 抽象类

##### 1.1抽象类的定义

在Java中，一个没有方法体的方法应该定义为抽象方法 ，而类中如果有抽象方法，该类必须定义为抽象类

##### 1.2抽象类的特点

* 抽象类和抽象方法必须使用abstract关键字修饰

​		public abstract class 类名{}

​		public abstract void eat();

* 抽象类中不一定有抽象方法，抽象方法的类一定是抽象类

* 抽象类不能实例化

  ​			抽象类如何实例化呢?参照多态的方法，通过子类对象实例化，这叫抽象类多态

* 抽象类的子类

  ​	要么重写抽象类中的所有抽象方法

  ​	要么是抽象类

  ##### 1.3 抽象类的成员特点

*  成员变量

​			可以是变量

​			也可以是常量

* 构造方法

​			有构造方法，但是不能实例化

​			那么，构造方法的作用是什么呢？用于子类访问父类数据的初始化

* 成员方法 

​			可以有抽象方法：限定子类必须完成某些动作

​			也可以有非抽象方法：提高代码的复用性

#### 接口

#####  1.1接口概述

   接口就是一种***公共的规范标准***，只要符合规范标准，大家都可以使用

​	Java中的接口更多的体现在***对行为的抽像***

##### 1.2 接口的特点

* 接口用关键字**interface** 修饰

​			public **interface** 接口名 {}

* 类实现接口用**implements** 表示

​			public class 类名 **implements** 接口名{}

* 接口不能实例化

​			接口实现实例化参照多态的方式，通过实现类对象实例化这叫接口多态

​			多态的形式：具体多态，***抽象类多态，接口多态***。

​			*多态的前提*：有继承或者实现关系；有方法重写；有父（类/接口）引用指向（子/实现）类对象

* 接口中的实现类

​			要么重写接口中的所有抽象方法

​			要么是抽象类

##### 1.3接口的成员特点

* 成员变量 

​			只能是常量

​			默认修饰符：**public static final** 

* 构造方法

​			接口中没有构造方法，因为接口主要是对行为进行抽象的，是没有具体存在

​			一个类如果没有父类会自动继承字自Object类

* 成员方法

​			只能是抽象方法

​			默认修饰符：public abstract

   ##### 1.4类和接口的关系

* 类和类的关系

​			继承关系，只能单继承，但是可以多层继承

* 类和接口的关系

​			实现关系，可以但实现，也可以多实现，还可以在继承一个类的同时实现多个接口

* 接口和接口的关系

​			继承关系，可以单继承，也可以多继承

##### 1.5 抽象类和接口的区别

* 成员区别

​			抽象类					变量，常量；有构造方法；由抽象方法，也有非抽象方法

​			接口						常量；抽象方法

* 关系区别

​			类与类					继承，单继承

​			类与接口				实现，可以单实现，也可以多实现

​			接口与接口			继承，单继承，也可以多继承

* 设计理念区别

​			抽象类					对类抽象，包括属性、行为

​			接口						对行为抽象，主要是行为

​		门和警报的例子

​		门：都有open()和close()两个动作，这个时候，我们可以分别使用抽象类和接口来定义这个抽象的概念

​		**强调** 抽象类是对事物类的抽象，而接口是对行为的抽象

~~~
   //抽象类
   public abstract class Door{
   		public abstract void open();
   		public abstract void close();	
   }

~~~

~~~
  //接口
  public interface Door{
  		void open();
  		void close();
  }
~~~

~~~
  public interface Alram{
  		VOID alarm();
  }
  public abstract class Door{
  		public abstract void open();
   		public abstract void close();	
  }
  public class AlramDoor extends Door implements Alram{
  		public void open(){
  		//....
  		}
  		public void close(){
  		//....
  		}
  		public void alram(){
  		//....
  		}
  }
~~~

#### 形参和返回值

##### 1.1类名作为形参和返回值

* 方法的形参类名，其实需要的是该类的对象
* 方法的返回值是类名，其实返回的是该类的对象



##### 1.2 抽象类名作为形参和返回值

* 方法的形参是抽象类名，其实需要的是该抽象类的子类对象
* 方法的返回值是抽象类名，其实返回的是该抽象类的子类对象.



##### 1.3 接口名作为形参和返回值

* 方法的形参是接口名，其实需要的是该接口的实现类对像
* 方法的返回值是接口名，其实返回的是该接口的实现类对象





#### 内部类

##### 1.1内部类概述

**内部类**：就是在类中定义一个类。举例：在一个类A中定义一个类B，类B就被称为内部类

内部类的定义格式

* 格式

~~~ 
public class 类名{
			修饰符 class 类名{
			}
  }
~~~

* 范例

~~~ 
public class Outer{
		public class Inner{
		
		}
}
~~~

内部类的特点

* 内部类可以直接访问外部类的成员，包括私有
* 外部类要访问内部类的成员，必须要创建对象





##### 1.2成员内部类

按照内部类在类中定义的位置不同，可以分为一下两种形式

* 在类的成员位置：成员内部类
* 在类的局部位置：局部内部类

​		成员内部类，外界创建对像使用

* 格式：外部类名.内部内名  对象名 =外部对象.内部对象；
* 范例： Outer.Inner oi=new Outer().new Inner();



##### 1.3局部内部类

局部内部类实在方法中定义的的类，所以外界是无法直接使用的，需要在方法内部创建对象并使用

该类可以直接访问外部类的成员，也可以访问方法中的变量





##### 1.4匿名内部类

**前提：** 存在一个类或者接口，这里的类可以是具体类也是抽象类

* 格式

  ~~~
     new类名或者接口名（）{
     			重写方法
     } 
  ~~~

* 范例

  ~~~
     new Inter (){
     			public void show(){
     					
     			}
     } 
  ~~~

   ***本质：是一个继承了该类或者实现了该接口的子类匿名对象***  

  

  

  

  

##### 1.5内部类在开发中的使用

~~~
	/*
	调高接口
	  */
	 public Interface Jumpping{
	 	void jump();
	 }
	
~~~

~~~
	/*
	接口操作类，里面有一个方法，方法的参数是接口名
		*/
	public class JumppingOperator{
		public void (Jumpping j){
			j.jump;
		}
	}

~~~





#### 包





### 模块

模块的基本使用步骤

* 在模块的src目录下创建一个module-info.java 的描述性文件，该文件专门定义模块名，访问权限，模块的依赖等信息     描述性文件中使用模块导出和模块依赖来进行配置并使用

* 模块中所有未导出的包都是模块私有的，他们是不能再模块之外被访问的 ，在模块下的描述性文件中配置模块导出

  模块导出格式：**exports 包名；**

* 一个模块要访问其他的模块，必须明确指出指定依赖哪些模块，未明确的模块不能访问

  在模块下的描述性文件中配置模块依赖

  模块依赖格式：**requires 模块名；**

**模块服务使用**

模块服务的使用步骤









































## API

#### 1、API 概述

**API ** 应用程序编程接口

Java API对于程序员来说就是一本可以检索查找的【字典】，是JDK官方提供给程序开发者使用类的说明文档。这些类将底层的代码封装起来，我们不需要关注这些类底层是如何实现的，我们只需要知道这些类是如何使用的。平常开发使用JDK类库的时候，通过查阅API的方式进行的。

#####  1.2API 使用练习

需求 ：按照帮助文档的使用步骤学习Scanner类的使用，并实现键盘录入一个字符串，最后输出在控制台

**注意 ：调用方法的时候，如果方法有返回值，我们使用变量接收** 









####  2、String

##### 2.1 String 概述

String 类在**java.lang** 包下，所以使用的时候不需要导包

**String** 类代表**字符串**，Java程序中的所有字符串文字（例如 ”absdajld“ ）都被实现此类的实例

也就是说，**Java程序中所有的双引号字符串，都是String类的对象** 

**字符串的特点** 

* 字符串不可变，他们的值在创建后不可更改
* 虽然String的值是不可改变的，但是他们可以被共享
* 字符串效果相当于字符数组（**char[]**），但是底层原理是字节数组（**byte[]**）

##### 2.2 String 构造方法









* public String ()						创建一个空白字符串对象，不含有任何内容
* public String (char[] chs)      根据字符数组的内容，来创建字符串对象
* public String (byte[] bys)      根据字节数组的内容，来创建字符串对象
* String s="abc";                       直接赋值的方法创建字符串对象，内容就是abc

**推荐使用直接赋值的方法** 



















##### 2.3 String 对象的特点

1). 通过new 创建的字符串对象，每一次new都会申请一个内存空间，虽然内容相同，但是地址不同

~~~
   char[] chs={'a','b','c'}
   String s1=new String(chs)；
   String s2=new String(chs)；
~~~



上面的代码中，JVM会首先创建一个字符数组，然后每一次new的时候都会重新创建一个新的地址，只不过         	s1 ，s2 参考的字符串内容是一样的

2） 以" "方式给出的字符串，只要字符序列相同（顺序和大小写相同），无论在程序代码中出现几次， JVM 都只会建立一个String对象，并在字符串池中维护

~~~
String s3="abc";
String s4="abc";
~~~

在上面的代码中，针对第一行代码，JVM会建立一个String类对象放在字符串池中，并给s3参考；

第二行则s4直接参考字符串池中的String对象，也就是说他们本质上是同一对象















##### 2.4字符串的比较

使用**= =**作比较

* 基本类型：比较的是数值是否相同
* 引用类型：比较的是地址值是否相同

字符串是对象，他比较内容是否相同，是通过一个方法来实现的，这个方法是：**equals()**

* public boolean equals(Object anObject):将该字符串与指定的对象进行比较。由于我们比较的是字符串对象，所以参数直接传递一个字符串

**line.charAt()可以访问字符串中的单个字符，类似于数组的访问** 







##### 2.5 通过帮助文档查看String中的方法

public boolean equals(Object anObject )         比较字符串的内容，严格区分大写小写（用户名和密码）

public char charAt (int index)							  返回指定索引处的char值

public int length（）                                             返回该字符串的长度





#### 3. StringBuilder 

​		如果对字符串进行拼接操作，每次拼接，都会构建一个新的String对象，既耗时，又浪费内存空间，而这些操作还不可以避免。**但是** 我们可以通过Java提供的**StringBuilder**类来解决这个问题

##### 3.1StringBuilder概述

StringBulider是一个可变的字符串类，我们可以把它看成是一容器

这里的可变指的是 StringBuilder 对象中的内容是可变的







##### 3.2StringBuilder 构造方法

public StringBuilder()                    创建一个空白的可变的字符串对象，不含有任何内容

public StringBuilder(String str)    根据字符串的内容，来创建可变的字符串对象









##### 3.3 StringBuilder 的添加和反转方法

* public StringBuilder append(任意类型)				添加数据，并返回对象本身

~~~ 
	 StringBuilder sb=new  StringBuilder();
	 sb.append("hello");
	 sb.append("world");
	 sout(sb);//输出的是helloworld
~~~





* public StringBuilder revese()								 返回相反的字符序列

~~~
	StringBuilder sb=new  StringBuilder("helloworld");
	sb.reverse();
	sout(sb);//输出的是dlrowolleh
~~~









##### 3.4 StringBuilder和String相互转换

**1. StringBuilder转换为String** 

​		 public String **toString ()**; 通过toString（）；就可以把StringBuilder转换为String

~~~
	StringBuilder sb = new StringBuilder();
	sb.append("hello");
	String s=sb;//这个事错误的做法，两个对像的类型不一样
	
	String s = sb.toString();
	sout(s)//输出的内容是helloworld
~~~









**2.String转换为StringBuilder**

​		public **StringBuilder(String s)**: 通过构造方法就可以实现把String转换为StringBuilder

~~~
	String s="hello";
	StringBuilder sb=s;//错误的
	
	StringBuilder sb=new StringBuilder(s);
	sout(s);//输出的内容是hello；
~~~



#### 4、Math

##### 4.1 Math类概述

Math包含执行基本数字运算的方法









##### 4.2 Math类的常用方法

* public static int abs (int a)      			返回参数的绝对值

  ~~~
    		System.out.println(Math.abs(88));//输出88
          System.out.println(Math.abs(-88));//输出88
  ~~~



* pubic static double ceil (double a)    返回大于或等于参数的最小double值，等于一个整数

  ~~~
     		System.out.println(Math.ceil(12.34));//输出13.0
          System.out.println(Math.ceil(12.56));//输出13.0
  ~~~

  

* public static double floor(double a)  返回小于或等于参数的大double值，等于一个整数

  ~~~
  		System.out.println(Math.floor(12.34));//输出12.0
          System.out.println(Math.floor(12.56));//输出12.0
  ~~~

  

* public static void int round (float a)   按照四舍五入返回最接近参数的int

  ~~~
   		System.out.println(Math.round(12.34F));//输出12
          System.out.println(Math.round(12.56F));//输出13
  ~~~

  

* public static int max(int a,int b)           返回两个int值中的较大值

  ~~~
  		 System.out.println(Math.max(66,88));//输出88
  ~~~

  

* public static int min(int a,int b)           返回两个int值中的较小值

  ~~~
  
          System.out.println(Math.min(66,88));//输出66
  ~~~

  

* public static double pow(double a,double b)返回a的b次幂

  ~~~
  		System.out.println(Math.pow(2.0,3.0));//输出8.0
          System.out.println(Math.pow(3.0,2.0));//输出9.0
  ~~~

  

* public static double random() 返回double的正值，[0.0,1.0]

  ~~~
  		System.out.println(Math.random());//输出一个0.0到1.2之间的double类型的随机数
  ~~~

#### 5、System

##### 5.1 System类概述

System包含几个有用的类字段和方法，它不能被实例化







##### 5.2 System类的常用方法

* public static void exit (int  status)

  ~~~
  		System.exit(0);//终止Java虚拟机
  ~~~

  

* public static long currentTimeMillis()  返回当前时间与1970年的时间差(以毫秒为单位)

  ~~~
  	System.out.println(System.currentTimeMillis()*1.0/1000/60/60/24/365);
  ~~~

  









#### 6、Object类

##### 6.1 Object类的概述

Obeject是类层次结构的根，每个类都可以将Object作为超类。所有的类都直接或者间接的继承自该类

**构造方法：** public Object()       顶级父类只有无参构造方法

**ctrl+B可以查看选中方法的源码**

在创建的类中重写toString方法，可以简单明了清晰的显示出对象的内容







##### 6.2 Object 类的常用方法

* public String toString()    返回对象的字符串表达形式。建议所有子类重写该方法，自动生成
* public boolean equals(Object obj)计较对象是否相等。默认比较地址，重写可以比较内容，自动生成

#### 7、Array

##### 7.1 冒泡排序

*  如果有n个数据进行排序，总共需要比较n-1次

* 每一次比较完毕，下一次的比较就会少一个数据参与

  ~~~
   			for (int i = 0; i < a.length-1; i++) {
              for (int j = i ; j < a.length - i - 1; j++) {
                  if (a[j] > a[j + 1]) {
                      int temp = a[j];
                      a[j] = a[j + 1];
                      a[j + 1] = temp;
              			    }
            			  }
      	    }
  ~~~

  

##### 7.2 Array类的概述和常用方法

Array类包含用于操作数组的各种方法

* public static String toString (int[] a)  返回数组的内容的字符串形式

  

* public static void sort(int[] a)返回按照数字顺序排列指定的数组

  ~~~
   int[] a={12,54,989,5,526};
          System.out.println("排序前："+ Arrays.toString(a));
          //输出排序前：[12, 54, 989, 5, 526]
          Arrays.sort(a);
          System.out.println("排序后："+ Arrays.toString(a));
          //输出排序后：[5, 12, 54, 526, 989]
          
  ~~~

**工具类的设计思想：**

* 构造方法用private
* 成员用public static修饰









#### 8、基本类型包转类

##### 8.1 基本类型包装类概述

将基本数据类型封装成对象的好处在于可以在对像中定义更多的功能方法操作该数据

常用的操作之一：用于基本数据类型与字符串之间的转换

基本数据类型					包装类

​		**byte							Byte**

​	   **short						   Short**

​		**int 							   Integer**

​		**long							 Long**

​		**float							 Float**

​		**double						 Double**

​		**char							  Character**

​		**boolean						Boolean**

##### 8.2 Integer类的概述和使用

**Integer** : 包装一个对象中的原始类型int的值

* public Integer (int value)							根据int值创建Integer对象**(过时)**

* public Integer(String s)								根据String值创建Integer对象**(过时)**

* public static Integer valueOf(int i)				返回表示指定的int值的Integer 实例

  ~~~
  		Integer i3=Integer.valueOf(100);
          System.out.println(i3);//输出100
  ~~~

  

* public static Integer valueOf(String s)			返回一个保存指定值的Integer 对象 String

  ~~~
  		Integer i4=Integer.valueOf("100557");//这里的参数只能含有数字
          System.out.println(i4);//输出100557；
  ~~~

  

##### 8.3 int 和 String的相互转换

基本类型包装类最常见的操作就是：用于基本类型和字符串之间的相互转换

###### 1. int 转换为String

public static String **valueOf(int i)** 返回int参数的字符串形式表示形式。该方法是String类中的方法



###### String 转换为int

public static int **parseInt(String s)** :将字符串解析为int类型。该方法是Integer类中的方法

##### 案例：字符串中的数据排序



需求：有一个字符串：“91 27 46 38 50”，请写程序实现最终输出的结果是："27 38 46 50 91"

思路：

1. 定义一个字符串

2. 把字符串中的数字数据存储到一个int类型的数组中

   * 得到字符串中每一个数字数据

     public String[] split(String regex)

   * 定义一个int类型的数组，把String[]数组中的每一个元素存储到int数组中

     public static int parseInt(String s)

3. 对 int 数组进行排序
4. 把排序后的数组中的元素进行拼接得到一个字符串，这里的拼接采用StringBuilder来实现
5. 输出结果

~~~
		package itheima_03;

import java.util.Arrays;

public class sort {
    public static void main(String[] args) {
        String s = "91 27 46 38 50";
        String[] Array = s.split(" ");
        // for(int i=0;i<Array.length;i++){
        //    System.out.println(Array[i]);
        // }
        int[] a = new int[Array.length];
        for (int i = 0; i < Array.length; i++) {
            a[i] = Integer.parseInt(Array[i]);
        }
        Arrays.sort(a);
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < a.length; i++) {
            if (i == a.length - 1) {
                sb.append(a[i]);
            } else {
                sb.append(a[i]).append(" ");
            }
        }
        String result = sb.toString();
        System.out.println(result);
    }
}

~~~



##### 8.4自动装箱和拆箱

* 装箱：把基本数据类型转换为对应的包装类类型
* 拆箱：把包装类类型转换为对应的基本数据类型

~~~
Integer i=100;		//自动装箱
i+=200;			//i=i+200;i+200  i+200自动拆箱;  i=i+200;是自动装箱
~~~

**注意：** 在使用包装类类型的时候，如果操作，做好先判断是否为nill

​				**只要是对象，在使用前就必须进行不为null的判断** 









#### 9、日期类

##### 9.1 Date类叙述和构造方法

Date 代表了一个待定的时间，精确到毫秒



* public Date()     分配一个Date对象，并初始化，以便它代表它被分别配的时间，精确到毫秒
* public Date(long date) 分配一个Date对象，并将其初始化为表示从基准时间起指定的毫秒数

##### 9.2 Date类常用的方法

* public long getTime ()	获取的是日期对象从1970年一月一日 00 ：00 ：00 到现在的毫秒
* public void setTime (long time)    设置时间，给的是毫秒值



##### 9.3 SimpleDateFormat 类

###### 9.31  SimpleDateFormat 类概述

 	SimpleDateFormat 类是一个具体的类，用于以区域设置敏感的方式格式化和解析日期。我们重点学习**日期格式化和解析**



日期和时间格式由日期和时间模式字符串指定，在日期和时间模式的字符串中，从**'A'**到**'Z'**以及从**'a'**到**'z'**引号的字母被解释为表示日期或时间的字符串的组件的模式字母

* **y	**				年
* **M	**			   月	
* **d     **               日
* **H  **                  时
* **m  **                 分
* **s **                    秒





####  9.4 SimpleDateFormat 的构造方法

* public SimpleDateFormat () 构造一个 SimpleDateFormat ，使用默认模式和日期格式
* pubic  SimpleDateFormat (String pattern) 构造一个 SimpleDateFormat 使用给定的模式和默认的日期格式



  ##### 9.5  SimpleDateFormat  格式化和解析日期

###### 1 、格式化（从Date到String）

public final String **format(Date date)**:将日期格式化称日期/时间字符串

```
  Date d=new Date();
       // SimpleDateFormat sdf=new SimpleDateFormat();
            SimpleDateFormat sdf=new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
        String s=sdf.format(d);
        System.out.println(s);

```



###### 2、解析（从String 到Date）

public Date **parse(String sourse):**从给定字符串的开始解析文本用以生成日期







##### 9.6 Calendar 类概述

Canlendar 为某一时刻和一组日历字段之间的转换提供了一些方法，并为操作日历字段提供了一些方法

Canlendar 提供了一个类方法 getlnstance 用于获取 Canlendar 对象，其日历字段已使用当前日期和时间初始化：

Canlendar rightNow =Canlendar.getlnstace ();

##### 9.7 Canlendar 的常用方法

* public int get(int filed) 返回给定日历字段的值
* public abstract void add (int filed ,int amount)  根据日历规则，将指定的时间量添加或减去给定日历的字段
* public final void sent (int year ,int month,int date)  设置当前日历的年月日

 



## 集合

### 集合基础

##### 1.1 集合概述

集合的特点： 提供一种存储空间可变的存储类型，存储空间的数据容量可以发生改变

首先学第一个ArryList

**ArrayList< E >**:

* 可调整大小的数组实现
* < E >：是一种特殊的数据类型，泛型。
* ArrayList的使用需要导包 **import java.util.ArrayList;**
* 使用的时候，我们使用引用数据类型替换即可

​					举例：ArrayList< String >,ArrayList< Student >









##### 1.2ArrayList构造方法和添加方法

* public ArrayList() 	创建一个空的集合对象

  ~~~
  //创建一个名字为array的集合
          ArrayList<String> array = new ArrayList<String>();
  ~~~









* public boolean add(E e)  将指定的元素追加到此集合的末尾

  ~~~
  	    System.out.println(array);
          //输出的是[]
          //System.out.println(array.add("helloworld"));
          //输出的是ture  原因是array.add("helloworld")执行成功
          array.add("hello");
          array.add("world");
          System.out.println("array："+array);
          //输出的是array:[hello,world]
  ~~~







* public void add（int index ，E element）将次集合中的指定元素插入指定的位置

  ~~~
  	  //将java添加到集合的0号位位置，该位置及以后的元素依次向后移动一个索引
  	  array.add(0,"java ");
          System.out.println(array);
          //输出的结果是[java , hello, world]
      }//不能在大于集合长度的索引位置添加元素，会产生集合的索引越界
  ~~~







##### 1.3 ArrayList集合常用方法

* public boolean remove(Object o)删除指定的元素，并返回删除是否成功

  ~~~
    ArrayList<String> array=new ArrayList<>();//创建集合
          array.add("hello");
          array.add("world");
          array.add("java");
          System.out.println(array);//输出[hello, world, java]
          System.out.println(array.remove("java"));//输出ture
          System.out.println(array);//输出[hello, world]
          System.out.println(array.remove("javaabc"));
          //输出false：javaabc在集合中不存在
  ~~~





* public E remove(int index)删除指定索引位置的元素，返回被删除的元素

  ~~~
  	ArrayList<String> array=new ArrayList<>();//创建集合
          array.add("hello");
          array.add("world");
          array.add("java");
          System.out.println(array);//输出[hello, world, java]
          System.out.println(array.remove(2));//输出java
          System.out.println(array);输出[hello, world]
  			如果删除的时候输入的索引不存在  程序会报错
  
  ~~~



* public E set(int index,E element)修改指定索引处的元素，返回被修改的元素

  ~~~
  
          ArrayList<String> array=new ArrayList<>();//创建集合
          array.add("hello");
          array.add("world");
          array.add("java");
          System.out.println(array);//输出[hello, world, java]
          System.out.println(array.set(2,"javaee"));//输出java
          System.out.println(array);//输出[hello, world, javaee]
  
  ~~~



* public E get(int index)返回指定索引位置的元素

  ~~~
  
        
          ArrayList<String> array = new ArrayList<>();//创建集合
          array.add("hello");
          array.add("world");
          array.add("java");
          System.out.println(array);//输出[hello, world, java]
          System.out.println(array.get(0));//输出hello
          System.out.println(array.get(1));//输出world
          System.out.println(array.get(2));//输出java
          System.out.println(array);
  
  ~~~

  

* public int size()返回集合中元素个数

  ~~~
  	
          ArrayList<String> array = new ArrayList<>();//创建集合
          array.add("hello");
          array.add("world");
          array.add("java");
          System.out.println(array);//输出[hello, world, java]
          System.out.println(array.size());//输出3
          System.out.println(array);
  ~~~







###  集合进阶

#### 1、Collection

##### 1.1 集合的概述和使用

**Collection 集合概述**

* 是单列集合的顶层接口，它表示一组对象，这些对象称成为Clloection的元素
* JDK 不提供此类接口的任何直接实现，他提供更具体的子接口（如Set和List）

**创建 Collection 集合的对象**

* 多态的方式
* 具体的实现类 ArrayList

##### 1.2 Collection 集合的常用方法

*  boolean add(E,e)						添加元素

  ~~~
    			co.add("hello");
              co.add("world");
          System.out.println(co);
          //输出[hello, world]
  ~~~

  

* boolean remove(Object o)        从集合中移除指定的元素

  ~~~
  	    co.add("hello");
          co.add("world");
          co.add("java");
          co.remove("world");//删除world
          System.out.println(co);
          //输出的结果是  [hello, java]
  ~~~

  

* void clear()                                    清空集合中的方法

  ~~~
  		co.add("hello");
          co.add("world");
          co.add("java");
          System.out.println(co);//输出[hello,world,java]
          co.clear();//清空
          System.out.println(co);//输出[]
  ~~~

  

* boolean contains(Objects o)      判断集合中是否存在指定的元素

  ~~~
  		co.add("hello");
          co.add("world");
          co.add("java");
          System.out.println(co.contains("world"));
          //判断"world"是否存在       输出true
  ~~~

  

* boolean isEmpty()                        判断集合是否为空

  ~~~
  		co.add("hello");
          co.add("world");
          co.add("java");
          System.out.println(co);//输出[hello,world,java]
          co.clear();//清空
          System.out.println(co.isEmpty());//输出 true
  ~~~

  

* int size()                                          集合的长度，也就是集合元素的个数

  ~~~
  		co.add("hello");
          co.add("world");
          co.add("java");
          System.out.println(co.size());//输出3
  ~~~

  

##### 1.3 Collection集合的遍历

​	**Iterator** :迭代器，集合的专用遍历方式

* Iterator < E >iterator :返回此集合中元素的迭代器，通过集合的iterator()方法得到
* 迭代器是通过集合的 iterator()方法得到的，所以我们说它是依赖于集合而存在的

​	**Iterator中常用的方法**

* E next():返回迭代中的下一个元素

* boolean hasNext ():如果迭代中具有更多的元素，则返回true

  ~~~
  		Collection<String> c=new ArrayList<String>();
          c.add("hello");
          c.add("world");
          c.add("java");
          Iterator<String> it=c.iterator();
          while( it.hasNext()){
              System.out.println(it.next());
          }//输出的结果是hello   world  java
          }
  ~~~



























#### 2、List

##### 	2.1 List集合概述和特点

​				**概述**

* 有序集合(也成为序列)，用户可以精确控制列表中的每一个元素的插入位置。用户可以通过整数索引访问元素，并搜索列表中的元素

* 与Set集合不同，列表通常可允许重复的元素

  ​		**特点**

* 有序：存储和取出的元素顺序一致

* 可重复：存储的元素可以重复



##### 2.2 List集合特有的方法

* void add(int index,E element)		在此集合中的指定位置插入指定的元素

  ~~~
  		list.add("hello");
          list.add("world");
          list.add("java");
          System.out.println(list);//输出[hello, world, java]
          list.add(1,"javaee");
          System.out.println(list);//输出[hello, javaee, world, java]
  ~~~

  

* E remove(int index)                          删除指定索引处的元素，返回被删除的元素

  ~~~
  		list.add("hello");
          list.add("world");
          list.add("java");
          list.remove(1);
          System.out.println(list);//输出[hello, java]
  ~~~

  

* E set(int index,E element)                修改指定索引处的元素，返回被修改的元素

  ~~~
  		list.add("hello");
          list.add("world");
          list.add("java");
          System.out.println(list.set(1,"javaee"));//输出world
          System.out.println(list);//输出[hello, javaee, java]
  ~~~

  

* E get(int index)                                    返回指定索引处的元素

  ~~~
   		 list.add("hello");
          list.add("world");
          list.add("java");
          System.out.println(list.get(1));//输出world
  
  ~~~

  

##### 2.3 ListIterator列表迭代器

* 通过List 集合的listIterator()方法得到，所以说它是List集合特有的迭代器
* 用于允许程序员沿任一方向遍历列表的列表迭代器，在迭代期间修改列表，并获得列表迭代器的当前位置



ListIterator中的常用方法

* E next():			返回迭代中的下一个元素

* boolean hasNext():  如果迭代具有更多的元素则返回true

* E previous():       返回列表中的上一个元素

* boolean hasPrevious():  如果此列表迭代器在相反方向遍历列表时具有更多元素，则返回true

  ​    **跟上面的Iterator用法差不多**

* 

##### 2.4 增强for循环

增强for ：简化数组和Collection集合的遍历

* 实现Iterator接口的类允许其对象成为增强for语句的目标
* 内部原理是一个Iterator迭代器

增强for的格式

* 格式

  for（元素数据类型 变量名：数组或者Collection集合）{

  ​				//在此处使用变量即可，该变量就是元素

  }

* 范例

  int[] arr={1,2,3,4,5}

  for(int i : arr){

  System.out.println(i);

  }

##### 2.5 常见数据结构之栈

* 数据进入栈模型的过程称为：**压/进栈**

* 数据离开栈模型的过程称为：**弹/出栈**

  **栈是一种数据先进后出的模型**

##### 2.6 常见数据结构之队列

* 数据从后端进入队列模型的过程称为：**入队列**

* 数据从前端离开队列模型的过程称为：**出队列**

  **队列是一种数据先进先出的模型**



##### 2.7 常见数据结构之数组

数组是一种**查询快，增删慢**的模型

* 查询数据通过索引定位，查询任意数据耗时相同，**查询效率高**
* 删除数据时，要将原始数据删除，同时后面每一个数据迁移，**删除效率低**
* 添加数据时，添加位置后的每个数据后移，在添加元素，**添加效率极低**

##### 2.8 常见数据结构之链表

链表是一种**增删快**的模型

连表示一种**查询慢**的模型











##### 2.9 List 集合的子类特点

List 集合常用的子类：ArrayList ,LinkedList

* ArrayList：底层数据结构是数组，查询快，增删慢

* LinkedList：底层数据结构是链表，查询慢，增删快

  





##### 2.10 LinkLists集合特有的功能

* public void addFirst(E,e)



* public void addLast(E,e)



* public E getFirst()   



* public E removeFirst()



* public E removeLast()



















#### 3、Set

##### 3.1 Set集合概述和特点

Set集合特点

* 不包含重复元素的集合
* 没有带索引的方法，所以不能用普通for循环







##### 3.2哈希值

哈希值：是JDK根据对象地址或者字符串或者数字算出来的int类型的数值

* Object类中有一个办法可以获取**对象的哈希值**

public int hashCode() ;返回对象的hash值





##### 3.3 HashSet集合的概述和特点

HashSet集合特点

* 底层数据结构就是哈希表
* 对集合的迭代顺序不做任何保证也就是说不保证存储和取出的元素顺序一致
* 没有带索引的方法，所以不能使用普通for循环遍历
* 由于是Set集合，所以是不包含重复元素的集合















##### 3.4 LinkedHashSet 集合概述和特点

LinkedHashSet集合特点

* 哈希表和链表实现的Set接口，具有可预测的迭代次序

* 由链表保证元素有序，也就是说元素的存储和取出的顺序是一致的

* 由哈希表保证元素唯一，也就是说没有重复的元素

  











##### 3.5 TreeSet 集合概述和特点

TreeSet集合特点

* 元素有序，这里的顺序不是指存储和取出的顺序，而是按照一定的规则进行排序，具体的排序方式取决于构造方法

  TreeSet（）：根据其元素的自然顺序进行排序

  TreeSet(Comparator comparator):根据指定的比较器进行排序

* 没有带索引的方法，所以不能使用普通的for循环遍历
* 由于是Set集合，所以不包含重复元素的集合

TreeSet集合练习

* 存储整数并遍历

~~~
  		TreeSet<Integer> ts =new TreeSet<Integer>();
  		//因为集合存储的是引用类型 ，所以因该是用对应的包装类型
        ts.add(10);//自动装箱
        ts.add(30);
        ts.add(20);
        ts.add(30);
        for(int i :ts){
            System.out.println(i);//输出10，20，30
~~~









##### 3.6自然排序 Comparable 的使用

* 存储学生对象并遍历，创建TreeSet集合使用**无参构造方法**
* 要求：按照年龄从小到大排序，年龄相同时，按照姓名的字母顺序排序







**结论**

* 用TreeSet集合存储自定义对象，无参构造方法使用的是自排序对元素进行排序的
* 自然排序，就是让元素所属的类实现Comparable接口，重写compareTo(To)方法
* 重写方法时，一定要注意排序规则按照要求的主要条件和次要条件来写







##### 3.7比较排序 Comparator 的使用

* 存储学生对象并遍历，创建TreeSet集合使用带参构造方法
* 要求：按照年龄从小到大排序，年龄相同时，按照姓名的字母顺序排序

~~~
 TreeSet<Student > ts =new TreeSet<Student>(new Comparator<Student>() {
            @Override
            public int compare(Student s1, Student s2) {
                int num =s1.getAge()-s2.getAge();
                int num2= num==0? s1.getName().compareTo(s2.getName()):num;
                return num2;
            }
        });
        Student s1 =new Student("zhangsan",10);
        Student s2 =new Student("lisi",75);
        Student s3 =new Student("wangwu",5);
        Student s4 =new Student("zhaoliu",75);
        Student s5 =new Student("liqi",45);
        ts.add(s1);
        ts.add(s2);
        ts.add(s3);
        ts.add(s4);
        ts.add(s5);
        for(Student s :ts){
            System.out.println(s.getName()+","+s.getAge());
        }
    }
    //输出wangwu,5
		 zhangsan,10
 		 liqi,45
  		 lisi,75
		 zhaoliu,75

~~~



**结论** 

* 用 TreeSet 集合存储自定义对象，带参构造方法使用的是**比较器排序**对元素排序的

* 比较器排序，就是让集合构造方法接收Comparator的实现类对象，重写compare(T o1,T  o2)方法
* 重写方法时，一定要注意排序规则必须按照要求的主要条件和次要条件来写













#### 4、泛型

##### 4.1 泛型概述

 它提供了编译时类型安全监测机制。该机制允许在编译时检测到非法的类型，它的本质是参数化类型，也即是说所操作的数据类型被指定为一个参数

**参数化类型：就是将类型由原来的具体的类型参数化，然后在使用/调用时传入具体的类型**



这种参数类型可以用在类、方法、接口中，分别被称为泛型类、泛型方法、泛型接口

泛型定义格式

* <类型>：指的是一种类型的格式。这里的类型可以看成形参
* <类型1，类型2....>：指的是多类型的格式，多种类型之间用逗号隔开。
* 将来具体调用的时候给定的类型可以看成是实参，并且实参的类型只能是引用数据类型



**泛型的好处**

* 把运行期间的问题提前到了编译时期
* 避免了强制转换

##### 4.2泛型类

泛型类定义格式

* 格式：修饰符class类名<类名>{  }
* 范例：pubic class Generic < T >{  }

​			**此处的T可以是随便写的任意标识**，如常见的**T  E   K  V**等形式的参数常用于表示泛型

**泛型标记**

定义泛型时，一般采用几个标记：E、T、K、V、N、？。他们约定俗称的含义如下：

| 泛型标记 | 对应单词 | 说明                           |
| -------- | -------- | ------------------------------ |
| E        | Element  | 在容器中使用，表示容器中的元素 |
| T        | Type     | 表示普通的JAVA类               |
| K        | Key      | 表示键，例如：Map中的键Key     |
| V        | Value    | 表示值                         |
| N        | Number   | 表示数值类型                   |
| ？       |          | 表示不确定的JAVA类型           |

##### 4.3泛型方法

泛型方法的定义格式：

* 格式：修饰符<类型>返回值类型  方法名(类型  变量名){   }

* 范例：pubic < T >voids how(T t){    }

  ~~~
  public class Demo {
      public <T> void show(T t){
          System.out.println(t);
      }
      public static void main(String[] args) {
         Demo d =new Demo();
         d.show("张三");
         d.show(30);
         d.show(true);
         d.show(12.34);
      }
  }
  //输出
  张三
  30
  true
  12.34
  ~~~

  







##### 4.4 泛型接口

泛型接口定义格式： 

* 格式：修饰符interface 接口名<类型>{  }
* 范例 ：public interface Generic < T >{  }





##### 4.5类型通配符

为了表示各种泛型List的父类，可以使用类型通配符

* 类型通配符：<**?**>
* List<?>:表示元素类型位置的List，它的元素可以匹配任何类型
* 这种带通配符的List仅表示它是各种泛型的父类，并不能把元素添加到其中

如果我们不希望List<？>是任何类型List的父类，只希望它代表某一类泛型List的父类，可以使用通配符上限

* 类型通配符上限：**< ? extends 类型>**

* List< ?extends Number>:它表示的类型是Number或者其子类型

  也可以指定类型通配符的下限

* 类型通配符下限   ：**< ? super>**

* List < ?super Number>:它表示的类型是Number或者其父类型

  ~~~
  import java.util.ArrayList;
  import java.util.List;
  //Object->Number->Integer
  public class Demo {
      public static void main(String[] args) {
          List<?> l1 =new ArrayList<Object>();
          List<?> l2=new ArrayList<Number>();
          List<?> l3=new ArrayList<Integer>();
          System.out.println("_________________");
          
    //      List<? extends Number> l4=new ArrayList<Object>();
             List<? extends Number> l4=new ArrayList<Number>();
             List<? extends Number> l5=new ArrayList<Number>();
             List<? extends Number> l6=new ArrayList<Integer>();
  
          System.out.println("____________________");
          
          
          List<? super Number> l7 =new ArrayList<Object>();
          List<? super Number> l8 =new ArrayList<Number>();
         // List<? super Number> l9 =new ArrayList<Integer>();
          
          
       }
  }
  
  ~~~

  

##### 4.6可变参数

可变参数又称参数的个数可变，用作方法的形参出现，那么方法参数个数就是可变的了

* 格式：修饰符    返回值类型   方法名（数据类型...变量名）{}
* 范例：public static void int sum(int...a){}

~~~
	public class Demo {
    public static void main(String[] args) {

        System.out.println(sum(10,20));
        System.out.println(sum(10,20,30));
        System.out.println(sum(10,20,30,40));
        System.out.println(sum(10,20,30,40,50));
        System.out.println(sum(10,20,30,40,50,60));
    }
    public static  int sum(int ...a){
        int sum=0;
        for(int i:a){
            sum+=i;
        }
        return  sum;
    }
}

~~~



**可变参数注意事项：**

* 这里的变量其实是一个数组
* 如果一个方法有多个变量，包含可变参数，**可变参数放在最后**













##### 4.7可变参数的使用

Arrays工具类中有一个静态方法：

* public static < T > List< T >asList(T...a) :返回由指定数组支持的固定大小的列表

* 返回的集合不能做增删操作，可以做修改操作





List接口中有一个静态方法

* public static < E >List< E >of(E...elements):返回包含任意数量元素的不可变列表
* 返回的集合不能做增删改操作



Set接口中有一个静态方法

*  pubic static < E >Set< E > of(E...elements):返回一个包含任意元素的不可变集合
* 再给元素的时候，不能给重复元素
* 返回的集合不能做增删操作，没有修改的方法











#####  4.8 泛型局限性和常见错误



泛型主要用于编译阶段，编译后生成的字节码class文件不包含泛型中的类型信息。 类型参数在编译后会被替换成Object，运行时虚拟机并不知道泛型。因此，使用泛型时，如下几种情况是错误的：

1. 基本类型不能用于泛型

   `Test<int> t;` 这样写法是错误，我们可以使用对应的包装类`Test<Integer> t ;`

2. 不能通过类型参数创建对象

   `T elm = new T();` 运行时类型参数`T`会被替换成`Object`，无法创建T类型的对象，容易引起误解，java干脆禁止这种写法。













#### 5、Map

##### 5.1 Map集合的概述和使用

Map集合概述

* Interface Map<K,V>    K:键的类型；      V:值的类型

* 将建映射到值的对象；不能包含重复的键；每个键可以映射到最多一个值

* 举例 ：学生的学号和姓名

  ​				itheima001     张三

  ​				itheima002    李四

​					   itheima003    王五

创建Map集合对象

* 多态的方式
* 具体的实现类HashMap



























##### 5.2 Map 集合的基本功能

* V put(K key,V value)           添加元素

  

* V remove(Object key)        根据键删除键值对元素

  

* void clear ()                          移除所有的键值对元素

  

* boolean containsKey(Object key)           判断集合是否包含指定的键

  

* boolean containsValue(Object value)    判断集合是否包含指定的值

  

* boolean isEmpty()               判断集合是否为空

  

* int size()                               集合的长度，也就是集合中键值对的个数

​                         











##### 5.3 Map 集合的获取功能

* V get (Object key )                根据键获取值

  ~~~
  		 Map<String,String> map=new HashMap<String,String>();
          map.put("张无忌","赵敏");
          map.put("郭靖","黄蓉");
          map.put("杨过","小龙女");
          System.out.println(map.get("张无忌"));//输出赵敏
          System.out.println(map.get("张三丰"));//输出null
  ~~~

  

* Set< K >keySet()                   获取所有键得集合

  ~~~
  		 Map<String,String> map=new HashMap<String,String>();
          map.put("张无忌","赵敏");
          map.put("郭靖","黄蓉");
          map.put("杨过","小龙女");
          Set<String> keyset=map.keySet();
          for(String s:keyset){
              System.out.println(s);
          }	//输出：
         			 杨过
  				 郭靖
  				 张无忌
  ~~~

  

* Collection< V >values()       获取所有值得集合

  ~~~
  		Map<String,String> map=new HashMap<String,String>();
          map.put("张无忌","赵敏");
          map.put("郭靖","黄蓉");
          map.put("杨过","小龙女");
          Collection<String> values =map.values();
          for(String s:values){
              System.out.println(s);
          }	//输出：
          		小龙女
  			    黄蓉
  				赵敏
  ~~~

  

* Set< Map.Entry< K,V >>entrySet( )  获取所有键值对象的集合





















##### 5.4 Map集合的遍历（方式一）

我们存储的元素都是成对出现的，我们把Map看成是一个夫妻对的集合

遍历思路：

* 把所有丈夫给集中起来
* 遍历丈夫的集合，获取到每一个丈夫
* 根据丈夫去找对应的妻子

转化为Map集合中的操作：

* 获取所有键的集合。用keySet（）方法实现

* 遍历键的方法，获取到每一个键。用增强for实现

* 根据键去找值。用get（Object key）方法实现

  ~~~
   		Map<String,String> map=new HashMap<String,String>();
          map.put("张无忌","赵敏");
          map.put("郭靖","黄蓉");
          map.put("杨过","小龙女");
          Collection<String> values =map.values();
          //获取集合所有键的方法
          Set<String> keySet =map.keySet();
          //遍历键的集合，获取到每一个键。用增强for实现
          for(String key:keySet){
              //根据键去找值。用get（Object key)去实现
              String value =map.get(key);
              System.out.println(key+","+value);
  ~~~

  









##### 5.5 Map集合的遍历（方式2）

把Map看成是一个夫妻对的集合

遍历思路：

* 获取到所有结婚证的集合
* 遍历结婚证的集合，得到每一个结婚证
* 根据结婚证获取丈夫和妻子



转化为Map集合中的操作：

* 获取所有键值对对象的集合

  ​			**Set< Map.Entry<K,V>>entrySer():获取所有键值对对象的集合**

* 遍历键值对对象的集合，得到每一个键值对对象

  用增强for实现，得到每一个**Map.Entry**

* 根据键值对对象获取键和值

  用getKey（）得到键

  用getValue（）得到值

  ~~~
   		Map<String,String> map=new HashMap<String,String>();
          map.put("张无忌","赵敏");
          map.put("郭靖","黄蓉");
          map.put("杨过","小龙女");
          Collection<String> values =map.values();
          Set<Map.Entry<String,String>> entrySet =map.entrySet();
          for(Map.Entry<String,String> me:entrySet){
              String key=me.getKey();
              String value=me.getValue();
              System.out.println(key+","+value);
          }//输出：
         		 杨过,小龙女
  			 郭靖,黄蓉
  			 张无忌,赵敏
  ~~~

  







#### 6、Collections

##### 6.1 Collections 概述和使用

Collections类的概述

* 是针对集合操作的工具类



Collections类的常用操作方法

* public static < T extends Comparable<? super T>> void sort< List< T > list>  将指定的列表按升序排序

  ~~~
  List<Integer>  list=new ArrayList<Integer>();
          list.add(30);list.add(20);list.add(100);list.add(40);list.add(50);
          System.out.println(list);//输出[30, 20, 100, 40, 50]
          Collections.sort(list);
          System.out.println(list);//输出[20, 30, 40, 50, 100]
  ~~~

  

* public static void reverse(List<?> list) 反转指定列表中元素的顺序

  ~~~
  List<Integer>  list=new ArrayList<Integer>();
          list.add(30);list.add(20);list.add(100);list.add(40);list.add(50);
          System.out.println(list);//输出[30, 20, 100, 40, 50]
          Collections.sort(list);
          System.out.println(list);//输出[20, 30, 40, 50, 100]
          System.out.println(list);//输出[100, 50, 40, 30, 20]
  ~~~

  

* public static void shuffle(List<?> list): 使用默认的随机源随机排列指定的列表

  ~~~
  
  ~~~
## I/O

### 1、File

#### 1.1 File类概述和构造方法

File：他是文件和目录路径名的抽象表示

- 文件和目录是可以通过File封装成对象的
- 对于File而言，其封装的·并不是一个真正存在的文件，仅仅是一个路径而已。它是可以真实存在的，也是可以不存在的。将来是要通过具体的操作把这个路径的内容转化为具体存在的

构造方法

* File(String pathname)：通过将给定的路径名字符串转换为抽象路径名来创建新的File实例

  		File f1 =new File("D:\\itcast\\java.txt");
  	      System.out.println(f1);//输出D:\itcast\java.txt
* File(String parent ,String child)：从父路径名字符串和子路径名字符串创建新的File实例

  		File f2=new File("D:\\itcast","java.txt");
  	      System.out.println(f2);//输出D:\itcast\java.txt
* File(file parent,String child)：从父抽象路径名和子抽象路径名字符串创建新的File实例

  		File f3=new File("D:\\itcast");
  	      File f4=new File(f3,"java.txt");
  	      System.out.println(f4);//输出D:\itcast\java.txt
#### 1.2 File类创建功能

* public boolean creatNewFile()：  当具有该名字的文件不存在时，创建一个由该抽象路径名命名的新空文件

  _如果文件不存在，就创建文件，并返回true_

  _如果文件存在，就不创建文件，并返回false_

* public boolean mkdir()：创建由此抽象路径名命名的目录

  _如果目录不存在，就创建目录，并返回true_

  _如果目录存在，就不创建目录，并返回false_

* public boolean mkdirs()：创建由此抽象路径名命名的目录，包括任何必须但不存在的父目录

  _如果多级目录不存在，就创建多级目录，并返回true_

  _如果多级目录存在，就不创建多级目录，并返回false_













#### 1.3 File类删除功能

* public boolean delete()：删除由此抽象路径表示的文件或目录



绝对路径和相对路径的区别

* 绝对路径：**完整的路径名，**不需要任何其他的信息就可以定位它所表示的的文件。例如：**D: \\\FileTest\\\java.txt**
* 相对路径：必须使用取自其他路径名的信息进行解释。例如：**myFile\\\java.txt**



删除目录时的注意事项：

* 如果一个**目录中有内容**（目录，文件），**不能直接删除**。应该先删除目录中的内容，最后才能删除目录









#### 1.4File类判断和获取功能

* public boolean isDirectory()：测试此抽象路径名表示的File是否为目录
* public boolean isFile()：测试此抽象路径名表示的File是否为文件
* public boolean exists()：测试此抽象路径名表示的File是否存在
* public String getAbsolutePath()：返回此抽象路径名的绝对路径名字符串
* public String getPath()：将此抽象路径名转换为路径名字符串
* pubic String getName()：返回此抽象路径名表示的文件或目录的名称
* public String[] list()：返回此抽象路径名表示的目录中的文件和目录的名称字符串数组
* pubic File[]  listFile()：返回此抽象路径表示的目录中的文件和目录的File对象数组











#### 1.5 递归

递归概述：以编程的角度来看，递归指的是方法定义中调用方法本身的现象



递归解决问题的思路：

把一个复杂的问题层层转化为一个**与原问题相似的规模较小**的问题来求解

递归策略只需**少量的程序**就可描述出解题过程所需要的多次重复计算



递归解决问题要找到两个内容：

* 递归出口：否则会出现内存溢出
* 递归规则：与原问题相似的规模较小的问题

























### 2、字节流

#### 2.1 IO流概述和分类

IO流概述:

* IO:输入/输出(Input/Output)

* 流：是一种抽象的概念，是对数据传输的总称。也就是说数据在设备间的传输称为流，流的本质是数据传输

* IO流就是用来处理设备间的传输问题的

                 常见的应用：文件复制；文件上传；文件下载；



IO流分类：

* 按数据的流向

  		输入流：读数据：数据流向是数据源到程序
		
  		输出流：写数据：数据流向是程序到目的地

* 按数据类型来分

  	字节流：以字节为单位获取数据，一般以Stream结尾
		
  							字节输入流；字节输出流
		
  	字符流：以字符单位获取数据，命名上以Reade/Writer结尾
		
  							字符输入流；字符输出流

* 如果数据通过Window自带的记事本打开，我们可以读懂里面的内容，就用字符流，否者用字节流。不知的情况下用字节流







#### 2.2 字节流写数据

 字节流抽象基类

* InputStream ：这个抽象类是表示字节输入流的所有类的超类
* OutputStream：这个抽象类是表示字节输出流的所有类的超类
* 子类名特点：子类名称都是以其父类名作为子类名的后缀





FileOutputStream：文件输出流用于将数据写入File

* FileOutputStream（String name）：创建文件输出流以指定的名称写入文件





使用字节输出流写数据的步骤：

* 创建字节输出流对象（调用系统的功能创建了文件，创建字节输出流对象，让字节输出流对象指向文件）
* 调用字节输出流对象写数据的方法
* 释放资源（关闭此文件输出流并释放与此流相关联的任何系统资源）

























#### 2.3 字节流写数据的三种方法



* void write(int b):将指定的字节写入此文件输出流，一次写一个字节数据
* void write(byte[] b): 将b.length 字节从指定的字节数组写入此文件输出流。一次写入一个字节数组的数据
* void write(byte[] b,int off,int len):将len 字节从指定的字节数组开始，从偏移量off开始写入此文件的输出流 。一次写一个字节数组的部分数据













#### 2.4 字节流写数据的两个小问题

字节流写数据如何实现换行

* 写完数据后，加换行符”\n“

字节流写数据如何实现追加写入

* public FileOutputStream(String name,boolean append)
* 创建文件输出流以指定的名称写入文件。如果第二个参数为true，则字节将写入文件的末尾而不是开头









#### 2.5 字节流写数据加异常处理

**finally**：在异常处理时提供finally块来执行所有清楚操作。比如说IO流中的释放资源

特点：被finally控制的语句一定会执行，除非JVM退出



  ~~~

try{
	可能出现异常的代码；
}catch(异常类名  变量名){
	异常处理的代码；
}finally{
	执行所有清楚操作；
}
  ~~~
#### 2.6字节流读数据

##### 2.6.1一次读一个字节的数据

需求：把文件 fos.txt 中的内容读取出来在控制台输出



FileInputStream:从文件系统中的文件获取输入字节

* FileInputStream（String name）：通过打开与实际文件的连接来创建一个FileInputStream,该文件由文件系统中的路径名name命名



使用字节输入流读数据的步骤：

1. 创建字节输入流对象
2. 调用字节输入流对象的读数据方法
3. 释放资源





##### 2.6.2一次读一个字节数组数据

需求：把文件fos.txt中的内容读出来在控制台输出

使用字节输入流读数据的步骤

1. 创建字节输入流对象
2. 调用字节输入流对象的读数据方法
3. 释放资源

~~~


public static void main(String[] args) throws IOException {
        FileInputStream fis=new FileInputStream("D:\\FileTest\\git 学习.txt");
        FileOutputStream fos =new FileOutputStream("myByteStream\\git学习.txt");
        int bys;
        while((bys=fis.read())!=-1){
            fos.write(bys);
        }
        fos.close();
        fis.close();
~~~
~~~


FileInputStream fis = new FileInputStream("myByteStream\\fos.txt");
        byte[] bys = new byte[1024];//1024及其整数倍
        int len;
        while ((len = fis.read(bys)) != -1) {
            System.out.println(new String(bys, 0, len));
        }
        fis.close();
~~~






#### 2.7 字节缓冲流

* BufferedOutputStream: 该类实现缓冲输出流。通过设置这样的输出流应用程序可以向底层输出流写入字节，而不必为写入的每一个字节导致底层系统的调用
* BufferedInputStream:创建BufferedInputStream 将创建一个内部缓冲区数组。当从流中读取或者跳过字节时，内部缓冲区将根据需要从所含的输入流中重新填充，一次很多字节



构造方法：

* 字节缓冲输出流：BufferedOutputStream(OutputStream out)
* 字节缓冲输入流：BufferedInputStream(InputStream in)

为什么构造方法需要的时字节流，而不是具体的文件或路径

* 字节缓冲流**仅仅提供缓冲区** ，而真正的读取数据还得依靠基本的字节流对象进行操作

















### 3、字符流

#### 3.1 出现字符流的原因

由于字节流操作中文不是特别方便，所以Java就提供了字符流

* **字符流=字节流 +编码表**



用字节流复制文本文件时，本地文件也有中文，但是没有问题，原因是最终底层操作会自动进行字拼接成中文，如何识别中文？

* 汉字在存储的时候，无论选择哪种编码储存，第一个字节都是负数















#### 3.2编码表

基础知识：

* 计算机中存储的信息都是用**二进制**数表示的；我们在屏幕上看到的英文，汉字字符时二进制数转换之后的结果

* 按照某种规则，将字符存储到计算机中，称为边**编码**。反之，将存储在计算机中的二进制数按照某种规则解析显示出来，称为**解码**。如果按照A编码存储，就必须按照A编码解析，否则就会出现乱码

  			字符编码：就是一套自然语言的字符与二进制之间的对应规则（A，65）

字符集：

* 是一个系统支持的所有字符的集合，包括个国家的文字、标点符号、图形符号、数字等
* 计算机要准确的存储和识别各种字符集的符号，就需要进行字符编码，一套字符集必然至少有一套字符编码。常见的字符集由ASCII字符集、GBXXX字符集、Unicode字符集等

Unicode字符集：

* 为表达任意语言的任意字符而设计，是业界的一种标准，也称为统一码、标准万国码。它最多使用四个字节的数字来表达每个字母、符号、或者文字。由三种编码方案、UTF-8、UTF-16、UTF32。最为常用的是UTF-8编码
* **UTF-8编码**：可以表示Unicode标准中任意字符，它是电子邮件、网页及其他存储或发送文字的应用中，优先采用的编码。互联网工作小组（IETF）要求所有互联网协议都必须支持UTF-8编码。它使用一至四个字节为每个字符编码



**小结：采用何种规则编码，就要采用对应规则解码，否则就会出现乱码**





#### 3.3 字符串中的编码解码问题

编码：

* byte[] getBytes():使用平台的默认字符集将该String编码为一系列字节，将结果存储到新的字节数组中
* byte[] getBytes(String charsetName): 使用指定的字符集将该String编码为一系列字节，将结果存储到新的字节数组中

解码：

* String(byte[] bytes):通过使用平台的默认字符集解码指定的字节数组来构造性的String
* String (byte[] bytes,String charsetName):通过指定字符集解码指定的字节数组来构造性的String





#### 3.4 字符流中的编码解码问题

字符流抽象基类

* Reader: 字符输出流的抽象类
* Writer:字符输入流的抽象类



 字符流中和编码解码问题相关的两个类

* InputStreamReader
* OutputStreamWriter









#### 3.5 字符流写数据的5种方式

* public write(int c)              写一个字符
* void write(char[] cbuf)       写入一个字符数组
* void write(char[] cbuf,int off,int len) 写入字符数组的一部分
* void write(String str)               写入一个字符串
* void write(String str,int off,int len) 写入一个字符串的一部分







* flush()  刷新流，话可以继续写数据
* close()  关闭流，释放数据 ，但是关闭之前会先刷新流。一但关闭就不能再写数据



#### 3.6 字符流读数据的2种方式



* in read()                            一次读一个字符数据

* in read(char[]  cbuf)       一次读一个字符数组数据





#### 3.7 转换流改进

转换流的名字较长，而我们常见的操作都是按照本地默认的编码实现的，所以为了简化书写，转换流提供了对应的子类

* FileReader: 用于读取字符文件的便捷类

 			FileReader（String fileName）

* FileWriter:用于写入字符文件的便捷类

			FileWriter(String fileName)











#### 3.8 字符缓冲流

字符缓冲流：

* BufferedWriter:将文本写入字符输出流，缓冲字符，以提供单个字符，数组和字符串的高校读取，可以指定缓冲区的大小，或者接受默认大小。默认值足够大，可用于大多数用途
* BuffereReader:从字符输入流读取文本，缓冲字符，以提供单个字符，数组和字符串的高校读取，可以指定缓冲区的大小，或者接受默认大小。默认值足够大，可用于大多数用途







构造方法：

* BufferedWriter(Writer out)

* BufferedReader(Reader in)



















#### 3.9 字符缓冲流特有功能

BufferedWriter:

*  void newLine();写一行行分隔符，行分隔符字符串由系统属性定义

BufferedReader:

* public String readLine():  读一行文字。结果包含行内的内容的字符串，不包括任何终止字符，如果流的结尾已经到达，则为null











#### 3.10 IO流小结



![image-20220427173524650](C:\Users\27815\AppData\Roaming\Typora\typora-user-images\image-20220427173524650.png)









![image-20220427173537946](C:\Users\27815\AppData\Roaming\Typora\typora-user-images\image-20220427173537946.png)













**案例：复制单极文件夹**

思路：

1. 创建数据源目录File对象，路径是E：\\\itcast

2. 获取数据源目录File对象的名称（itcast）

3. 创建目的地目录File对象，路径名是模块名+ iacast组成(myCharStream\\\itcast)

4. 判断目的地目录对应的File对象是否存在，如果不存在就创建

5. 获取源目录下所有文件的File数组

6. 遍历File数组，得到每一个File对象，该File对象就是数据源文件

   		数据源文件：D：\\\STL\\\第3章STL和基本数据结构.pptx

7. 获取数据源文件File对象名称（第3章STL和基本数据结构.pptx）

8. 创建目的地文件File对象，路径名是目的地目录+数据源文件：第3章STL和基本数据结构.pptx组成的（myCharStream\\\数据源文件：第3章STL和基本数据结构.pptx）

9. 复制文件

**由于文件不仅仅是文本文件，所以采用字节流复制文件**





    package itheima01;
    import java.io.*;
    
    public class copyfolderDemo {
    public static void main(String[] args) throws IOException {
        File  srcFolder=new File("D:\\STL");
       String srcFolderName= srcFolder.getName();
        File destFolder=new File("myCharStream",srcFolderName);
        if(!destFolder.exists()){
            destFolder.mkdir();
        }
        File[] listFiles=srcFolder.listFiles();
        for(File srcfile:listFiles){
            String srcFileName=srcfile.getName();
            File destFile=new File(destFolder,srcFileName);
            copyFile(srcfile,destFile);
        }
    }
    public static void copyFile(File srcFile,File destFile) throws IOException {
        BufferedInputStream bis =new BufferedInputStream(new FileInputStream(srcFile));
        BufferedOutputStream bos=new BufferedOutputStream(new FileOutputStream(destFile));
        byte[] bys=new byte[1024];
        int len;
        while((len=bis.read(bys))!=-1){
            bos.write(bys,0,len);
        }
        bos.close();
        bis.close();
    }
    }


**案例：复制多级文件夹** 

需求：把”D\\\typora"复制到C盘目录下

思路：

1. 创建数据源File对象，路径是E:\\\typora
2. 创建目的地File对象，路径是F：\\\
3. 写方法实现文件夹的复制，参数为数据源File对象和目的地File对象
4. 判断数据源File是否为目录

					是：
		
							A：在目的地下创建和数据源File名称一样的目录
		
							B：获取数据源File下所有文件或则目录的File数组
		
							C：遍历该File数组，得到每一个File对象
		
							D：把该File作为数据源File对象递归调用复制文件夹的方法
		
				不是：说明是文件，用字节流实现

~~~
 itheima_02;
 import java.io.*;

public class Demo {
    public static void main(String[] args)throws IOException {
        File srcFile=new File("D:\\typora");
        File destFile=new File("C:\\");
        copyFolder(srcFile,destFile);
    }
    //复制文件夹
    public static void copyFolder(File srcFile,File destFile) throws IOException {
        if(srcFile.isDirectory()){
            String srcFileName=srcFile.getName();
            File newFolder=new File(destFile,srcFileName);//F:\\typora
            if(!newFolder.exists()){
                newFolder.mkdir();
            }
            File[] files=srcFile.listFiles();
            for(File file:files){
                copyFolder(file,newFolder);
            }
        }
        else{
            File newFile=new File(destFile,srcFile.getName());
            copyFile(srcFile,destFile);
        }
    }
    //字节缓冲流方法复制文件
    public static  void copyFile(File srcFile,File destFile) throws IOException {
        BufferedInputStream bis=new BufferedInputStream(new FileInputStream(srcFile));
        BufferedOutputStream bos=new BufferedOutputStream(new FileOutputStream(destFile));
        int len;
        byte[] bys =new byte[1024];
        while((len=bis.read(bys))!=-1){
            bos.write(bys,0,len);
        }
        bis.close();
        bos.close();

    }

}
~~~
#### 3.11 复制文件的异常处理

try...catch...finally的做法：

~~~
try{
	可能出现异常的代码；
}catch(异常类名 变量名){
	异常的处理代码；
}finally{
	执行所有清除操作；
}



~~~
JDK7改进方案：
~~~
try(定义流对象){
	可能出现异常的代码；
}catch(异常类名 变量名){
	异常的处理代码；
}
自动释放资源


~~~
JDK9改进方案：
~~~

定义输入流对象
定义输出流对象
try(输入流对象；输出流对象){
	可能出现异常的代码：
}catch（异常类名 变量名）{
	异常处理的代码；
}
自动释放资源
~~~












### 4 特殊操作流



#### 4.1 标准输入输出流

System类中有两个静态的成员变量：

* public static final InputStream in:标准输入流。通常该流对应于键盘输入或者由主机环境或用户指定的另一个输入源
* public static final PrintStream out:标准输出流。通常该流对应于显示输出或由主机环境或用户指定的另一个输出目标

自己实现键盘录入数据：

* BufferedReader  br=new BufferedReader(new InputStreamReader(System.in));



写起来太麻烦，Java提供了一个类实现键盘录入

* Scanner sc=new Scanner(System.in);



输出语句的本质：是一个标准的输出流

* PrintStream ps=System.out;
* PrintStream类有的方法，System.out都可以用











#### 4.2 打印流

打印流分类：

* 字节打印流：PrintStream
* 字符打印流：PrintWriter



打印流的特点：

* 只负责输出数据，不负责读取数据3
* 有自己的特有方法



字节打印流

* PrintStream(String fileName):使用指定的文件名创建新的打印流
* 使用继承父类的方法写数据，查看的时候会转码；使用自己特有的方法写数据，查看的时候原样输出



字符打印流PrintWriter的构造方法：

* PrintWriter(String fileName) 使用指定的文件名创建一个新的PrintWriter，而不需要自动执行刷新

* PrintWriter(Writer out ,boolean autoFlush):

   创建一个新的PrintWriter 

  * out :字符输出流
  * autoFlush：一个布尔值，如果为真，则println，printf，或format方法将刷新输出缓冲流









#### 4.3 对象序列化流

 对象序列化：就是将对象全部保存到磁盘中，或者在网络中传输对象

这种机制就是使用一个字节序列表示一个对象，该字节序列包含：对象的类型、对象的数据和对象中存储的属性等信息字节序列写到文件之后，相当于文件中持久保存了一个对象的信息

反之，该字节序列还可以从文件中读取回来，重构对象，对它进行反序列化



要实现序列化和反序列化流就要使用对象系列化流和对象反序列化流：

* 对象序列化流：ObjectOutputStream
* 对象反序列化流：ObjectInputStream



**对象序列化流：ObjectOutputStream**

* 将Java对象的原始数据类型和图形写OutputStream。可以使用ObjectOutputStream读取对象。可以通过使用流的文件来实现对象的持久存储。如果流是网络套接字流，则可以在另一个主机上或另一个进程中重构对象

构造方法

* ObjectOutputStream(OutPutStream out):创建一个写入指定的OutputStream的ObjectOutputStream

序列化对象的方法：

* void writeObject(Object obj):将指定的对象写入ObjectOutputStream



**注意：**

* 一个对象想要被序列化，该对象所属的类必须实现Serializable接口
* Serializable 是一个标记接口，实现该接口不需要重写任何方法



**对象反序列化流：：ObjectInputStream**

* ：ObjectInputStream 反序列化先前使用：ObjectOutputStream编写的原始数据和对象



构造方法

* ：ObjectInputStream（InputStream）：创建从指定的InputStream读取ObjectInputStream



反序列化对象的方法

* Object readObject():从ObjectInputStream读取一个对象



用对象序列化流序列化了一个对象后，假如我们修改了对象所属文件，读取数据会抛出**InvaildClassException**

只要给所属的类添加一个serialVersionUID

**private static final long serialVersionUID=42L;**



如果一个对象中的某个成员变量不想被序列化

* 给该成员变量加transient关键字修饰，该关键字标记的成员变量不参与序列化过程













#### 4.4 Properties

Properties概述：

* 是一个Map体系的集合类
* Properties可以保存到流中或从流中加载

Properties作为集合的特有方法：

* Object setProperty(String key,String value)  设置集合的键和值，都是String类型，底层调用Hashtable方法put

* String getProperty(String key) 使用此属性列表中指定的键搜索属性

* Set< String >stringPropertyNames( ) 从该属性列表中返回一个不可修改的键集，其中键及其对应的值是字符串

  





Properties和IO流结合的方法：

* void load(InputStream inStream):   从输入字节流读取属性列表
* **void load(Reader reader)** ： 从输入字符流读取属性列表
* void store(OutputStream out,String comments)：将此属性列表（键和元素对）写入此Properties表中，以适合于使用load(InputStream)方法的格式写入输出流字节
* **void store(Writer writer,String comments)** ：将此属性列表（键和元素对）写入此Properties表中，以适合于使用load（Reader）方法的格式写入输出字符流









### 5、转换流

![image-20220510210706287](https://www.itbaizhan.com/wiki/imgs/image-20220510210706287-165218802743017.png)

InputStreamReader/OutputStreamWriter用来实现将字节流转化成字符流。

**通过转换流解决乱码**

ANSI(American National Standards Institute)美国国家标准协会

* InputStreamReader :是对输入流进行转换，将输入字节流转换为字符流     字节->字符

* OutputStreamWriter :是对输出流进行转换，将输出字符流转换为字节流     字符->字节



### 6、数据流

![image-20220510210327376](https://www.itbaizhan.com/wiki/imgs/image-20220510210327376-165218780905215.png)

数据流将“基本数据类型与字符串类型”作为数据源，从而允许程序以与机器无关的方式从底层输入输出流中操作Java基本数据类型与字符串类型。

DataInputStream和DataOutputStream提供了可以存取与机器无关的所有Java基础类型数据（如：int、double、String等）的方法。



**使用数据流时，读取的顺序一定要与写入的顺序一致，否则不能正确读取数据。**

### 7、FileUtils类中常用方法的介绍

打开FileUtils的api文档，我们抽出一些工作中比较常用的方法，进行总结和讲解。总结如下：

| 方法名                | 使用说明                                                     |
| --------------------- | ------------------------------------------------------------ |
| cleanDirectory        | 清空目录，但不删除目录                                       |
| contentEquals         | 比较两个文件的内容是否相同                                   |
| copyDirectory         | 将一个目录内容拷贝到另一个目录。可以通过FileFilter过滤需要拷贝的文件 |
| copyFile              | 将一个文件拷贝到一个新的地址                                 |
| copyFileToDirectory   | 将一个文件拷贝到某个目录下                                   |
| copyInputStreamToFile | 将一个输入流中的内容拷贝到某个文件                           |
| deleteDirectory       | 删除目录                                                     |
| deleteQuietly         | 删除文件                                                     |
| listFiles             | 列出指定目录下的所有文件                                     |
| openInputSteam        | 打开指定文件的输入流                                         |
| readFileToString      | 将文件内容作为字符串返回                                     |
| readLines             | 将文件内容按行返回到一个字符串数组中                         |
| size                  | 返回文件或目录的大小                                         |
| write                 | 将字符串内容直接写到文件中                                   |
| writeByteArrayToFile  | 将字节数组内容写到文件中                                     |
| writeLines            | 将容器中的元素的toString方法返回的内容依次写入文件中         |
| writeStringToFile     | 将字符串内容写到文件中                                       |

读取文件内容，并输出到控制台上（只需一行代码！）

**IOUtils的妙用**

打开IOUtils的api文档，我们发现它的方法大部分都是重载的。所以，我们理解它的方法并不是难事。因此，对于方法的用法总结如下：

| 方法名                                  | 使用说明                                                   |
| --------------------------------------- | ---------------------------------------------------------- |
| buffer                                  | 将传入的流进行包装，变成缓冲流。并可以通过参数指定缓冲大小 |
| closeQueitly                            | 关闭流                                                     |
| contentEquals                           | 比较两个流中的内容是否一致                                 |
| copy                                    | 将输入流中的内容拷贝到输出流中，并可以指定字符编码         |
| copyLarge                               | 将输入流中的内容拷贝到输出流中，适合大于2G内容的拷贝       |
| lineIterator                            | 返回可以迭代每一行内容的迭代器                             |
| read                                    | 将输入流中的部分内容读入到字节数组中                       |
| readFully                               | 将输入流中的所有内容读入到字节数组中                       |
| readLine                                | 读入输入流内容中的一行                                     |
| toBufferedInputStream，toBufferedReader | 将输入转为带缓存的输入流                                   |
| toByteArray，toCharArray                | 将输入流的内容转为字节数组、字符数组                       |
| toString                                | 将输入流或数组中的内容转化为字符串                         |
| write                                   | 向流里面写入内容                                           |
| writeLine                               | 向流里面写入一行内容                                       |

我们没有必要对每个方法做测试，只是演示一下读入d:/sxt.txt文件内容到程序中，并转成String对象，打印出来。





### **8、IO总结**

- 按流的方向分类：

  - 输入流：数据源到程序(InputStream、Reader读进来)。
  - 输出流：程序到目的地(OutputStream、Writer写出去)。

  

- 按流的处理数据单元分类：

  - 字节流：按照字节读取数据(InputStream、OutputStream)。
  - 字符流：按照字符读取数据(Reader、Writer)。

- 按流的功能分类：

  - 节点流：可以直接从数据源或目的地读写数据。
  - 处理流：不直接连接到数据源或目的地，是处理流的流。通过对其他流的处理提高程序的性能。

- IO的四个基本抽象类：InputStream、OutputStream、Reader、Writer

- InputStream的实现类：

  - FileInputStream
  - BufferedInputStream
  - DataInputStream
  - ObjectInputStream

- OutputStream的实现类：

  - FileOutputStream
  - BufferedOutputStream
  - DataOutputStream
  - ObjectOutputStream

- Reader的实现类

  - FileReader
  - BufferedReader
  - InputStreamReader

- Writer的实现类

  - FileWriter
  - BufferedWriter
  - OutputStreamWriter
  - PrintWriter

- 把Java对象转换为字节序列的过程称为对象的序列化。

- 把字节序列恢复为Java对象的过程称为对象的反序列化。





































































## Maven

* Maven 是专门用于管理和构建Java项目的工具，它的主要功能有：
  * 提供了一套标准化的项目结构
  * 提供了一套标准化的构建流程（编译、测试、打包、发布......）
  * 提供了一套依赖管理机制





**Maven 提供了一套标准化的项目结构，所有的IDE使用Maven构建的项目结构完全一样，所有IDE创建的Maven项目可以通用**

* 依赖管理：
  * 依赖管理其实就是管理你项目所依赖的第三方资源包（jar包、插件...）



### 1. Maven简介

* Apache Maven 是一个项目管理和构建的工具，它基于项目对象模型的概念，通过一小段描述信息来管理项目的构建、报告和文档
* 官网： <http://maven.apache.org/>







**仓库分类：**

	本地仓库：自己计算机上的一个目录
	
	中央仓库：由Maven团队维护的全球唯一的仓库
	
			地址：<http://repo1.maven.org/maven2/>	
	
	远程仓库：一般由公司团队搭建的私有仓库



### Meaven 基本使用





#### Maven常用命令

* compile:  编译
* clean:  清理
* test:  测试
* package:  打包
* install:  安装





#### Maven生命周期

* Maven 构建项目生命周期描述的是一次构建过程经历了多少个事件
* Maven对项目构建的生命周期划分为3套
  * clean：清理工作
  * default：核心工作、编译、测试、打包、安装等
  * site：产生报告、发布站点





#### Maven坐标

* 使用坐标来定义项目或引入项目中需要的依赖

* Maven 坐标的主要组成

  * groupld :定义当前Maven项目隶属于组织名称（通常是域名反写，例如：com.itheima）
  * artifactld : 定义当前Maven项目的名称（通常是模块名称）
  * version : 定义当前项目的版本号

  

#### 依赖范围

* 通过设置坐标的依赖范围（scope），可以设置 对应的jar包的作用范围：编译环境、测试环境、运行环境
  * test: 
  * provided:
  * runtime:
  * system:
  * import:
* < scope >默认值：compile

## 异常

### 1.1、异常概述

**异常：**就是程序出现了不正常的情况



**异常体系**	

* Throwable
  * Error
  * Exception
    * RuntimeException
    * 非RuntimeException

Error:严重问题，不需要处理

Exception：称为异常类，它表示程序本身可以处理的问题

* RuntimeException：在编译期间是不检查的，出现问题后，需要我们回来修改代码
* 非RuntimeException：在编译期就必须处理的，否则程序不能通过编译，就更不能正常运行了















 ### 1.2、JVM默认处理方案

如果程序出了问题，我们没有做任何处理，最终JVM会做出默认的处理

* 把异常的名称，异常原因及异常出现的位置等信息输出在了控制台
* 程序停止执行









### 1.3、异常处理

* try ... catch...
* throws



















### 1.4、异常处理之 try...catch...

* 格式

~~~
  try{
  	可能出现异常的代码;
  }catch(异常类名 变量名){
  		异常处理的代码;
  }

~~~
  执行流程:

  程序从try里面的代码开始执行

  出现异常，会自动生成一个异常类对象，该异常对象将被提交给Java运行时系统  ，当Java运行时系统接收到异常对象时，会到catch中去找匹配的异常类，找到后进行异常处理

  执行完毕后，程序还可以继续往下执行





### 1.5、Throwable的成员方法

> public String getMessage()    返回此throwable的详细消息字符串

> public String toString ()    返回此可抛出的简短描述

> public void printStackTrace()   把异常的错误信息输出在控制台











### 1.6 编译时异常和运行时异常的区别

Java中的异常被分为两大类：**编译时异常**和**运行时异常** ，也称为**受检异常**，**非受检异常**

所有的RuntimeException类及其子类被称为运行时异常，其他的异常都是编译时异常



* 编译时异常：必须显示处理，否则程序就会发生错误，无法通过执行
* 运行时异常：无需显示处理，也可以和编译时异常一样出理



### 1.7、异常处理之throws

格式：

> throws  异常类名

* 这个格式是跟在方法的括号后面的







### 1.8、自定义异常

**格式**

  ~~~
public class 异常类名 extends Exception{
	无参构造
	带参构造
}

  ~~~
### 1.9、throws和throw的区别

**throws**

* 在方法后面申明，跟的是异常类名
* 表示抛出异常，由该方法的调用者来处理
* 表示出现异常的一种可能性，并不一定会发生这些异常

****

**throw**

* 在方法体内，跟的是异常对象名
* 表示抛出异常，由方法体内的语句处理
* 执行throw一定是抛出了某种异常



### try-with-resourse

~~~
try(开启一个资源){

}catch(){

}
//在IO的时候不需要自己去释放资源
~~~
## 多线程

### 1、多线程的介绍

#### 1.1、 多线程的基本概念

**什么是程序？**

程序（Program）是一个静态的概念，一般对应于操作系统中的一个可执行文件。

**什么是进程?**

执行中的程序叫做进程(Process)，是一个动态的概念。其实进程就是一个在内存中独立运行的程序空间 。

> 现代操作系统比如Mac OS X，Linux，Windows等，都是支持“多任务”的操作系统，叫“多任务”呢？简单地说，就是操作系统可以同时运行多个任务。打个比方，你一边在用逛淘宝，一边在听音乐，一边在用微信聊天，这就是多任务，至少同时有3个任务正在运行。还有很多任务悄悄地在后台同时运行着，只是桌面上没有显示而已





**什么是线程？**

线程（Thread）是操作系统能够进行运算调度的最小单位。它被包含在进程之中，是进程中的实际运作单位。

> 有些进程还不止同时干一件事，比如微信，它可以同时进行打字聊天，视频聊天，朋友圈等事情。在一个进程内部，要同时干多件事，就需要同时运行多个“子任务”，我们把进程内的这些“子任务”称为线程（Thread）。

![image-20220123101220506](https://www.itbaizhan.com/wiki/imgs/image-20220123101220506.png)





#### 1.2、进程与线程的区别

*  线程是程序执行的最小单位，而进程是操作系统分配资源的最小单位；

* 一个进程由一个或多个线程组成，线程是一个进程中代码的不同执行路线；
* 进程之间相互独立，但同一进程下的各个线程之间共享程序的内存空间(包括代码段、数据集、堆等)及一些进程级的资源(如打开文件和信号)，某进程内的线程在其它进程不可见；
* 调度和切换：线程上下文切换比进程上下文切换要快得多



**进程：拥有自己独立的堆和栈，既不共享堆，也不共享栈；**

**进程：进程由操作系统调度；**

**线程：线程：被包含在进程之中，是进程中的实际运作单位。**





#### 1.3、什么是并发



并发是指在一段时间内同时做多个事情。当有多个线程在运行时,如果只有一个CPU,这种情况下计算机操作系统会采用并发技术实现并发运行，具体做法是采用“ 时间片轮询算法”，在一个时间段的线程代码运行时，其它线程处于就绪状。这种方式我们称之为并发。(Concurrent)。



* 串行(serial)：一个CPU上，按顺序完成多个任务
* 并行(parallelism)：指的是任务数小于等于cpu核数，即任务真的是一起执行的
* 并发(concurrency)：一个CPU采用时间片管理方式，交替的处理多个任务。一般是是任务数多余cpu核数，通过操作系统的各种任务调度算法，实现用多个任务“一起”执行（实际上总有一些任务不在执行，因为切换任务的速度相当快，看上去一起执行而已）





#### 1.4、线程的执行特点

**方法的执行特点**

![image-20220519142157308](https://www.itbaizhan.com/wiki/imgs/image-20220519142157308-16529413184406.png)

**线程的执行特点**

![image-20220519142241469](https://www.itbaizhan.com/wiki/imgs/image-20220519142241469-16529413627337.png)















#### 1.5、什么是主线程以及子线程

**主线程**

当Java程序启动时，一个线程会立刻运行，该线程通常叫做程序的主线程（main thread），即main方法对应的线程，它是程序开始时就执行的。

Java应用程序会有一个main方法，是作为某个类的方法出现的。当程序启动时，该方法就会第一个自动的得到执行，并成为程序的主线程。也就是说，main方法是一个应用的入口，也代表了这个应用的主线程。JVM在执行main方法时,main方法会进入到栈内存,JVM会通过操作系统开辟一条main方法通向cpu的执行路径,cpu就可以通过这个路径来执行main方法,而这个路径有一个名字,叫main(主)线程



**主线程的特点**

它是产生其他子线程的线程。

它不一定是最后完成执行的线程，子线程可能在它结束之后还在运行。

**子线程**

在主线程中创建并启动的线程，一般称之为子线程。

















### 2、多线程的创建



#### 2.1、通过继承Thread类实现多线程



继承Thread类实现多线程的步骤：

* 在Java中负责实现线程功能的类是java.lang.Thread 类。

> **此种方式的缺点：**如果我们的类已经继承了一个类（如小程序必须继承自 Applet 类），则无法再继承 Thread 类。

* 可以通过创建 Thread的实例来创建新的线程。
* 每个线程都是通过某个特定的Thread对象所对应的方法run( )来完成其操作的，方法run( )称为线程体。
* 通过调用Thread类的start()方法来启动一个线程。











#### 2.2、通过Runnable接口实现多线程

在开发中，我们应用更多的是通过Runnable接口实现多线程。这种方式克服了继承Thread类的缺点，即在实现Runnable接口的同时还可以继承某个类。



从源码角度看，Thread类也是实现了Runnable接口。Runnable接口的源码如下：

~~~
public interface Runnable {
     void run();
}
~~~

**注意：**

* **通过实现Runnable接口的方法更为常用**
* **一个线程只能被调用一次**







#### 2.3、线程的执行流程



![image-20220519160229845](https://www.itbaizhan.com/wiki/imgs/image-20220519160229845-16529473509189.png)















#### 2.4、线程的状态和生命周期

![image-20220123101751746](https://www.itbaizhan.com/wiki/imgs/image-20220123101751746.png)

一个线程对象在它的生命周期内，需要经历5个状态。

1. 新生状态(New)

   用new关键字建立一个线程对象后，该线程对象就处于新生状态。处于新生状态的线程有自己的内存空间，通过调用start方法进入就绪状态。

2. 就绪状态(Runnable)

   处于就绪状态的线程已经具备了运行条件，但是还没有被分配到CPU，处于“线程就绪队列”，等待系统为其分配CPU。就绪状态并不是执行状态，当系统选定一个等待执行的Thread对象后，它就会进入执行状态。一旦获得CPU，线程就进入运行状态并自动调用自己的run方法。有4种原因会导致线程进入就绪状态：

   1. 新建线程：调用start()方法，进入就绪状态；
   2. 阻塞线程：阻塞解除，进入就绪状态；
   3. 运行线程：调用yield()方法，直接进入就绪状态；
   4. 运行线程：JVM将CPU资源从本线程切换到其他线程。

3. 运行状态(Running)

   在运行状态的线程执行自己run方法中的代码，直到调用其他方法而终止或等待某资源而阻塞或完成任务而死亡。如果在给定的时间片内没有执行结束，就会被系统给换下来回到就绪状态。也可能由于某些“导致阻塞的事件”而进入阻塞状态。

4. 阻塞状态(Blocked)

   阻塞指的是暂停一个线程的执行以等待某个条件发生（如某资源就绪）。

   >有4种原因会导致阻塞：
   >
   >1. 执行sleep(int millsecond)方法，使当前线程休眠，进入阻塞状态。当指定的时间到了后，线程进入就绪状态。
   >2. 执行wait()方法，使当前线程进入阻塞状态。当使用nofity()方法唤醒这个线程后，它进入就绪状态。
   >3. 线程运行时，某个操作进入阻塞状态，比如执行IO流操作(read()/write()方法本身就是阻塞的方法)。只有当引起该操作阻塞的原因消失后，线程进入就绪状态。
   >4. join()线程联合: 当某个线程等待另一个线程执行结束后，才能继续执行时，使用join()方法

5. 死亡状态(Terminated)

死亡状态是线程生命周期中的最后一个阶段。线程死亡的原因有两个。一个是正常运行的线程完成了它run()方法内的全部工作； 另一个是线程被强制终止，如通过执行stop()或destroy()方法来终止一个线程（注：stop()/destroy()方法已经被JDK废弃，不推荐使用）。

**当一个线程进入死亡状态以后，就不能再回到其它状态了。**







### 3、多线程的使用

#### 3.1、终止线程的典型方法

终止线程我们一般不使用JDK提供的stop()/destroy()方法(它们本身也被JDK废弃了)。通常的做法是提供一个boolean型的终止变量，当这个变量置为false，则终止线程的运行。



~~~
import java.io.IOException;

public class TestThread3 implements Runnable{

    private boolean flag=true;

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName()+"线程开始");
            int i=0;

            while(flag){
                System.out.println( i+":"+Thread.currentThread().getName()+"线程开始");
                i++;
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        System.out.println(Thread.currentThread().getName()+"线程结束");



        }
        public  void stop(){
            this.flag=false;
    }

    public static void main(String[] args) {
        System.out.println("主线程开始");
            TestThread3 tst=new TestThread3();
        Thread t=new Thread(tst);
        t.start();

        try {
            System.in.read();
        } catch (IOException e) {
            e.printStackTrace();
        }
        tst.stop();
        System.out.println("主线程结束");
    }
}

~~~











#### 3.2、线程休眠

sleep()方法：可以让正在运行的线程进入阻塞状态，直到休眠时间满了，进入就绪状态。sleep方法的参数为休眠的毫秒数。





#### 3.3、线程让步



yield()让当前正在运行的线程回到就绪状态，以允许具有相同优先级的其他线程获得运行的机会。因此，使用yield()的目的是让具有相同优先级的线程之间能够适当的轮换执行。但是，实际中无法保证yield()达到让步的目的，因为，让步的线程可能被线程调度程序再次选中。

使用yield方法时要注意的几点：

- yield是一个静态的方法。
- 调用yield后，yield告诉当前线程把运行机会交给具有相同优先级的线程。
- yield不能保证，当前线程迅速从运行状态切换到就绪状态。
- yield只能是将当前线程从运行状态转换到就绪状态，而不能是等待或者阻塞状态。







#### 3.4、线程联合

前线程邀请调用方法的线程优先执行，在调用方法的线程执行结束之前，当前线程不能再次执行。线程A在运行期间，可以调用线程B的join()方法，让线程B和线程A联合。这样，线程A就必须等待线程B执行完毕后，才能继续执行。

**join方法的使用**

join()方法就是指调用该方法的线程在执行完run()方法后，再执行join方法后面的代码，即将两个线程合并，用于实现同步控制。













#### 3.5、获取当前线程名称



* 方式一

  ​	this.getName()获取线程名称，该方法适用于继承Thread实现多线程方式。

```
class GetName1 extends Thread{
   @Override
    public void run() {
        System.out.println(this.getName());
    }
}
```

* 方式二

  ​	Thread.currentThread().getName()获取线程名称，该方法适用于实现Runnable接口实现多线程方式。

```

class GetName2 implements Runnable{
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName());
    }
}
```

 











#### 3.6、设置线程名称

* 方式一

  ​	通过构造方法设置线程名称。

```
class SetName1 extends Thread{
    public SetName1(String name){
        super(name);
    }
    @Override
    public void run() {
        System.out.println(this.getName());
    }
}
public class SetNameThread {
    public static void main(String[] args) {
        SetName1 setName1 = new SetName1("SetName1");
        setName1.start();
    }
}
```

* 方式二

  ​	通过setName()方法设置线程名称。

```
class SetName2 implements Runnable{
    @Override
    public void run() {
5
        System.out.println(Thread.currentThread().getName());
    }
}
public class SetNameThread {
    public static void main(String[] args) {
        Thread thread = new Thread(new SetName2());
        thread.setName("SetName2");
        thread.start();
    }
}
```

 





#### 3.7、判断线程是否存活

isAlive()方法： 判断当前的线程是否处于活动状态。

活动状态是指线程已经启动且尚未终止，线程处于正在运行或准备开始运行的状态，就认为线程是存活的。





















### 4、线程的优先级以及守护线程



#### 4.1、线程的优先级介绍

**什么是线程的优先级**

每一个线程都是有优先级的，我们可以为每个线程定义线程的优先级，但是这并不能保证高优先级的线程会在低优先级的线程前执行。线程的优先级用数字表示，范围从1到10，一个线程的缺省优先级是5。

Java 的线程优先级调度会委托给操作系统去处理，所以与具体的操作系统优先级有关，如非特别需要，一般无需设置线程优先级。

> **注意**
>
> 线程的优先级，不是说哪个线程优先执行，如果设置某个线程的优先级高。那就是有可能被执行的概率高。并不是优先执行。



#### 4.2、限定优先级的使用

使用下列方法获得或设置线程对象的优先级。

- int getPriority();
- void setPriority(int newPriority);

> **注意：优先级低只是意味着获得调度的概率低。并不是绝对先调用优先级高的线程后调用优先级低的线程。**







#### 4.3、守护线程介绍

**什么是守护线程**

在Java中有两类线程：

- User Thread(用户线程)：就是应用程序里的自定义线程。
- Daemon Thread(守护线程)：比如垃圾回收线程，就是最典型的守护线程。

守护线程（即Daemon Thread），是一个服务线程，准确地来说就是服务其他的线程，这是它的作用，而其他的线程只有一种，那就是用户线程。

守护线程特点：

​		**守护线程会随着用户线程死亡而死亡。**

守护线程与用户线程的区别：

​		用户线程，不随着主线程的死亡而死亡。用户线程只有两种情况会死掉，

1. 在run()中异常终止。
2. 正常把run()执行完毕，线程死亡。

**守护线程，随着用户线程的死亡而死亡，当用户线程死亡守护线程也会随之死亡。**











#### 4.4、守护线程的使用

​		最经典的案例就是JVM的垃圾回收，JVM的主线程在运行时会启动很多的辅助线程，其中有一个就是做垃圾回收的辅助线程(守护线程)

一个线程的存在是为了给另一个线程服务的线程，而且要求声明周期随主线程消亡而消亡，可以用守护线程实现





















###  5、线程同步

#### 5.1、线程同步介绍

**线程冲突现象**

![image-20220521153746821](https://www.itbaizhan.com/wiki/imgs/image-20220521153746821-16531186678974.png)

**同步问题的提出**

现实生活中，我们会遇到“同一个资源，多个人都想使用”的问题。 比如：教室里，只有一台电脑，多个人都想使用。天然的解决办法就是，在电脑旁边，大家排队。前一人使用完后，后一人再使用。

**线程同步的概念**

处理多线程问题时，多个线程访问同一个对象，并且某些线程还想修改这个对象。 这时候，我们就需要用到“线程同步”。 线程同步其实就是一种等待机制，多个需要同时访问此对象的线程进入这个对象的等待池形成队列，等待前面的线程使用完毕后，下一个线程再使用。 



#### 5.2、线程冲突案例演示

我们以银行取款经典案例来演示线程冲突现象。

银行取钱的基本流程基本上可以分为如下几个步骤。

（1）用户输入账户、密码，系统判断用户的账户、密码是否匹配。

（2）用户输入取款金额

（3）系统判断账户余额是否大于或等于取款金额

（4）如果余额大于或等于取款金额，则取钱成功；如果余额小于取款金额，则取钱失败。

~~~
D:\ideaProjects\百战\Thread\src\Bank
~~~





#### 5.3、实现线程同步

由于同一进程的多个线程共享同一块存储空间，在带来方便的同时，也带来了访问冲突的问题。Java语言提供了专门机制以解决这种冲突，有效避免了同一个数据对象被多个线程同时访问造成的这种问题。这套机制就是synchronized关键字。

**synchronized语法结构：**

synchronized(锁对象){ 　 同步代码  }

**synchronized关键字使用时需要考虑的问题：**

- 需要对那部分的代码在执行时具有线程互斥的能力(线程互斥：并行变串行)。
- 需要对哪些线程中的代码具有互斥能力(通过synchronized锁对象来决定)。



**它包括两种用法：**

synchronized 方法和 synchronized 块。

- synchronized 方法

  通过在方法声明中加入 synchronized关键字来声明，语法如下：

  ```
  1
  public  synchronized  void accessVal(int newVal);
  ```

  synchronized 在方法声明时使用：放在访问控制符(public)之前或之后。这时同一个对象下synchronized方法在多线程中执行时，该方法是同步的，即一次只能有一个线程进入该方法，其他线程要想在此时调用该方法，只能排队等候，当前线程(就是在synchronized方法内部的线程)执行完该方法后，别的线程才能进入。

- synchronized块

  synchronized 方法的缺陷：若将一个大的方法声明为synchronized 将会大大影响效率。

  Java 为我们提供了更好的解决办法，那就是 synchronized 块。 块可以让我们精确地控制到具体的“成员变量”，缩小同步的范围，提高效率。





















#### 5.4、使用this作为线程对象锁

**语法结构：**

* ~~~
  synchronized(this){
  	//同步代码
  }
  ~~~

或

* ~~~
  public synchronized void accessVal(int newVal){
  	//同步代码
  }
  ~~~



**使用字符串作为线程对象锁**

所有线程在执行synchronized时都会同步





**使用class作为线程对象锁**

所有线程中拥有相同Class对象中的synchronized会互斥



**使用自定义对象作为线程对象锁**

所有的线程拥有相同自定义对象的synchronized会互斥

```
synchronized(自定义对象){ 
2
　　    //同步代码 
3
}
```













#### 5.5、死锁及其解决方案

**死锁的概念**

![image-20220522201430621](https://www.itbaizhan.com/wiki/imgs/image-20220522201430621-16532216717493.png)

“死锁”指的是：

多个线程各自占有一些共享资源，并且互相等待其他线程占有的资源才能进行，而导致两个或者多个线程都在等待对方释放资源，都停止执行的情形。

> 某一个同步块需要同时拥有“两个以上对象的锁”时，就可能会发生“死锁”的问题。比如，“化妆线程”需要同时拥有“镜子对象”、“口红对象”才能运行同步块。那么，实际运行时，“小丫的化妆线程”拥有了“镜子对象”，“大丫的化妆线程”拥有了“口红对象”，都在互相等待对方释放资源，才能化妆。这样，两个线程就形成了互相等待，无法继续运行的“死锁状态”。

 



**死锁的解决**

死锁是由于 “同步块需要同时持有多个对象锁造成”的，要解决这个问题，思路很简单，就是：同一个代码块，不要同时持有两个对象锁。















### 6、线程并发协作

#### 6.1、生产者消费者模式介绍

![image-20220524144814064](https://www.itbaizhan.com/wiki/imgs/image-20220524144814064-16533748950163.png)

多线程环境下，我们经常需要多个线程的并发和协作。这个时候，就需要了解一个重要的多线程并发协作模型“生产者/消费者模式”。

**角色介绍**

- **什么是生产者？**

  生产者指的是负责生产数据的模块（这里模块可能是：方法、对象、线程、进程）。

- **什么是消费者？**

  消费者指的是负责处理数据的模块（这里模块可能是：方法、对象、线程、进程）。

- **什么是缓冲区？**

  消费者不能直接使用生产者的数据，它们之间有个“缓冲区”。生产者将生产好的数据放入“缓冲区”，消费者从“缓冲区”拿要处理的数据。

缓冲区是实现并发的核心，缓冲区的设置有两个好处：

1. 实现线程的并发协作

   有了缓冲区以后，生产者线程只需要往缓冲区里面放置数据，而不需要管消费者消费的情况；同样，消费者只需要从缓冲区拿数据处理即可，也不需要管生产者生产的情况。 这样，就从逻辑上实现了“生产者线程”和“消费者线程”的分离，解除了生产者与消费者之间的耦合。

2. 解决忙闲不均，提高效率

   生产者生产数据慢时，缓冲区仍有数据，不影响消费者消费；消费者处理数据慢时，生产者仍然可以继续往缓冲区里面放置数据 。

 





#### 6.2、线程并发总结

线程并发协作（也叫线程通信）

生产者消费者模式：

1. 生产者和消费者共享同一个资源，并且生产者和消费者之间相互依赖，互为条件。

2. 对于生产者，没有生产产品之前，消费者要进入等待状态。而生产了产品之后，又需要马上通知消费者消费。

3. 对于消费者，在消费之后，要通知生产者已经消费结束，需要继续生产新产品以供消费。

4. 在生产者消费者问题中，仅有synchronized是不够的。synchronized可阻止并发更新同一个共享资源，实现了同步但是synchronized不能用来实现不同线程之间的消息传递（通信）。

5. 那线程是通过哪些方法来进行消息传递（通信）的呢？见如下总结：

   | **方法名**                              | **作 用**                                                    |
   | --------------------------------------- | ------------------------------------------------------------ |
   | final void wait()                       | 表示线程一直等待，直到得到其它线程通知                       |
   | void wait(long timeout)                 | 线程等待指定毫秒参数的时间                                   |
   | final void wait(long timeout,int nanos) | 线程等待指定毫秒、微秒的时间                                 |
   | final void notify()                     | 唤醒一个处于等待状态的线程                                   |
   | final void notifyAll()                  | 唤醒同一个对象上所有调用wait()方法的线程，优先级别高的线程优先运行 |

6. 以上方法均是java.lang.Object类的方法；

都只能在同步方法或者同步代码块中使用，否则会抛出异常。



> **OldLu建议**
>
> 在实际开发中，尤其是“架构设计”中，会大量使用这个模式。 对于初学者了解即可，如果晋升到中高级开发人员，这就是必须掌握的内容。

 







































## 反射



### 1、反射介绍

![image-20220529104312037](https://www.itbaizhan.com/wiki/imgs/image-20220529104312037-16537921932711.png)**什么是反射**

Java 反射机制是Java语言一个很重要的特性，它使得Java具有了“动态性”。在Java程序运行时，对于任意的一个类，我们能不能知道这个类有哪些属性和方法呢？对于任意的一个对象，我们又能不能调用它任意的方法？答案是肯定的！这种动态获取类的信息以及动态调用对象方法的功能就来自于Java 语言的反射（Reflection）机制。

**反射的作用**

简单来说两个作用，RTTI（运行时类型识别）和DC（动态创建）。

我们知道反射机制允许程序在运行时取得任何一个已知名称的class的内部信息，包括其modifiers(修饰符)，fields(属性)，methods(方法)等，并可于运行时改变fields内容或调用methods。那么我们便可以更灵活的编写代码，代码可以在运行时装配，无需在组件之间进行源代码链接，降低代码的耦合度；还有动态代理的实现等等；但是需要注意的是反射使用不当会造成很高的资源消耗！





**Java创建对象的三个阶段**

* Source源码阶段
* Class类对象阶段
* Runtime运行时对象





















### 2、获取Class对象



- 通过getClass()方法;
- 通过.class 静态属性;
- 通过Class类中的静态方法forName();















### 3、获取类的构造方法



**方法介绍**

| 方法名                                             | 描述                                                         |
| -------------------------------------------------- | ------------------------------------------------------------ |
| getDeclaredConstructors()                          | 返回 Constructor 对象的一个数组，这些对象反映此 Class 对象表示的类声明的所有构造方法。 |
| getConstructors()                                  | 返回一个包含某些 Constructor 对象的数组，这些对象反映此 Class 对象所表示的类的所有公共（public）构造方法。 |
| getConstructor(Class<?>... parameterTypes)         | 返回一个 Constructor 对象，它反映此 Class 对象所表示的类的指定公共（public）构造方法。 |
| getDeclaredConstructor(Class<?>... parameterTypes) | 返回一个 Constructor 对象，该对象反映此 Class 对象所表示的类或接口的指定构造方法。 |





















### 4、获取成员变量



#### 4.1、方法说明及使用

getFields()      返回Field类型的一个数组，其中包含Field对象中的所有公共字段

getDeclaredFields()  返回Field类型的一个数组，其中包含Field对象的所有字段

getField(String fieldName)    返回一个公共成员的Field指定对象

getDeclareField(String fieldName)   返回一个Field指定对象









#### 4.2、操作成员变量

~~~
public class GetField2 {
    public static void main(String[] args)throws Exception  {
        Class clazz = Users.class;
        Field field = clazz.getField("userage");
        //对象实例化
        Object obj = clazz.newInstance();
        //为成员变量赋予新的值
        field.set(obj,18);
        //获取成员变量的值
        Object o = field.get(obj);
        System.out.println(o);

    }
}
~~~







### 5、获取方法

**方法介绍**

| 方法名                             | 描述                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| getFields()                        | 返回Field类型的一个数组,其中包含 Field对象的所有公共(public)字段。 |
| getDeclaredFields()                | 返回Field类型的一个数组,其中包含 Field对象的所有字段。       |
| getField(String fieldName)         | 返回一个公共成员的Field指定对象。                            |
| getDeclaredField(String fieldName) | 返回一个 Field指定对象。                                     |



通过invoke() 方法来调用方法



### 6、获取其他信息



~~~
public class GetClassInfo {
    public static void main(String[] args) {
        Class clazz = Users.class;
        //获取类名
        String className = clazz.getName();
        System.out.println(className);
        //获取包名
        Package p = clazz.getPackage();
        System.out.println(p.getName());
        //获取超类
        Class superClass = clazz.getSuperclass();
        System.out.println(superClass.getName());
        //获取该类实现的所有接口
        Class[] interfaces = clazz.getInterfaces();
        for(Class inter:interfaces){
            System.out.println(inter.getName());
        }
    }
}
~~~



### 7、setAccessible方法

setAccessible()方法：

setAccessible是启用和禁用访问安全检查的开关。值为 true 则指示反射的对象在使用时应该取消 Java 语言访问检查。值为 false 则指示反射的对象应该实施 Java 语言访问检查;默认值为false。

由于JDK的安全检查耗时较多.所以通过setAccessible(true)的方式关闭安全检查就可以达到提升反射速度的目的。







### 8、本章总结

- Java 反射机制是Java语言一个很重要的特性，它使得Java具有了“动态性”。
- 反射机制的优点：
  - 更灵活。
  - 更开放。
- 反射机制的缺点：
  - 降低程序执行的效率。
  - 增加代码维护的困难。
- 获取Class类的对象的三种方式：
  - 运用getClass()。
  - 运用.class 语法。
  - 运用Class.forName()（最常被使用）。
- 反射机制的常见操作
  - 动态加载类、动态获取类的信息（属性、方法、构造器）。
  - 动态构造对象。
  - 动态调用类和对象的任意方法。
  - 动态调用和处理属性。
  - 获取泛型信息。
  - 处理注解。







## Lambda表达式

### 1、Lambda表达式介绍

#### 1.1、Lambda简介

**() -> {}**

**Lambda Expressions**

可以取代大部分的匿名内部类，写出更优雅的Java代码，尤其在集合的遍历和其他操作中，可以极大的优化代码结构

​	在Java语言中，可以为变量赋予一个值



**利用Lambda特性，可以把一个代码块赋予给一个变量**















#### 1.2、Lambda作用

最直观的作用就是使得代码变得更加简洁











#### 1.3、接口要求

虽然使用Lambda表达式可以对某些接口进行简单的实现，但并不是所有的接口都可以使用Lambda表达式来实现。Lambda规定接口中只能有一个需要被实现的方法，不是规定接口中只能有一个方法

 

​			jdk8 中有另一个新特性： default，被default 修饰的方法会有默认实现，不是必须被实现的方法，所以不影响Lambda表达式的使用





#### 1.4、@FunctionalInterface 注解使用



@FunctionalIterface标记在接口上，“函数式接口”是指仅仅只包含一个抽象方法的接口

### 2、Lambda表达式语法

#### 2.1、语法结构

~~~
(parameters)->expression
或者
(parameters)->{statements}
~~~

 	语法形式为()->{},其中()用来描述参数列表，{} 用来描述方法体，

->为Lambda运算符，读作(goes to)







#### 2.2、Lambda表达式的重要特征



* 可选类型声明:不需要声明参数类型，编译器可以统一识别参数值。



* 可选的参数圆括号:一个参数无需定义圆括号，但多个参数需要定义圆括号。



* 可选的大括号:如果主体包含了一个语句，就不需要使用大括号。



* 可选的返回关键字:如果主体只有一个表达式返回值则编译器会自动返回值，大括号需要指明表达式返回一个数值

 





















### 3、Lambda表达式入门案例



#### 3.1、定义接口







D:\ideaProjects\百战\Lambda\src\Test.java









































### 4、Lambda表达式的使用

#### 4.1、Lambda表达式引用方法.

​	有时候我们不是必须使用Lambda的函数体定义实现，我们可以利用Lambda表达式指向一个已经被实现的方法



**语法**：

方法归属者：：方法名 静态方法的归属类名，普通方法的归属者为对象



**要求**

* 参数个数以及类型需要与函数接口中的抽象方法一致
* 返回值类型要与函数接口中的抽象方法的返回值一致



#### 4.2、Lambda表达式创建线程

~~~
public class Test03 {

    public static void main(String[] args) {

        System.out.println(Thread.currentThread().getName()+"\t开始");
        Runnable runnable=()->{
            for(int i=0;i<20;i++){
                System.out.println(Thread.currentThread().getName()+"   "+i);
                try {
                    Thread.sleep(500);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        };
        
        
        
        new Thread(runnable,"Lambda Thread").start();
//或者 new Thread(()->{},"Lambda Thread").start();
        System.out.println(Thread.currentThread().getName()+"\t结束");
    }
}

~~~























### 5、操作集合

#### 5.1、遍历集合

我们可以调用集合的public void forEach(Consumer< ? super E>action)方法，通过Lambda表达式得方法遍历集合中的元素，

Consumer接口是JDK为我们提供的一个函数式接口



~~~

import java.util.ArrayList;
import java.util.List;
import java.util.function.Consumer;
class ConsumerImpl implements Consumer{
    @Override
    public void accept(Object o) {
        System.out.println(o);
    }
}
public class Test04 {

    public static void main(String[] args) {
        List<Integer> list = new ArrayList<Integer>();
        list.add(11);
        list.add(22);
        list.add(33);
        list.add(44);
        list.add(55);
        list.add(66);
        list.add(77);
        list.add(88);
        list.add(99);
      //  ConsumerImpl c=new ConsumerImpl();
        //        list.forEach(c);
//或者
        list.forEach(System.out::println);

    }
}

~~~



#### 5.2、删除集合中的元素

我们通过 public boolean removeIf(Predicate< ?super E> filter) 方法来删除集合中的某个元素

~~~
public class Test05 {
    public static void main(String[] args) {
        List<Integer> list = new ArrayList<Integer>();
        list.add(11);
        list.add(22);
        list.add(33);
        list.add(44);
        list.add(55);
        list.add(66);
        list.add(77);
        list.add(88);
        list.add(99);
        list.removeIf(ele->ele.equals(77));
        System.out.println(list);
    }
}
~~~





#### 5.3、元素排序

之前我们若要为集合内的元素排序，就必须调用sort方法，传入比较器重写Compare方法的比较器对象，现在我们可以用Lambda表达式来简化代码

~~~

~~~





### 6、Lambda表达式的闭包问题

闭包的本质就是代码片断。所以闭包可以理解成一个代码片断的引用。在Java中匿名内部类也是闭包的一种实现方式。



在闭包中访问外部的变量时，外部变量必须是final类型，虚拟机会帮我们加上 final修饰关键字。







## 网络编程(百战)

### 1、网络编程基本概念

#### 1.1、计算机网络

计算机网络是指将地理位置不同的具有独立功能的多台计算机及其外部设备，通过通信线路连接起来，在网络操作系统，网络管理软件及网络通信协议的管理和协调下，实现资源共享和信息传递的计算机系统。

从其中我们可以提取到以下内容：

1. 计算机网络的作用：资源共享和信息传递。
2. 计算机网络的组成：
   - 计算机硬件：计算机（大中小型服务器，台式机、笔记本等）、外部设备（路由器、交换机等）、通信线路（双绞线、光纤等）。             
   - 计算机软件：网络操作系统（Windows 2000 Server/Advance Server、Unix、Linux等）、网络管理软件（WorkWin、SugarNMS等）、网络通信协议（如TCP/IP协议栈等）。



#### 1.2、网络通信协议

![image-20211209112048423](https://www.itbaizhan.com/wiki/imgs/image-20211209112048423.png)

**什么是网络通信协议**

通过计算机网络可以实现不同计算机之间的连接与通信，但是计算机网络中实现通信必须有一些约定即通信协议，对速率、传输代码、代码结构、传输控制步骤、出错控制等制定标准。

国际标准化组织(ISO，即International Organization for Standardization)定义了网络通信协议的基本框架，被称为OSI（Open System Interconnect，即开放系统互联）模型。要制定通讯规则，内容会很多，比如要考虑A电脑如何找到B电脑，A电脑在发送信息给B电脑时是否需要B电脑进行反馈，A电脑传送给B电脑的数据格式又是怎样的？内容太多太杂，所以OSI模型将这些通讯标准进行层次划分，每一层次解决一个类别的问题，这样就使得标准的制定没那么复杂。OSI模型制定的七层标准模型，分别是：应用层，表示层，会话层，传输层，网络层，数据链路层，物理层。

OSI七层协议模型：

![image-20220525145914759](https://www.itbaizhan.com/wiki/imgs/image-20220525145914759-16534619556872.png)

**网络协议的分层**

虽然国际标准化组织制定了这样一个网络通信协议的模型，但是实际上互联网通讯使用最多的网络通信协议是TCP/IP网络通信协议。

TCP/IP 模型，也是按照层次划分，共四层：应用层，传输层，网络层，网络接口层（物理+数据链路层）。

OSI模型与TCP/IP模型的对应关系：

![image-20220525151808274](https://www.itbaizhan.com/wiki/imgs/image-20220525151808274-16534630893075.png)











#### 1.3、数据的封装与解封

![image-20220526091048716](https://www.itbaizhan.com/wiki/imgs/image-20220526091048716-16535274498671.png)

数据封装（Data Encapsulation）是指将协议数据单元（PDU）封装在一组协议头和协议尾中的过程。在OSI七层参考模型中，每层主要负责与其它机器上的对等层进行通信。该过程是在协议数据单元（PDU）中实现的，其中每层的PDU一般由本层的协议头、协议尾和数据封装构成。

- 数据发送处理过程
  1. 应用层将数据交给传输层，传输层添加上TCP的控制信息（称为TCP头部），这个数据单元称为段（Segment），加入控制信息的过程称为封装。然后，将段交给网络层。
  2. 网络层接收到段，再添加上IP头部，这个数据单元称为包（Packet）。然后，将包交给数据链路层。
  3. 数据链路层接收到包，再添加上MAC头部和尾部，这个数据单元称为帧（Frame）。然后，将帧交给物理层。
  4. 物理层将接收到的数据转化为比特流，然后在网线中传送。



- 数据接收处理过程
  1. 物理层接收到比特流，经过处理后将数据交给数据链路层。
  2. 数据链路层将接收到的数据转化为数据帧，再除去MAC头部和尾部，这个除去控制信息的过程称为解封，然后将包交给网络层。
  3. 网络层接收到包，再除去IP头部，然后将段交给传输层。
  4. 传输层接收到段，再除去TCP头部，然后将数据交给应用层。



从以上传输过程中，可以总结出以下规则：

1. 发送方数据处理的方式是从高层到底层，逐层进行数据封装。
2. 接收方数据处理的方式是从底层到高层，逐层进行数据解封。



接收方的每一层只把对该层有意义的数据拿走，或者说每一层只能处理发送方同等层的数据，然后把其余的部分传递给上一层，这就是对等层通信的概念。



数据封装与解封：

数据封装

![image-20220525160043418](https://www.itbaizhan.com/wiki/imgs/image-20220525160043418-16534656444636.png)

数据解封

![image-20220525160104765](https://www.itbaizhan.com/wiki/imgs/image-20220525160104765-16534656657938.png)











#### 1.4、IP地址



![image-20211209110317010](https://www.itbaizhan.com/wiki/imgs/image-20211209110317010.png)

**IP地址**

IP是Internet Protocol Address，即"互联网协议地址"。

用来标识网络中的一个通信实体的地址。通信实体可以是计算机、路由器等。 比如互联网的每个服务器都要有自己的IP地址，而每个局域网的计算机要通信也要配置IP地址。

路由器是连接两个或多个网络的网络设备。

IP地址分类：

| 类别 | 最大网络数    | IP地址范围                | 单个网段最大主机数 | 私有IP地址范围              |
| ---- | ------------- | ------------------------- | ------------------ | --------------------------- |
| A    | 126(2^7-2)    | 1.0.0.1-127.255.255.254   | 16777214           | 10.0.0.0-10.255.255.255     |
| B    | 16384(2^14)   | 128.0.0.1-191.255.255.254 | 65534              | 172.16.0.0-172.31.255.255   |
| C    | 2097152(2^21) | 192.0.0.1-223.255.255.254 | 254                | 192.168.0.0-192.168.255.255 |

目前主流使用的IP地址是IPV4，但是随着网络规模的不断扩大，IPV4面临着枯竭的危险，所以推出了IPV6。

![image-20211209124041972](https://www.itbaizhan.com/wiki/imgs/image-20211209124041972.png)

> IPV4，采用32位地址长度，只有大约43亿个地址，它只有4段数字，每一段最大不超过255。随着互联网的发展，IP地址不够用了，在2019年11月25日IPv4位地址分配完毕。
>
> IPv6采用128位地址长度，几乎可以不受限制地提供地址。按保守方法估算IPv6实际可分配的地址，整个地球的每平方米面积上仍可分配1000多个地址。

IP地址实际上是一个32位整数（称为IPv4），以字符串表示的IP地址如`192.168.0.1`实际上是把32位整数按8位分组后的数字表示，目的是便于阅读。

IPv6地址实际上是一个128位整数，它是目前使用的IPv4的升级版，以字符串表示类似于`2001:0db8:85a3:0042:1000:8a2e:0370:7334`

**公有地址**

公有地址（Public address）由Inter NIC（Internet Network Information Center互联网信息中心）负责。这些IP地址分配给注册并向Inter NIC提出申请的组织机构。通过它直接访问互联网。

**私有地址**

私有地址（Private address）属于非注册地址，专门为组织机构内部使用。

以下列出留用的内部私有地址

A类 10.0.0.0--10.255.255.255

B类 172.16.0.0--172.31.255.255

C类 192.168.0.0--192.168.255.255

> **注意事项**
>
> - 127.0.0.1 本机地址
> - 192.168.0.0--192.168.255.255为私有地址，属于非注册地址，专门为组织机构内部使用





#### 1.5、端口port



端口号用来识别计算机中进行通信的应用程序。因此，它也被称为程序地址。

一台计算机上同时可以运行多个程序。传输层协议正是利用这些端口号识别本机中正在进行通信的应用程序，并准确地进行数据传输。

![image-20211209151558070](https://www.itbaizhan.com/wiki/imgs/image-20211209151558070.png)

> **总结**
>
> - IP地址好比每个人的地址（门牌号），端口好比是房间号。必须同时指定IP地址和端口号才能够正确的发送数据。
> - IP地址好比为电话号码，而端口号就好比为分机号。

**端口分配**

端口是虚拟的概念，并不是说在主机上真的有若干个端口。通过端口，可以在一个主机上运行多个网络应用程序。 端口的表示是一个16位的二进制整数，对应十进制的0-65535。

操作系统中一共提供了0~65535可用端口范围。

按端口号分类：

**公认端口（Well Known Ports）：**从0到1023，它们紧密绑定（binding）于一些服务。通常这些端口的通讯明确表明了某种服务的协议。例如：80端口实际上总是HTTP通讯。

**注册端口（Registered Ports）：**从1024到65535。它们松散地绑定于一些服务。也就是说有许多服务绑定于这些端口，这些端口同样用于许多其它目的。例如：许多系统处理动态端口从1024左右开始。













#### 1.6、URL

























#### 1.7、Socket



Scoket实际是传输层供给应用层的编程接口

































#### 1.8、TCP协议和UDP协议的区别































### 2、网络编程常用类

Java为了跨平台，在网络应用通讯时是不允许直接调用操作系统接口的，而是由java.net包来提供网络功能







#### 2.1 _InetAddress



#####  _InetAddress的使用

作用：封装计算机的IP地址和DNS(没有端口信息)

> 注：DNS 是Domain Name System，域名系统



> **特点：**
>
> 这个类没有构造方法。如果要得到对象，只能通过静态方法：getLocalHost()、getByName()、getAllByName()、getAddress()、getHostName()



 **_获取本机信息**

获取本机信息需要使用getLocalHost方法创建InetAddress对象。getLocalHost()方法返回一个InetAddress对象，这个对象包含了本机的IP地址，计算机名等信息





##### _根据域名获取计算机的信息

根据域名获取计算机信息时需要使用getByName（"域名"）方法创建InetAddress对象





##### _根据IP获取计算机的信息

根据IP地址获取计算机信息时需要使用getByname("IP")方法创建InetAddress对象











#### 2.2_InetSocketAddress 



























#### 2.3_URL的使用





































































### 3、TCP通信的实现

#### 3.1、TCP通信介绍





















### 4、UDP协议的实现





































### 网络编程总结























## 网络编程（黑马）

### 1、网络编程入门

#### 1.1、网络编程概述

**计算机网络**

* 是指将地理位置不同的具有独立功能的多台计算机及其外部设备，通过通信线路连接起来，在网络操作系统，网络管理软件及网络通信协议的管理和协调下，实现资源共享和信息传递的计算机系统

**网络编程**

* 在网络通信协议下，实现网络互联的不同计算机上运行的程序间可以进行数据交换













#### 1.2 网络编程的三要素

**IP地址**

* 要想让网络中的计算机能够相互通信必须为每台计算机指定一个标识号，通过这个标识号来指定要接收数据的计算机和识别发送的计算，而IP地址就是这个标识号。也就是设备的标识。

**端口**

* 网络的通信，本质上是两个应用程序的通信。每台计算机都有很多的应用程序，那么网络通信时，如何区分这些应用程序呢，，，，，如果说IP地址可以唯一标识网络中的设备，那么端口号就可以唯一标识设备中的应用程序了。也就是应用程序的标识





**协议**

* 通过计算机网络可以使多台计算机实现连接，位于同一个网络中的计算机再进行通信时需要遵守一定的规则，这就好比在道路中行驶的汽车需要遵守交通规则一样。在计算机网络中，这些连接和通讯的规则被称为网络通信协议，他对数据的传输格式、传输效率、传输步骤等做了统一规定，通信双方必须同时遵守才能完成数据交换。常见的协议有UDP协议和TCP协议



#### 1.3 IP地址

IP地址：是网络中设备的唯一标识



IP地址分为两大类

* IPv4：是给每个连接在网络上的主机分配一个32bit地址。按照TCP/IP规定，IP地址使用二进制来表示，每个IP地址长32bit，也就是4个字节。例如一个采用二进制形式的IP地址是“11000000 10 10 1000 00000001 01000010”这么长的IP地址，处理起来也太费劲了。为了方便使用，IP地址经常被写成10进制的形式，中间使用符号“ . ”分隔不同的字节。于是上面的IP地址可以用192.168.1.66 。IP地址的这种表示方法叫做 ” 点分十进制表示法 “ ，这显然比1和0容易记忆得多
* IPv6：由于互联网的蓬勃发展，IP地址的需求量愈来愈大，但是网络地址资源有限，使得IP地址的分配越发紧张。为了扩大地址空间，通过IPv6重新定义了地址空间，采用128位地址长度，每16个字节一组，分成8组十六进制数，这样就解决了网络地址资源不够的问题



常用命令：

* ipconfig :查看本机IP地址
* ping IP地址：检查网络是否连接

特殊IP地址：

* 127.0.0.1：是回送地址，可以表示本机地址一般用来测试使用







#### 1.4 InetAddress 使用

为了方便我们对IP地址的获取和操作，Java提供了一个类	InetAddress供我们使用



InetAddress :此类表示Internet协议(IP)地址

* static InetAddress getByName(String host): 确定主机名称的IP地址。主机名称可以是机器名称，也可以是IP地址
* String getHostName(): 获取此IP地址的主机名
* String getHostAddress()： 返回文本显示中的IP地址字符串











#### 1.5 端口

端口：设备上应用程序的唯一标识

端口号：用两个字节表示的整数，他的取值范围是0~65535.其中，0~1023之间的端口号用于一些知名的网络服务和应用，普通的应用程序需要使用1024以上的端口号。如果端口号被另一个服务器或应用所占用，会导致当前程序启动失败





#### 1.6 协议

协议：计算机网络中，连接和通信的规则被称为网络通信协议



**UDP协议**

* 用户数据报协议（User Datagram Protocol）

* UDP是无连接通信协议，即在数据传输时，数据的发送端和接收端不建立逻辑连接。简单来说，当一台计算机像另一台计算机发送数据时，发送端不会确认接收端是否存在，就会发送数据，同样接收端在收到数据时也不会向发送端反馈是否收到数据

  由于使用UDP协议消耗资源小，通信效率高，所以通常都会用于音频、视频和普通数据的传输

* 例如视频会议通常采用UDP协议，因为这种情况即使偶尔丢失一两个数据包，也不会对接收的结果产生太大的影响。但是使用UDP协议传送数据时，由于UDP的面向无连接性，不能保证数据的完整性，因此在传输重要数据时不建议使用UDP协议



**TCP协议**

* 传输控制协议(Transmission Control Protocol)
* TCP协议是面向连接的通信协议，即传输数据之前，在发送端和接收端建立逻辑连接，然后再传输数据，它提供了两台计算机之间可靠无差错的数据传输。在TCP连接中必须要明确客户端和服务器端，由客户端向服务器发出连接请求，每次连接创建都需要经过"三次握手"
* 三次握手：TCP协议中，在发送数据的准备阶段，客户端与服务器之间的三次交互，以保证连接可靠。
  * 第一次握手，客户端向服务器发出连接请求，等待服务器确认
  * 第二次握手，服务器向客户端回送一个响应，通知客户端收到了一个连接请求
  * 第三次握手，客户端再次向服务器端发送确认信息，确认连接
* 完成了三次握手之后，连接建立后，客户端和服务器就可以开始进行数据传输了。由于这种面向连接的特性，TCP协议可以保证传输数据的安全，所以应用十分广泛。例如上传文件、下载文件、浏览网页等

**使用坐标导入jar坐标**

* 在 pom.xml 中编写< dependencies >标签
* 在< dependencies >标签中 使用< dependency > 引入坐标  
* 定义坐标的 **groupId **,  **artifactId**,  **version**























### 2、UDP通信程序

#### 2.1、UDP通信原理

UDP协议是一种不可靠的网络协议，它在通信的两端各建立一个Socket对象，但是这两个Socket只是发送，接收数据的对象

因此 对于基于UDP协议的通信双方而言，没有所谓的客户端和服务器端的概念

Java提供了DatagramSocket类作为基于UDP协议的Socket







#### 2.2、UDP发送数据

发送数据步骤

1. 创建发送端Socket对象(DatagramSocjet

   DatagramSocket()

2. 创建数据，并把数据打包

 		DatagramPacket(byte[] bys,int length ,InetAddress address ,int port)

3. 调用DatagramSocket对象的方法发送数据

		void sent (DatagramPacket p)

4. 关闭发送端

		void close()



### 3、TCP通讯程序

#### 3.1、TCP通信原理

TCP通信协议是一种可靠的网络协议，他在通信的两端各建立一个Socket对象，从而在通信的两端形成网络虚拟链路

一旦建立了虚拟的网络链路，两端的程序就可以通过虚拟链路进行通讯



Java对基于TCP协议的网络提供了良好的封装，使用Socket对象来表示两端的通信端口，并通过Socket产生IO流来进行网络通讯

Java为客户端提供了Socket类，为服务器端提供了ServerSocket类





#### 3.2、TCP发送数据

发送数据步骤：

* 创建客户端的Socket对象（Socket）

  Socket(String out,int port)

* 获取输出流，写数据

  OutputStream getOutputStream()

* 释放数据

  void close()











#### 3.3 、TCP接收数据

接受数据的步骤

* 创建服务器的Socket对象（ServerSocket）

  ServerSocket(int port)

* 监听客户端连接，返回一个Socket对象

  Socket accept()

* 获取输入流数据，读数据，并把数据显示在控制台

		InputStream getInputStream()

* 释放资源

  void close



## 数据结构



### 1、数据结构简介



















### 2、线性结构





















### 3、树形结构



























































# 数据库

## 一、基础篇

### 1、MySQL概述

#### 1.1 数据库相关概念：

数据库： 存储数据的仓库，数据是有组织的进行存储   DataBase

数据库管理系统：操纵和管理数据库的大型软件   DataBase Managements Ststem 

SQL ：操作关系型数据库的编程语言，定义了一套操作数据库统一**标准** Structured Query Language

数据库的启动与停止

* 启动

net start mysql80

* 停止

net stop mysql80





#### 1.2**客户端连接**

mysql[-h 170.0.0.1] [-p 3306] -u root -p

MYSQL自带的客户端命令行



#### 1.3关系型数据库（RDBMS）

概念：建立在关系模型的基础上，由多张相互连接的二维表组成的数据库

**特点：**

1. 使用表存储数据，格式统一，便于维护
2. 使用SQL语言操作，标准统一，使用方便





### 2、关系模型

#### 2.1 主键

* 对于关系表，有个很重要的约束，就是任意两条记录不能重复。不能重复不是指两条记录不完全相同，而是指能够通过某个字段唯一区分出不同的记录，这个字段被称为*主键*。



* 关系数据库实际上还允许通过多个字段唯一标识记录，即两个或更多的字段都设置为主键，这种主键被称为联合主键。对于联合主键，允许一列有重复，只要不是所有主键列都重复即可



* 主键是关系表中记录的唯一标识。主键的选取非常重要：主键不要带有业务含义，而应该使用BIGINT自增或者GUID类型。主键也不应该允许`NULL`。

  可以使用多个列作为联合主键，但联合主键并不常用。







#### 2.2 外键

* 关系数据库通过外键可以实现一对多、多对多和一对一的关系。外键既可以通过数据库来约束，也可以不设置约束，仅依靠应用程序的逻辑来保证



外键并不是通过列名实现的，而是通过定义外键约束实现的：

```
ALTER TABLE students
ADD CONSTRAINT fk_class_id
FOREIGN KEY (class_id)
REFERENCES classes (id);
其中，外键约束的名称fk_class_id可以任意，FOREIGN KEY (class_id)指定了class_id作为外键，REFERENCES classes (id)指定了这个外键将关联到classes表的id列（即classes表的主键）。
```

#### 2.3 索引

索引是关系数据库中对某一列或多个列的值进行预排序的数据结构。通过使用索引，可以让数据库系统不必扫描整个表，而是直接定位到符合条件的记录，这样就大大加快了查询速度。



* 唯一索引

  通过创建唯一索引，可以保证某一列的值具有唯一性。













### 3、SQL

#### 2.1 SQL通用语法

1. SQL语句可以单行或者多行书写，以分号结尾。
2. SQL语句可以用空格/缩进来增强语句的可读性。
3. MySQL数据库的SQL语句不分大小写，关键字建议用大写
4. 注释：
   * 单行注释：--注释内容 或者 # 注释内容（MySQL特有）
   * 多行注释：/* 注释内容 */



#### 2.2 SQL分类

* DDL    :数据定义语言，用来定义数据库对象（数据库，表，字段）

* DML   ：数据库操作语言，用来对数据库表中的数据进行增删改
* DQL    ：数据查询语言，用来查询数据库中表的记录
* DCL     ：数据控制语言，用来创建数据库用户，控制数据库的访问权限





#### 2.3 DDL

##### 2.3.1DDL- 数据库操作

* 查询
  * 查询所有数据库       SHOW DATABASES;
  * 查询当前数据库       SELECT DATABASE();

* 创建
  * CREATE DATABASE [ IF NOT EXITS ] 数据库名 [DEFAULT CHARSET 字符集] [COLLATE 排序规则]；
* 删除
  * DROP DATABASE [ IF EXIT ]数据库名；
* 使用
  * USE 数据库名

##### 2.3.2 DDL-表操作

###### 2.3.2.1查询

* 查询当前数据库所有表        SHOW TABLES;
* 查询表结构                           DESC 表名

* 查询指定表的建表语句        SHOW CREATE TABLE 表名；

###### 2.3.2.2创建

* CREATE TABLE 表名（

						字段1   字段1类型 [ COMMENT  字段1注释 ]，
		
						字段2   字段1类型 [ COMMENT  字段2注释 ]，
		
						字段3   字段1类型 [ COMMENT  字段3注释 ]，
		
						字段4   字段1类型 [ COMMENT  字段4注释 ]，
		
						............
		
						字段n   字段n类型 [ COMMENT  字段n注释 ]

）[ COMMENT 表注释 ]；

###### 2.3.2.3数据类型

MySQL中的数据类型有很多，主要分为三类：数值类型、字符串类型、日期时间类型。

加上 UNSIGNED 表示无负数

1. 数值类型
   1. 	TINYINT     1byte      小整数值
   2.     SMALLINT   2bytes     大整数值
   3.     MEDIUMINT  3bytes    大整数值
   4.     INT 或者INTEGER 4bytes  大整数值
   5.     BIGINT 8bytes   极大整数值
   6.     FOLAT   4bytes   单精度浮点数值
   7.     DOUBLE  8bytes   双精度浮点数值
   8.     DECIMAL    小数值（精确定点数）

2. 字符串类型
   1. CHAR                0~255 bytes  定长字符串     性能高
   2. VARCHAR         0~65535 bytes 变长字符串   占用的空间根据存储的内容而定
   3. TINYBOLB         0~255 bytes   不超过255个字符的二进制数
   4. TINYTEXT          0~255 bytes      短文本字符串
   5. BLOB                 0~65535 bytes   二进制形式的长文本数据
   6. TEXT                  0~65535 bytes   长文本数据
   7. MEDIUMBLOB  0~16777215 bytes 二进制形式的中等长度文本数据
   8. MEDIUMTEXT    0~16777215 bytes 中等长度文本数据
   9. LONGBLOB       二进制形式的极大文本数据
   10. LONGTEXT         极大文本数据
3. 日期时间类
   1. DATE              3					YYYY-MM-DD									日期值
   2. TIME               3                    HH:MM:SS                                         时间值或持续时间
   3. YEAR               1                    YYYY                                                   年份值
   4. DATETIME      8                    YYYY-MM-DD HH:MM:SS                混合日期和时间值 
   5. TIMESTAMP   4                    YYYY-MM-DD HH:MM:SS                混合日期和时间值，时间戳

###### 2.3.2.4 修改

* 添加字段

  ALTER TABLE 表名 ADD 字段名 类型（长度）[ COMMENT 注释] [约束]；

* 修改数据类型

		ALTER TABLE 表名 MODIFY 字段名 新数据类型；

* 修改字段名和字段类型

  ALTER TABLE 表名 CHANGE 旧字段名 类型 [ COMMENT 注释] [约束]；

* 删除字段

  ALTER TABLE 表名 DROP 字段名；

* 修改表名

  ALTER TABLE 表名 RENAME TO 新表名；

* 删除表
  * DROP TABLE [ IF EXISTS ]表名；  删除表
  * TRUNCATE TABLE 表名；删除指定表，并重新创建该表

**注意：在删除表的时候，表中的数据也会被删除。**



























#### 2.4 DML

* DML介绍

  DML是用来对数据库中的数据记录进行增删改操作的。

  * 添加数据 	**INSERT**
  * 修改数据	 **UPDATA**
  * 删除数据     **DELETE**

* DML-添加数据

  1. 给指定的字段添加数据

     INSERT INTO 表名（字段1，字段2...） VALUES(值1，值2...)；

  2. 给全部字段添加数据

     INSERT INTO 表名VALUES（值1，值2...）；

  3. 批量添加数据

     INSERT 表名（字段1，字段2...） VALUES（值1，值2...），（值1，值2...），（值1，值2...）...

     INSERT INTO表名 VALUES（值1，值2...），（值1，值2...）...；

     **插入数据时，指定的字段顺序需要与指定的值得顺序一一对应的**

     **字符串和日期数据应该包含在引号中**

     **插入的数据大小应该在字段的规定范围内**

     

* DML- 修改数据

  1. UPDATE 表名 SET  字段名1=值1，字段名2=值2，...[WHERE 条件]；

     **修改语句的条件可以有，也可以没有，如果没有条件则回修改整张表的所有数据**

* DML-删除数据

  1. DELETE FROM 表名（WHERE 条件）；

     1. **DELETE 语句的条件可以有，也可以没有，如果没有条件，则会删除整张表所有的数据。**
     2. **DELETE语句不能删除某一个字段的值（可以用UPTATE）**

     













#### 2.5 DQL

* DQL-介绍

  DQL是数据查询语言，用来查询数据库表中的数据



* DQL-语法

  * SELETE

     			字段列表

    FROM

    			表名列表

    WHERE

    			条件列表

    GROUP BY

    			分组字段列表

    HAVING

    			分组后条件列表

    ORDER BY

    			排序字段列表

    LIMIT

    			分页参数

* 基本查询
* 条件查询（WHERE)
* 聚合函数(COUNT、MAX、MIN、MIN、AVG、SUM
* 分组查询（GROUP BY）
*  排序查询(ORDER BY)
* 分页查询(LIMIT)

##### 2.3.1 DQL-基本查询

1. 查询多个字段

   SELECT字段1，字段2，字段3...FROM表名

   SELECT *FROM 表名

2. 设置别名

   SELECT 字段1[AS 别名1]， 字段2[AS 别名2]...FROM表名；

3. 去除重复记录

   SELECT DISTINCT 字段列表 FROM 表名



##### 2.3.2 DQL-条件查询

1. 语法

   SELECT 字段列表 FROM 表名 WHERE 条件列表

2. 条件

		可以跟**比较运算符**和**逻辑运算符		
		
		BETWEEN...AND ...(在某个范围内，含最大小值)
		
		IN(...)在in之后的列表中的值，多选一
		
		LIKE  占位符  模糊匹配（_匹配单个字符，%匹配多个字符）
		
		IS NULL 是NULL



##### 2.3.3 DQL-聚合函数

1. 介绍

   将一列数据作为一个整体，进行纵向计算。

2. 常见聚合函数

   1. count     统计数量
   2. max        最大值
   3. min         最小值
   4. avg         平均值
   5. sum        求和

3. 语法

		**SELECT 聚合函数（字段列表） FROM 表名；**
		
			**NULL值不参与所有聚合函数运算**
	
4. ##### 2.3.5 DQL-分组查询

   1. 语法	

      SELECT 字段列表 FROM 表名 [WHERE 条件] GROUP BY 分组字段名[HAVING 分组后过滤条件]

   2. WHERE和HAVING区别
      * 执行时机不同：WHERE是分组之前进行过滤，不满足WHERE条件，不参与分组；而HAVING 是分组之后对结果进行过滤。
      * 判断条件不同：WHERE不能对聚合函数进行判断，而HAVING 可以。

   **注意：**

   			**执行顺序：WHERE>聚合函数>HAVING。**
   	
   			**分组之后，查询的字段一般为聚合函数和分组字段，查询其他的字段毫无意义**

   

   

   

   

   

   ##### 2.3.6 DQL-排序查询

   1. 语法

   		SELECT 字段列表 FROM 表名 ORDER BY 字段1 排序方式1...；

   2. 排序方式

      * ASC ：升序（默认值）
      * DESC：降序

      **注意：如果是多字段排序，当地一个字段相同时，才会根据第二个字段进行排序**

   

   

   

   

   

   ##### 2.3.7 DQL-分页查询

   1. 语法

      SELECT 字段列表 FROM 表名 LIMIT 起始索引，查询记录数；

   **注意：**

   * 起始索引是从0开始，起始索引=（查询页码-1）*每页显示记录数
   * 分页查询是数据库的方言，不同的数据库有不同的实现，MySQL中是LIMIT
   * 如果查询的是第一页数据，其实索引可以省略，直接简写为LIMIT=10；

   

   

   

   ##### 2.3.8 DQL-执行顺序

   FROM

   			表明列表

   WHERE

   			条件列表

   GROUP BY

   			分组字段列表

   HAVING 

   			分组后条件列表

   SELECT

   			字段列表

   ORDER BY

   			排序字段列表

   LIMIT 

   			分页参数

   

   

   

   

   

   

   

   

   

   

   

   

   #### 2.6 DCL

   ##### 2.6.1 DCL介绍

   DCL是用来管理数据库 用户、控制数据库的访问 权限

   

   

   ##### 2.6.2 DCL-管理用户

   * 查询用户

     USE mysql;

     SELECT * FROM USER;

   * 创建用户

     CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码'；

   * 修改用户密码

     ALTER USER '用户名'@'主机名' IDENTIFIED WITH MYSQL_NATIVE_PASSWORD BY '新密码'；

   * 删除用户

     DROP USER '用户名'@'主机名';

   ##### 2.6.3 DCL-权限控制

   * MySQL中定义了很多种权限，但常用的只有：

     1. ALL,ALL PRIVILEGES           所有权限

     2. SELECT                                 查询数据
     3. INSERT                                 插入数据
     4. UPDATE                               修改数据
     5. DELATE                                删除数据
     6. ALTER                                  修改表
     7. DROP                                  删除数据库/表/视图
     8. CREATE                               创建数据库/表

   * 查询权限

     SHOW GRANTS FOR '用户名'@'主机名'；

   * 授予权限

     GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名'；

   * 撤销权限

     REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名'；

   

   **多个权限之间，使用逗号进行分隔**

   **授权时，数据库名和表名可以使用*进行统配，代表所有。**











### 4、函数

**函数** 是指一段可以直接被另一段程序调用的程序或代码



#### 4.1 、字符串函数



















#### 4.2、数值函数









#### 4.3、日期函数





















#### 4.4、流程函数























## JDBC API

**JDBC 就是使用Java语言操作关系数据库的一套API**



JDBC的好处是：

- 各数据库厂商使用相同的接口，Java代码不需要针对不同数据库分别开发；
- Java程序编译期仅依赖java.sql包，不依赖具体数据库的jar包；
- 可随时替换底层数据库，访问数据库的Java代码基本不变。



#### 1、DriverManager



* DriverManager(驱动管理类)作用：

  1. 注册驱动

     Class.forName("com.mysql.jdbc.Driver");

  2. 获取数据库连接

     1. url:连接路径		
     2. user:用户名
     3. password:密码

#### 2、Connection

* Connection(数据库连接对象)作用:

  1. 获取执行SQL的对象

     * 普通执行SQL对象

       > Statement  createStatement()

     * 预编译SQL的执行SQL对象：防止SQL注入

       >PreparedStatement  preparedStatement (sql)

     * 执行存储过程的对象

       > CallableStatement prepareCall(sql)

  

  2. 管理事务

     * MyAQL事务管理

       >开启事务：BEGIN;/START TRANSACTION;
       >
       >提交事务：COMMIT;
       >
       >回滚事务：ROLLBACK;

       MySQL默认自动提交事务

     * JDBC 事务管理：Connection 接口中定义了3个对应的方法

       >开启事务：setAutoCommit(boolean autoCommit): ture 为自动提交事务；false为手动提交事物，即为开启事务
       >
       >提交事务：commit()
       >
       >回滚事务：rollback()

     **用try catch 来实现JDBC的事务管理**







#### 3、Statement

* Statement 作用：

  1. 执行SQL语句

     int  executeUpdate(sql):执行DML、DDL语句

     返回值：（1）DML语句影响的行数（2）DDL语句执行成功后也有可能返回0

     resultSet executeQuery(sql):执行DQL语句

     返回值：ResultSet 结果集对象

  







#### 4、ResultSet

* ResultSet（结果集对象）作用：

  1. 封装了DQL语句的查询结果

     > Result stmt.executeQuery(sql):执行DQL语句，返回ResultSet对象

* 获取查询结果

  > boolean next():(1)将光标从当前位置向前移动一行 （2）判断当前行是否为有效行

  返回值：

  * true：有效行，当前行有数据
  * false：无效行，当前行没有数据

  > xxx getXxx(参数)：获取数据

  * xxx：数据类型；如 int getInt(参数)；String getString (参数)
  * 参数：
    * int:列的编号，从1开始
    * String：列的名称

* 使用步骤:

  1. 游标向下移动一行，并判断该行是否为有效行：next()

  2. 获取数据：getXxx(参数)

     //循环判断游标是否是最后一行末尾
     while(rs.next()){
     	//获取数据
     	rs.getXxx(参数);
     }
     
     ~~~
     
     ~~~


​     



#### 5、PreparedStatement

##### 5.1 PreparedStatement作用：

1. 预编译SQL语句并执行：预防SQL注入问题

   1. 获取PreparedStatement 对象

      ~~~
      //SQL语句中的参数值，使用？ 占位符代替
      String sql="SELECT * FROM USER WHERE NAME=? AND PASSWORD =?";
      
      //通过Connection 对象获取，并传入对应的SQL语句
      PreparedStatement patmt =conn.prepareStatement(sql);
      ~~~

   2. 设置参数值

      ~~~
      PreparedStatement 对象：SetXxx(参数1,参数2):给？赋值
      Xxx：数据类型；如setInt(参数1，参数2)
      参数：
      	参数1：？的位置编号开始，从1开始
      	参数2：？的值
      ~~~

   3. 执行SQL

      >executeUpdate();/executeQuery();不需要再传递sql



* SQL注入
  * SQL注入是通过操作输入来修改事先定义好的SQL语句，用以达到执行代码对服务器进行**攻击**的方法







**步骤：SQL注入演示**

​			需求：完成用户登录

SELECT*FROM tb_user WHERE username='张三'and password='123





##### 5.2 PreparedStatement 原理

* PreparedStatement 好处”

  1. 预编译SQL，性能更高
  2. 防止SQL注入：**将敏感字符进行转义**

* PreparedStatement 预编译功能开启：useServerPrepStmts=true

* 配置MySQL执行日志(重启mysql服务后生效)

  ~~~
  log-output=FILE
  general-log=1
  general_log_file="D:\mysql.log"
  slow-query-log=1
  slow_query_log-file="D:\mysql_slow.log"
  long_query_time=2
  ~~~

  

* PreparedStatement原理

  1. 在获取PreparedStatement对象时，将SQL语句发送给MySQL服务器进行检查，编译（很耗时）
  2. 执行时就不用在进行这些步骤了，速度更快
  3. 如果SQL语句模板一样，则只需要一次检查、编译





































## 数据库四大特性























































## 数据库连接池

#### 1、数据库连接池简介

* **数据库连接池**是一个容器，负责分配、管理数据库连接(Connection)
* 它允许应用程序重复使用一个现有的数据库连接，而不是再重新建立一个
* 它释放空闲时间超过最大空闲时间的数据库连接来避免因为没有释放数据库连接而引起的数据库连接遗漏
* 好处
  * 资源重开
  * 提升系统响应速度
  * 避免数据库连接遗漏

#### 2、数据库连接池实现

* 标准接口：DataSource

  * 官方(SUN)提供的数据库连接池标准接口，由第三方组织实现该接口

  * 功能：获取连接

    > Connection getConnetioc()

* 常见的数据库连接池

  * DBCP
  * C3P0
  * Druid

* Druid(德鲁伊)

  * Druid连接池思是阿里巴巴开源的数据库连接池项目
  * 功能强大，性能优秀，是Java语言最好的数据库连接池之一



#### 3、Druid使用步骤

1. 导入jar包durid-1.1.12.jar
2. 定义配置文件
3. 加载配置文件
4. 获取数据库连接池对象
5. 获取链接









### MyBatis

* MyBatis 是一款优秀的持久层框架，用于简化JDBC开发



**持久层**

* 负责将数据保存到数据库的那一层的代码
* JavaEE三层架构：表现层、业务层、**持久层**

**框架**

* 框架就是一个半成品软件，是一套可重用的、通用的、软件基础代码模型
* 在框架的基础之上构建软件编写更加的高效、规范、通用、可扩展

### MyBatis 快速入门













# HTML/CSS/JS

## HTML

* html是一种语言所有的网页都是用html这门语言编写出来的
* HTML（Hyper Text Markup Language）：超文本标记语言
  * 超文本：超越了文本的限制，比普通文本的更强大的。除了文字信息，还可以定义图片、音频、视频等内容
  * 标记语言：由标签构成的语言
* HTML运行在浏览器上，HTML标签由浏览器来解析
* HTML的标签都是预定义好的。例如：使用< img >展示图片
* W3C标准：网页主要由三部分组成
  * 结构：HTML
  * 表现：CSS
  * 行为：JavaScript









### 1、HTML 快速入门

1. 新建文本文件，后缀名改为.html

2. 编写HTML结构标签

3. 在< head >中定义关于文档的信息

4. 在< title >中定义文档的标题

5. 在< body >中定义文档的主体

   







### 2、基础标签

* < h1 >~< h6 >  定义标题h1最大，h6最小

* < font > 定义文本的字体、字体尺寸、字体颜色

* < b >定义粗体文字

* < i >定义斜体文字

* < u >定义文本下划线

* < center >定义文本居中

* < p >定义段落

* < br >定义拆行

* < hr 定义水平线>















### 3、图片、音频、视频标签

* img:定义图片
  * src：规定显示图像的URL（统一资源定位符）
  * height：定义图像的高度
  * width：定义图像的宽度
* audio:定义音频。支持的音频格式：**MP3、WAV、OGG**
  * src:规定音频的  URL
  * controls：显示播放控件
* video：定义视频。支持的视频格式：**MP4、WebM、OGG**
  * src:规定视频的  URL
  * controls：显示播放的控件









### 4、超链接标签

* < a > ：定义超链接，用于连接到另一个资源
  * href：指定访问资源的URL
  * target：指定打开资源的默认方式
    * _self：默认值，在当前页面打开
    * _blank：在空白页面打开









### 5、列表标签

* 有序标签(order list)

  ~~~
  <ol>
  	<li>咖啡</li>
  	<li>茶</li>
  	<li>牛奶</li>
  </ol>
  ~~~

* 无序列表(unorder list)

  ~~~
  <ul>
  	<li>咖啡</li>
  	<li>茶</li>
  	<li>牛奶</li>
  </ul>
  ~~~

  

< ol > 定义有序列表

< ul > 定义无序列表

< li > 定义列表项

* type：设置项目符号











### 6、表格标签

* < table > 定义表格
  * border：规定表格边框的宽度
  * width：规定表格的宽度
  * cellspacing：规定单元格之间的空白
* < tr >定义行
  * align：定义表格行的内容对齐方式
* < td >定义单元格
  * rowspan：规定单元格可横跨的行数
  * colspan：规定单元格可横跨的列数
* < th >定义表头单元格

~~~
<table width="50%" border="1" cellspacing="0">
    <tr height="50">
        <th>序号</th>
        <th>品牌Logo</th>
        <th>品牌名称</th>
        <th>企业名称</th>
    </tr>
    <tr align="center">
        <td>010</td>
        <td><img src="三只松鼠.png" width="60" heigth="50"></td>
        <td>三只松鼠</td>
        <td>三只松鼠</td>
    </tr>
    <tr align="center">
        <td>009</td>
        <td><img src="优衣库.png" width="60" heigth="50"></td>
        <td>优衣库</td>
        <td>优衣库</td>
    </tr>
    <tr align="center">
        <td>008</td>
        <td><img src="小米.png" width="60",height="50"></td>
        <td>小米</td>
        <td>小米科技有限公司</td>
    </tr>
</table>
~~~



















### 7、布局标签

* < div >：定义HTML文档中的一个区域部分，经常与CSS一起使用，用来布局网页
* < span > :用于组合行内元素













### 8、表单标签

* 表单：在网页中负责数据采集功能，使用< from >标签定义表单
  * action:规定当提交表单时向何处发送表单数据，URL
  * method:规定用于发送表单数据的方式
    * get：浏览器会直接将数据附在表单的action URL之后。
    * post：浏览器会将数据放到http请求消息中。无大小限制
* 表单项(元素)：不同类型的input元素、下拉列表、文本域等
  * < input >:表单项，通过type属性控制输入形式
  * < select >:定义下拉列表，< option >定义列表项
  * < textarea > :文本域
* **type取值**
  1. text                   默认值，定义单行的输入字段
  2. password        定义密码字段
  3. radio                定义单元按钮
  4. checkbox        定义复选框
  5. file                    定义文件上传按钮
  6. hidden             定义隐藏的输入字段
  7. submit             定义提交按钮，提交按钮会把表达把数据发送到服务器
  8. reset               定义重置按钮，重置按钮会清除表单中的所有数据
  9. button            定义可点击按钮



























## CSS

* CSS是一门语言，用于控制网页表现

​		CSS（Cascading Style Sheet）:层叠样式表



### 1、CSS导入方式



* CSS导入HTML有三种方式：

  1. 内联样式：在标签的内部使用style属性，属性值是css属性键值对

  ~~~ 
  <div style="color:red">Hello CSS~</div>
  ~~~

  

  2. 内部样式：定义< style >标签在标签内部定义CSS样式

     ~~~
     <style type="text/css">
     	div{
     		color:red;
     	}
     </style>
     ~~~

  3. 外部样式：定义link标签，引入外部的CSS文件

  ~~~
  <link rel ="stylesheet" href="demo.css">
  ~~~

  ~~~
  demo.css:
  div{
  	color:red;
  }
  ~~~

  

















### 2、CSS选择器

* 概念：选择器是选取需要设置样式的元素（标签）

  ~~~
  div{
  	color:red;
  }
  ~~~

* 分类：

  1. 元素选择器

     ~~~
     元素名称{color:red;}  div{color:red;}
     ~~~

  2. id选择器

     ~~~
     #id属性值{color:red;} 
     #name{color:red;}<div id="name">hello</div>
     ~~~

  3. 类选择器

     ~~~
     .class属性值{color：red;}
     .cls{color:red;}<div class="cls">hello</div>
     ~~~

     























### 3、CSS属性

* 见W3schools































## JavaScript

* JavaScript是一门跨平台的、面向对象的脚本语言，用来控制网页行为的，他能使网页可交互
* JavaScript和Java是完全不同的语言，无论是概念还是设计。但是基础语法类似









### 1、JavaScript 引入方式

1. **内部脚本：将JS代码定义在HTML页面中**

   ~~~
   <script>
   	alert("hello JS~");
   </script>
   ~~~

   *在HTML文档中可以在任意地方，放置任意数量的< script >*

   *一般把脚本置于< body >元素的底部，可改善显示速度，因为脚本执行会拖慢显示*

2. **外部脚本：将JS代码定义在外部JS文件中，然后引入到HTML页面中**

* 外部文件：demo.js 

  > alert("hello JS~");

* 引入外部JS文件

  > < script  src="../JS/demo.js">< /script >

  1. *外部脚本不能包含< script >标签*
  2. *< script >标签不能自闭和*



### 2、JavaScript 基础语法

#### 2.1 书写语法

1. 区分大小写：与Java一样，变量名、函数名以及其他一切东西都是区分大小写的

2. 每行结尾的分号可有可无

3. 注释：

   1. 单行注释：//注释内容
   2. 多行注释：/\*注释内容\*/

4. 大括号表示代码块

   ~~~
   if(count==3){
   	alert(count);
   }
   ~~~

   

   

   

#### 2.2 输出语句

* 使用window.alert() 写入警告框
* 使用document.writer() 写入HTML输出
* 使用console.log()学入浏览器控制台















#### 2.3 变量

* JavaScript 中用var 关键字（variable的缩写）来声明

  ~~~
  var test=20;
  test="张三";
  ~~~

* JavaScript是一门弱类型语言，变量可以存放不同类型的值

* 变量名需要遵循如下规则：

  * 组成的字符可以是任何字母、数字、下划线、或$
  * 数字不能开头
  * 建议使用驼峰命名

* ECMAScript6新增了**let**关键字来定义变量。它的用法类似于**var**，但是所声明的变量，只在**let**关键字所在的代码块内有效，且不允许重复声明

* ECMAScript6新增了**let**关键字，用来声明一个只读的常量。一旦声明，常量的值就不能改变







#### 2.4 数据类型

JavaScript中分为：原始类型和应用类型

5种原始类型：

* number:数字(整数、小数、NaN(Not a Number))
* string: 字符、字符串，单双引皆可
* boolean:布尔。true,false
* null:对象为空
* undefined:当声明的变量未初始化时，该变量的默认值是undefined

**使用typeof运算符可以获取数据类型**

> alert(typeof age);



**类型转换**

* 其他类型转换未number类型

  * string：按照字符串的字面值，转为数字，如果字面值不是数字，则转为NaN。。。一般使用parseInt

  * boolean：true转为1，false转为0

* 其他类型转化为boolean类型

  * number：0和NaN转为false，其他的数字转为true
  * string：空字符串转为false，其他的字符串转为true
  * null：转为flase
  * undefined：转为false

#### 2.5、运算符

* 一元运算符：++，--
* 算术运算符：+，-，*，/，%
* 赋值运算符：=，+=，-=...
* 关系运算符：>,<,>=,<=,!=,==,===...
* 逻辑运算符：&&,||,!
* 三元运算符：条件表达式？true_value:false_value

**==是值相等就返回真**

**===是数据类型以及值都相等才返回真**





#### 2.6、流程控制语句

* if：
* switch：
* for：
* while：
* do...while...：









#### 2.7、函数

函数(方法)是被设计为执行特定任务的代码块

* 定义：JavaScript通过function关键字进行定义，语法为：

  ~~~
  function functionName(参数1，参数2...){
  	要执行代码
  }
  ~~~

  **注意：**

  ​		**形式参数不需要类型。因为JavaScript是弱类型语言**

  ​		**返回值也不需要定义类型，可以在函数内部直接使用return返回即可**

  ~~~
  function add(a,b){
  	return a+b;
  }
  ~~~

  

* 调用：函数名称(实际参数列表);

  ~~~
  let result=add(1,2)
  ~~~

* **定义方式二：**

  ~~~
  var functionName=function(参数列表){
  	要执行的代码
  }
  ~~~

* 调用：JS中，函数调用可以传递任意个数参数

### 3、JavaScript 对象

#### 3.1 Array

JavaScript Array对象用于定义数组

* 定义

  ~~~
  var 变量名=new Array(元素列表);//方法1
  var arr=new Array(1,2,3)
  
  var 变量名=[元素列表]；//方法2
  var arr=[1,2,3]
  ~~~

* 访问

  ~~~
  arr[索引]=值;
  arr[0]=1;
  ~~~

**注意： JS数组类似于Java集合，长度，类型都可变**





* 方法
  * push：添加方法
  * splice（start，end）：删除元素

#### 3.2 String

* 定义

  ~~~
  var 变量名=new String(s);//方式一
  var str=new String("hello");
  var 变量名=S;//方式二
  var str="hello";
  var str='hello';
  ~~~

* 属性

  length    字符串的长度

* 方法

  charAt（）  返回指定位置的字符

  IndexOf（）检索字符串

  trim（）去除字符串两端的空白字符



#### 3.3 自定义对象

* 格式

  ~~~
  var 对象名称={
  			属性名称1：属性值1，
  			属性名称2：属性值2，
  			...
  			函数名称：function(形参列表){}
  			...
  			};
  ~~~

  





### 4、BOM

* Browser Object Model 浏览器对象模型
* JavaScript 将浏览器的各个部分封装为对象
* 组成
  * Window：浏览器窗口对象
  * Navigator：浏览器对象
  * Screen：屏幕对象
  * History：历史记录对象
  * Location：地址栏对象

#### 4.1 Window

* Window：浏览器窗口对象
* 获取：直接使用window，其中window.可以省略
* 属性：获取其他的BOM对象
  * history   对history对象的只读引用
  * Navigator  对Navagator对象的只读引用
  * Screen    对Screen对象道德只读引用
  * location 用于窗口或框架的Location对象
* 方法
  * alert()  显示带有一段消息和一个确认按钮的警告框
  * confirm() 显示带有一段消息以及确认按钮和取消按钮的对话框
  * setInterval() 按照指定的周期来调用函数或计算表达式
  * setTimeout()在指定的毫秒数后调用函数或计算表达式





#### 4.2、History

* History :历史记录

* 获取：使用window.history获取，其中window.可以省略

  ~~~
  window.history.方法();
  history.方法()；
  ~~~

* 方法

  * back()    加载history列表中的前一个URL
  * forward()  加载history列表中的下一个URL









#### 4.3、location

* Location：地址栏对象

* 获取：使用window.location获取，其中window.可以省略

  ~~~
  window.location.方法();
  location.方法();
  ~~~

* 属性

  href   设置或返回完整的URL



















### 5、DOM



* Document Object Model 文档对象模型
* 将标记语言的各个组成部分封装为对象
  * Document:整个文档对象
  * Element:元素对象
  * Attribute:属性对象
  * Text:文本对象
  * Comment:注释对象

* **JavaScript通过DOM，就可以对HTML进行操作了**
  * 改变HTML元素的内容
  * 改变HTML元素的样式(CSS)
  * 对HTML DOM事件做出反应
  * 添加删除HTML元素

**获取Element对象**

* Element：元素对象

* 获取：使用document对象的方法来获取

  1. getElementById：根据Id属性值获取，返回一个Element对象
  2. getElementsByTagName：根据标签名称获取，返回Element对象数组
  3. getElementsByName：根据name属性值获取，返回Element对象数组
  4. getElementsByClassName：根据class属性值获取，返回Element对象数组

  







**常见的HTML Element对象的使用**





style：设置元素CSS样式

innerHTML：设置元素内容





### 6、事件监听

* 事件：HTML事件是发生在HTML元素上的”事情”。比如：
  * 按钮被点击
  * 鼠标移动到元素之上
  * 按下键盘按键
* 事件监听：JavaScript可以在事件被侦测到时**执行代码**

**事件绑定**

* 方式一：通过HTML标签中的事件属性进行绑定

  ~~~
  <input type="buttom" onclik='on()'>
  
  function on(){
  	alert("我被点了");
  }
  ~~~

* 方式二：通过DOM元素属性绑定

  ~~~
  <input type="buttom" id="btn">
  document.getElementById("btn").onclik=function(){
  	alert("我被电了");
  }
  ~~~

  









### 7、正则表达式

* 概念：正则表达式定义了字符串的组成规则

* 定义：

  1. 直接量：注意不要加引号

     ~~~
     var reg=/^\w{6,12}$/;
     ~~~

  2. 创建RegExp对象

     ~~~
     var reg=new RegExp("^\\w{6,12}$");
     ~~~

* 方法：

  * test(str) :判断指定字符串是否符合规则，返回true或false

* 语法：

  * ^：表示开始
  * $：表示结束
  * []：代表在某个范围内的单个字符
  * .：代表任意单个字符，除了换行和行结束符
  * \w：代表单词字符：字母、数字、下划线
  * \d：代表数字字符
  * 量词：
  * +：至少一个
  * *：零个或多个
  * ？：零个或一个
  * {x}：x个
  * {m,}：至少m个
  * {m,n}：m到n个











# JavaWeb开发(bz)



## 一、计网协议详解





### 1.1计算机网络通信



![image-20211125145302503](https://www.itbaizhan.com/wiki/imgs/image-20211125145302503-16378231837922.png)

**什么是通信协议**

简单来说，通信协议就是计算机之间通过网络实现通信时事先达成的一种“约定”；这种“约定”使那些由不同厂商的设备，不同CPU及不同操作系统组成的计算机之间，只要遵循相同的协议就可以实现通信。

协议可以分很多种，每一种协议都明确界定了它的行为规范：2台计算机之间必须能够支持相同的协议，并且遵循相同的协议进行处理，才能实现相互通信。

**协议的标准化**

计算机通信诞生之初，系统化与标准化未收到重视，不同厂商只出产各自的网络来实现通信，这样就造成了对用户使用计算机网络造成了很大障碍，缺乏灵活性和可扩展性。为解决该问题，国际标准化组织（ISO）在1978年提出了“开放系统互联参考模型”，即著名的OSI/RM模型（Open System Interconnection/Reference Model）。它将计算机网络体系结构的通信协议划分为七层，自下而上依次为：物理层（Physics Layer）、数据链路层（Data Link Layer）、网络层（Network Layer）、传输层（Transport Layer）、会话层（Session Layer）、表示层（Presentation Layer）、应用层（Application Layer）。

![image-20211123091201165](https://www.itbaizhan.com/wiki/imgs/image-20211123091201165-16376299224962.png)

除了标准的OSI七层模型以外，常见的网络层次划分还有TCP/IP四层协议以及TCP/IP五层协议，它们之间的对应关系如下图所示：

![1](https://www.itbaizhan.com/wiki/imgs/1.jpg)











### 1.2、TCP、IP协议群

* **什么是TCP/IP协议群**

从字面意义上讲，有人可能会认为 TCP/IP 是指 TCP 和 IP 两种协议。实际生活当中有时也确实就是指这两种协议。然而在很多情况下，它只是利用 IP 进行通信时所必须用到的协议群的统称。具体来说，IP 或 ICMP、TCP 或 UDP、TELNET 或 FTP、以及 HTTP 等都属于 TCP/IP 协议。他们与 TCP 或 IP 的关系紧密，是互联网必不可少的组成部分。TCP/IP 一词泛指这些协议，因此，有时也称 TCP/IP 为网络协议群。

![image-20211126112130581](https://www.itbaizhan.com/wiki/imgs/image-20211126112130581-16378968917491.png)

* **什么是应用协议**

应用协议是定义了运行在不同系统上的应用程序进程如何相互传递报文的协议。

常见的应用协议：

1. FTP协议

   文件传输协议（File Transfer Protocol）。是用于在网络上进行文件传输的一套标准协议，它工作在 OSI 模型的第七层， TCP 模型的第四层， 即应用层，FTP允许用户以文件操作的方式（如文件的增、删、改、查、传送等）与另一主机相互通信。

2. HTTP协议

   一个简单的请求-响应协议，它通常运行在TCP之上。它指定了客户端可能发送给服务器什么样的消息以及得到什么样的响应。

3. DNS协议

   DNS（Domain Name System，域名系统），万维网上作为域名和IP地址相互映射的一个分布式数据库，能够使用户更方便的访问互联网，而不用去记住能够被机器直接知读取的IP地址。

4. SNMP协议

   简单网络管理协议。专门设计用于在 IP 网络管理网络节点（服务器、工作站、路由器、交换机及HUBS等）的一种标准协议，它是一种应用层协议。

* **什么是传输协议**

传输层提供了进程间的逻辑通信，传输层向高层用户屏蔽了下面网络层的核心细节，使应用程序看起来像是在两个传输层实体之间有一条端到端的逻辑通信信道。

1. TCP协议

   TCP：(Transmission Control Protocol)传输控制协议，TCP是一种面向连接的、可靠的、基于字节流的传输层（Transport layer）通信协议。TCP 为提供可靠性传输，实行“顺序控制”或“重发控制”机制。此外还具备“流控制（流量控制）”、“拥塞控制”、提高网络利用率等众多功能。

2. UDP协议

   UDP：(User Datagram Protocol)的简称， 是不具有可靠性的数据报文协议。虽然可以确保发送消息的大小，却不能保证消息一定会到达。

   **TCP与UDP比较**

![微信图片_20211124162621](https://www.itbaizhan.com/wiki/imgs/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20211124162621.jpg)

TCP 和 UDP 的优缺点无法简单地、绝对地去做比较：TCP 用于在传输层有必要实现可靠传输的情况；UDP 主要用于那些对高速传输和实时性有较高要求的通信或广播通信。TCP 和 UDP 应该根据应用的目的按需使用。

* 什么是网际协议

网际协议是一个网络层协议，它包含寻址信息和控制信息 ，可使数据包在网络中路由。

1. IP协议

   IP协议（Internet Protocol网际互连协议），它主要是完成两个任务，一个是寻找地址，第二个是管理分割数据片。

2. ICMP协议

   ICMP 的主要功能包括，确认 IP 包是否成功送达目标地址，通知在发送过程当中 IP 包被废弃的具体原因，改善网络设置等。

3. ARP协议

   ARP协议(Address Resolution Protocol，地址解析协议)是一个位于TCP/IP协议群中的网络层，负责将某个IP地址解析成对应的MAC地址。主机将包含目标IP地址信息的ARP请求广播到网络中的所有主机，并接收返回消息，以此确定目标IP地址的物理地址。









###  1.3、TCP协议传输特点

TCP是一个可靠的传输协议，在创建连接时会经历三次握手，在断开连接时会经历四次挥手。TCP 三次握手，其实就是 TCP 应用在发送数据前，通过 TCP 协议跟通信对方协商好连接信息，建立起TCP的连接关系。

**建立连接的三次握手**

所谓三次握手是指建立一个 TCP 连接时需要客户端和服务器端总共发送三个包以确认连接的建立。

![image-20211126150554722](https://www.itbaizhan.com/wiki/imgs/image-20211126150554722-16379103557545.png)

TCP建立连接时要传输三个数据包，俗称三次握手（Three-way Handshaking）。可以形象的比喻为下面的对话：

设备A：“你好，设备B，我这里有数据要传送给你，建立连接吧。”

设备B：“好的，我这边已准备就绪。”

设备A：“谢谢你受理我的请求。”

- 第一次握手：客户端发送 SYN报文，并进入 SYN_SENT状态，等待服务器的确认；
- 第二次握手：服务器收到 SYN报文，需要给客户端发送 ACK 确认报文，同时服务器也要向客户端发送一个 SYN 报文，所以也就是向客户端发送 SYN + ACK 报文，此时服务器进入 SYN_RCVD状态；
- 第三次握手：客户端收到 SYN + ACK报文，向服务器发送确认包，客户端进入 ESTABLISHED 状态。待服务器收到客户端发送的 ACK 包也会进入 ESTABLISHED状态，完成三次握手。

**断开连接的四次挥手**

四次挥手即终止TCP连接，就是指断开一个TCP连接时，需要客户端和服务端总共发送4个包以确认连接的断开。当我们的应用程序不需要数据通信了，就会发起断开 TCP 连接。建立一个连接需要三次握手，而终止一个连接需要经过四次挥手。

![image-20211126150643428](https://www.itbaizhan.com/wiki/imgs/image-20211126150643428-16379104043146.png)

断开连接需要四次握手，可以形象的比喻为下面的对话：

设备A：“任务处理完毕，我希望断开连接。”

设备B：“哦，是吗？请稍等，我准备一下。”

等待片刻后……

设备B：“我准备好了，可以断开连接了。”

设备A：“好的，谢谢合作。”

- 第一次挥手。客户端发起 FIN包（FIN = 1）,客户端进入 FIN_WAIT_1 状态。TCP 规定，即使 FIN包不携带数据，也要消耗一个序号。
- 第二次挥手。服务器端收到 FIN包，发出确认包 ACK（ack = u + 1），并带上自己的序号 seq=v，服务器端进入了 CLOSE_WAIT状态。这个时候客户端已经没有数据要发送了，不过服务器端有数据发送的话，客户端依然需要接收。客户端接收到服务器端发送的 ACK后，进入了FIN_WAIT_2状态。
- 第三次挥手。服务器端数据发送完毕后，向客户端发送FIN包（seq=w ack=u+1），半连接状态下服务器可能又发送了一些数据，假设发送 seq 为 w。服务器此时进入了 LAST_ACK状态。
- 第四次挥手。客户端收到服务器的 FIN包后，发出确认包（ACK=1，ack=w+1），此时客户端就进入了TIME_WAIT状态。注意此时 TCP 连接还没有释放，必须经过2*MSL后，才进入CLOSED状态。而服务器端收到客户端的确认包ACK后就进入了CLOSED状态，可以看出服务器端结束 TCP 连接的时间要比客户端早一些







### 1.4、服务端口

* **端口作用**

端口号用来识别计算机中进行通信的应用程序。因此，它也被称为程序地址。

一台计算机上同时可以运行多个程序。传输层协议正是利用这些端口号识别本机中正在进行通信的应用程序，并准确地进行数据传输。

![image-20211123093422469](https://www.itbaizhan.com/wiki/imgs/image-20211123093422469-16376312634891.png)

* **端口分配**

操作系统中一共提供了0~65535可用端口范围。

按端口号分类：

公认端口（Well Known Ports）：从0到1023，它们紧密绑定（binding）于一些服务。通常这些端口的通讯明确表明了某种服务的协议。例如：80端口实际上总是HTTP通讯。

注册端口（Registered Ports）：从1024到65535。它们松散地绑定于一些服务。也就是说有许多服务绑定于这些端口，这些端口同样用于许多其它目的。例如：许多系统处理动态端口从1024左右开始。

* **常见的应用层协议与端口分配**

![image-20211123093555631](https://www.itbaizhan.com/wiki/imgs/image-20211123093555631-16376313567432.png)













### 1.5、数据包与处理流程

* **什么是数据包**

通信传输中的数据单位，一般也称“数据包”。在数据包中包括：包、帧、数据包、段、消息。

网络中传输的数据包由两部分组成：一部分是协议所要用到的首部，另一部分是上一层传过来的数据。首部的结构由协议的具体规范详细定义。在数据包的首部，明确标明了协议应该如何读取数据。反过来说，看到首部，也就能够了解该协议必要的信息以及所要处理的数据。包首部就像协议的脸。

![image-20211123093719713](https://www.itbaizhan.com/wiki/imgs/image-20211123093719713-16376314408063-16376316484865.png)

* **数据包处理流程**

![image-20211123093754803](https://www.itbaizhan.com/wiki/imgs/image-20211123093754803-16376314758864.png)





### 1.5 HTTP协议简介



####  1.5.1、HTTP协议介绍

**什么是超文本**

超文本是用超链接的方法，将各种不同空间的文字信息组织在一起的网状文本。超文本更是一种用户界面范式，用以显示文本及与文本之间相关的内容。

![image-20211123094151415](https://www.itbaizhan.com/wiki/imgs/image-20211123094151415-16376317124076.png)

![image-20211126171750022](https://www.itbaizhan.com/wiki/imgs/image-20211126171750022-16379182711137.png)

**什么是HTTP协议**

HTTP协议是Hyper Text Transfer Protocol（超文本传输协议）的缩写, HTTP是万维网（WWW:World Wide Web）的数据通信的基础。

HTTP是一个简单的请求-响应协议，它通常运行在TCP之上。它指定了客户端可能发送给服务器什么样的消息以及得到什么样的响应。

![image-20211123094319310](https://www.itbaizhan.com/wiki/imgs/image-20211123094319310-16376318004629.png)

HTTP是一个基于TCP/IP通信协议来传递数据（HTML 文件, 图片文件, 查询结果等）。

![img](https://www.itbaizhan.com/wiki/imgs/wpsE489.tmp.jpg)











#### 1.5.2、HTTP协议特点

支持客户端/服务端模式：

HTTP协议支持客户端服务端模式，需要使用浏览器作为客户端来访问服务端。

简单快速：

客户向服务器请求服务时，只需传送请求方法和路径。请求方法常用的有GET、POST等。每种方法规定了客户与服务器联系的类型不同。由于HTTP协议简单，使得HTTP服务器的程序规模小，因而通信速度很快

灵活：

HTTP允许传输任意类型的数据对象。正在传输的类型由Content-Type（Content-Type是HTTP包中用来表示内容类型的标识）加以标记。

无连接：

每次请求一次，释放一次连接。所以无连接表示每次连接只能处理一个请求。优点就是节省传输时间，实现简单。我们有时称这种无连接为短连接。对应的就有了长链接，长连接专门解决效率问题。当建立好了一个连接之后，可以多次请求。但是缺点就是容易造成占用资源不释放的问题。当HTTP协议头部中字段Connection：keep-alive表示支持长链接

单向性：

服务端永远是被动的等待客户端的请求。

无状态：

HTTP协议是无状态协议。无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。为了解决HTTP协议无状态，于是，两种用于保持HTTP连接状态的技术就应运而生了，一个是Cookie，而另一个则是Session。







#### 1.5.3、HTTP中的URI、URL、URN



* **URI**

URI：（Uniform Resource Identifier），统一资源标识符，是一个用于标识互联网某个唯一资源的字符串名称。

URL和URN都是URI的子集。

![url](https://www.itbaizhan.com/wiki/imgs/url.png)

URI是个纯粹的语法结构，用于指定标识Web资源的字符串的各个不同部分。他不属于定位符，因为根据该标识符无法定位任何资源。

* **URL**

URL(Uniform Resource Location)，统一资源定位符，可以帮助我们定位互联网上的某一个唯一资源，相当于是互联网资源的身份证号。

URL的五个元素包括在一个简单的地址中：

- 传送协议。
- 服务器。（通常为域名或者IP地址）
- 端口号。（以数字方式表示，若为HTTP的默认值“:80”可省略）
- 请求资源路径。
- 传递数据。（在URL中传递数据是以key=value的结构进行数据绑定，以“?”字符为起点，每个参数以“&”隔开，再以“=”分开参数名称与数据，通常以UTF8的URL编码，避开字符冲突的问题）

举个栗子：

```
1
http://www.itbaizhan.cn:80/course/id/18.html?a=3&b=4
```

其中：

1. http, 是协议；
2. [www.itbaizhan.cn](http://www.itbaizhan.cn/)，是服务器域名；
3. 80，是服务器上的默认网络端口号，默认不显示；
4. /course/id/18.html，是路径(URI：直接定位到对应的资源)；
5. ?a=3&b=4，请求时传递的数据；

* **URN**

URN（Uniform Resource Name，）统一资源名称，其目的是通过提供一种途径，用于在特定的命名空间资源的标识，以补充网址。

举个栗子：

```
1
www.itbaizhan.cn:80/course/id/18.html?a=3&b=4 
```

URN是URI的子集，包括名字(给定的命名空间内)，但是不包括访问方式来。













#### 1.5.4、HTTP协议的请求



![image-20211124161353561](https://www.itbaizhan.com/wiki/imgs/image-20211124161353561-16377416347053.png)

* **HTTP协议中的请求信息**

打开一个网页需要浏览器发送很多次Request

- 当你在浏览器输入URL [http://www.itbaizhan.cn](http://www.itbaizhan.cn/) 的时候，浏览器发送一个Request去获取 [http://www.itbaizhan.cn](http://www.itbaizhan.cn/) 的html. 服务器把Response发送回给浏览器。
- 浏览器分析Response中的 HTML，发现其中引用了很多其他文件，比如图片，CSS文件，JS文件。
- 浏览器会自动再次发送Request去获取图片，CSS文件，或者JS文件。
- 等所有的文件都下载成功后。 网页就被显示出来了。

* 请求状态分析(request)

Request 消息分为3部分，第一部分叫Request line, 第二部分叫Request header, 第三部分是Request body。Request header和Request body之间有个空行。



我们以访问：http://www.itbaizhan.cn/course/id/18.html为例，查看请求状态：

![image-20211123102413022](https://www.itbaizhan.com/wiki/imgs/image-20211123102413022.png)







 



#### 1.5.5 请求方式

- GET

  向指定的资源发出“显示”请求。GET请求中会将请求中传递的数据包含在URL中并在浏览器的地址栏中显示。GET请求传递数据时要求数据必须是字符。GET请求可以被浏览器缓存。

- POST

  向指定资源提交数据，请求服务器进行处理（例如提交表单或者上传文件）。数据被包含在请求体中。POST请求传递数据时，数据可以是字符也可以是字节型数据，默认为字符型。POST请求默认情况下不会被浏览器所缓存。

- HEAD

  向服务器索要与GET请求相一致的响应，只不过响应体将不会被返回。这一方法可以在不必传输整个响应内容的情况下，就可以获取包含在响应消息头度中的元信息。

- PUT

  向指定资源位置上传其最新内容。

- DELETE

  请求服务器删除Request-URI所标识的资源。

**GET和POST的区别(重要,面试常问)**

1. GET在浏览器回退时是无害的，而POST会再次提交请求。
2. GET产生的URL地址可以被Bookmark，而POST不可以。
3. GET请求会被浏览器主动cache，而POST不会，除非手动设置。
4. GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。
5. GET请求在URL中传送的参数是有长度限制的，而POST则没有。对参数的数据类型GET只接受ASCII字符，而POST即可是字符也可是字节。
6. GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息。
7. GET参数通过URL传递，POST放在Request body中。















### 1.6、MIME类型

**什么是MIME类型**

MIME(Multipurpose Internet Mail Extensions)多用途互联网邮件扩展类型。是设定某种扩展名的文件用一种应用程序来打开的方式类型，当该扩展名文件被访问的时候，浏览器会自动使用指定应用程序来打开。多用于指定一些客户端自定义的文件名，以及一些媒体文件打开方式。

**MIME类型的作用**

HTTP协议所产生的响应中正文部分可以是任意格式的数据，那么如何保证接收方能看得懂发送方发送的正文数据呢？HTTP协议采用MIME协议来规范正文的数据格式。

**MIME类型的使用**

在服务端我们可以设置响应头中Content-Type的值来指定响应类型。

**MIME类型对应列表**

| Type                                 | Meaning                          |
| ------------------------------------ | -------------------------------- |
| application/msword                   | Microsoft Word document          |
| application/octet-stream             | Unrecognized or binary data      |
| application/pdf                      | Acrobat (.pdf) file              |
| application/postscript               | PostScript file                  |
| application/vnd.lotus-notes          | Lotus Notes file                 |
| application/vnd.ms-excel             | Excel spreadsheet                |
| application/vnd.ms-powerpoint        | PowerPoint presentation          |
| application/x-gzip                   | Gzip archive                     |
| application/x-java-archive           | JAR file                         |
| application/x-java-serialized-object | Serialized Java object           |
| application/x-java-vm                | Java bytecode (.class) file      |
| application/zip                      | Zip archive                      |
| audio/basic                          | Sound file in .au or .snd format |
| audio/midi                           | MIDI sound file                  |
| audio/x-aiff                         | AIFF sound file                  |
| audio/x-wav                          | Microsoft Windows sound file     |
| image/gif                            | GIF image                        |
| image/jpeg                           | JPEG image                       |
| image/png                            | PNG image                        |
| image/tiff                           | TIFF image                       |
| image/x-xbitmap                      | X Windows bitmap image           |
| text/css                             | HTML cascading style sheet       |
| text/html                            | HTML document                    |
| text/plain                           | Plain text                       |
| text/xml                             | XML                              |
| video/mpeg                           | MPEG video clip                  |
| video/quicktime                      | QuickTime video clip             |



























##  二、Servlet技术详解



1. **JavaEE简介**

JavaEE（Java Enterprise Edition），Java企业版，是一个用于企业级web开发平台,它是一组Specification。最早由Sun公司定制并发布，后由Oracle负责维护。在JavaEE平台规范了在开发企业级web应用中的技术标准。

在JavaEE平台共包含了13个技术规范（随着JavaEE版本的变化所包含的技术点的数量会有增多）。它们分别是：JDBC、JNDI、EJB、RMI、Servlet、JSP、XML、JMS、Java IDL、JPA、JTA、JavaMail和JAF。
* avaEE缺点

1. JavaEE技术使用时过于复杂了。
2. JavaEE技术使用慢，效率过低。
3. JavaEE技术较重，很多技术需要依赖服务器中间件。

*  开源框架优点

1. 高效：开发变得简单，快速，并且有效。
2. 成本：很多框架都是免费，并且开发人员编写代码更快，所以客户成本自然 更低。
3. 支持：框架有文档支持，团队支持，或者大的社区支持，能迅速帮你解决问 题。



2. **服务器简介**
   1. 硬件服务器的构成与一般的PC比较相似，但是服务器在稳定性、安全性、性能等方面都要求更高，因为CPU、芯片组、内存、磁盘系统、网络等硬件和普通PC有所不同。
   2. 软件服务器（英文名称Server），也称伺服器。指一个管理资源并为用户提供服务的计算机软件，通常分为文件服务器、数据库服务器和应用程序服务器。运行以上软件的计算机或计算机系统也被称为服务器。



* 应用服务器是Java EE规范的具体实现, 可以执行/驱动基于JavaEE平台开发的web项目。绝大部分的应用服务器都是付费产品。









### 1、Tomcat

Tomcat服务器是Apache的一个开源免费的Web容器。它实现了JavaEE平台下部分技术规范，属于轻量级应用服务器。

**Tomcat作用**

可以在Tomcat中运行我们所编写的Servlet、JSP

#### 1.1、Tomcat启动与关闭



* **Tomcat启动**

  - 方式一

    运行startup.bat文件。

  - 方式二

    catlina.bat start

    其中catlina.bat是命令文件，start是启动Tomcat参数。

* **Tomcat关闭**

  - 方式一

    运行shutdown.bat文件。

  - 方式二

    catlina.bat stop

    其中catlina.bat是命令文件，stop是关闭Tomcat参数。

  - 方式三

    直接关闭掉控制台窗口。



* **访问Tomcat**

  访问Tomcat的URL格式：

  ```
  http://ip:port
  ```

  访问本机Tomcat的URL格式：

  ```
  http://localhost:8080
  ```









#### 1.2、Tomcat配置文件介绍



Tomcat 的配置文件由4个xml组成，分别是 context.xml、web.xml、server.xml、tomcat-users.xml。每个文件都有自己的功能与配置方法。

**context.xml**

context.xml 是 Tomcat 公用的环境配置。 Tomcat 服务器会定时去扫描这个文件。一旦发现文件被修改（时间戳改变了），就会自动重新加载这个文件，而不需要重启服务器 。

**web.xml**

Web应用程序描述文件，都是关于是Web应用程序的配置文件。所有Web应用的 web.xml 文件的父文件。

**server.xml**

是 tomcat 服务器的核心配置文件，server.xml的每一个元素都对应了 tomcat中的一个组件，通过对xml中元素的配置，实现对 tomcat中的各个组件和端口的配置。

**tomcat-users.xml**

配置访问Tomcat的用户以及角色的配置文件。















#### 1.3、配置Tomcat Manager

**什么是Tomcat Manager**

Tomcat Manager是Tomcat自带的、用于对Tomcat自身以及部署在Tomcat上的应用进行管理的web应用。默认情况下，Tomcat Manager是处于禁用状态的。准确的说，Tomcat Manager需要以用户角色进行登录并授权才能使用相应的功能，不过Tomcat并没有配置任何默认的用户，因此我们需要先进行用户配置后才能使用Tomcat Manager。

**配置Tomcat Manager的访问用户**

Tomcat Manager中没有默认用户，我们需要在tomcat-users.xml文件配置。Tomcat Manager的用户配置需要配置两个部分：角色配置、用户名及密码配置。

**Tomcat Manager中的角色分类**

- manager-gui角色：

  允许访问HTML GUI和状态页面(即URL路径为/manager/html/*)

- manager-script角色：

  允许访问文本界面和状态页面(即URL路径为/manager/text/*)

- manager-jmx角色：

  允许访问JMX代理和状态页面(即URL路径为/manager/jmxproxy/*)

- manager- status角色：

  仅允许访问状态页面(即URL路径为/manager/status/*)

**配置用户及角色**

修改tomcat-users.xml

```

<role rolename ="manager-gui"/> 

<user username ="tomcat" password ="tomcat" roles="manager-gui" />
```

**解除访问限制**

进入Tomcat的webapps目录下，打开webapps/manager/META-INF/context.xml文件，修改下面这段配置

```

<context antiresourcelocking="false" privileged="true">

<!-- 把下面这段注释掉 -->

<!-- <Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> -->

</context>
```

### 2、Tomcat架构讲解



#### 2.1Tomcat工作原理

Tomcat是一个能够处理请求并产生响应的应用程序。Tomcat实现了JavaEE平台下的一些技术规范，所以我们可以在Tomcat中运行我们所编写的Servlet、JSP。

![image-20211217140832023](https://www.itbaizhan.com/wiki/imgs/image-20211217140832023-16397213130081.png)



![image-20211129110419202](https://www.itbaizhan.com/wiki/imgs/image-20211129110419202-16381550601446.png)







#### 2.2Tomcat组件

* **Server组件**

启动一个server实例（即一个JVM进程），它监听在8005端口以接收shutdown命令。Server的定义不能使用同一个端口，这意味着如果在同一个物理机上启动了多个Server实例，必须配置它们使用不同的端口。

```
<Server port="8005" shutdown="SHUTDOWN">
```

port: 接收shutdown指令的端口，默认为8005；

shutdown：发往此Server用于实现关闭tomcat实例的命令字符串，默认为SHUTDOWN；



* **Service组件**

  Service主要用于关联一个引擎和与此引擎相关的连接器，每个连接器通过一个特定的端口和协议接收请求并将其转发至关联的引擎进行处理。困此，Service要包含一个引擎、一个或多个连接器。

  ```
  <Service name="Catalina">
  ```

  name：此服务的名称，默认为Catalina；







* **Connector组件**

​         支持处理不同请求的组件，一个引擎可以有一个或多个连接器，以适应多种请求方式。默认只开启了处理Http协议的连接器。如果需要使用其他协议，需要在Tomcat中配置该协议的连接器。

在Tomcat中连接器类型通常有4种：

1. HTTP连接器
2. SSL连接器
3. AJP 1.3连接器
4. proxy连接器

```
1
<Connector port="8888" protocol="HTTP/1.1"
2
               connectionTimeout="20000"
3
               redirectPort="8443" />
```

port：监听的端口

protocol：连接器使用的协议，默认为HTTP/1.1;

connectionTimeout：等待客户端发送请求的超时时间，单位为毫秒;

redirectPort：如果某连接器支持的协议是HTTP，当接收客户端发来的HTTPS请求时，则转发至此属性定义的端口;

maxThreads：支持的最大并发连接数，默认为200个













* **Engine组件**

Engine是Servlet处理器的一个实例，即servlet引擎，定义在server.xml中的Service标签中。Engine需要defaultHost属性来为其定义一个接收所有发往非明确定义虚拟主机的请求的Host组件。

![image-20211220155616973-16399869780174](https://www.itbaizhan.com/wiki/imgs/image-20211220155616973-16399869780174.png)

```
<Engine name="Catalina" defaultHost="localhost">
```

name：Engine组件的名称;

defaultHost：Tomcat支持基于FQDN(Fully Qualified Domain Name 全限定域名)的虚拟主机，这些虚拟主机可以通过在Engine容器中定义多个不同的Host组件来实现；但如果此引擎的连接器收到一个发往非明确定义虚拟主机的请求时则需要将此请求发往一个默认的虚拟主机进行处理，因此，在Engine中定义的多个虚拟主机的主机名称中至少要有一个跟defaultHost定义的主机名称同名









* **Host组件**

  虚拟主机（英语：virtual hosting）或称共享主机（shared web hosting），又称虚拟服务器，是一种在单一主机或主机群上，实现多网域服务的方法，可以运行多个网站或服务的技术。

  Host组件位于Engine容器中用于接收请求并进行相应处理的虚拟主机。通过该容器可以运行Servlet或者JSP来处理请求。

  ```
  <Host name="localhost"  appBase="webapps" unpackWARs="true" autoDeploy="true">
  ```

  name：虚拟主机的名称，Tomcat通过在请求URL中的域名与name中的值匹配，用于查找能够处理该请求的虚拟主机。如果未找到则交给在Engine中defaultHost指定的主机处理；

  appBase：此Host的webapps目录，即指定存放web应用程序的目录的路径；

  autoDeploy：在Tomcat处于运行状态时放置于appBase目录中的应用程序文件是否自动进行deploy；默认为true；

  unpackWARs：在启用此webapps时是否对WAR格式的归档文件先进行展开；默认为true；







* **Context**

  Context是Host的子组件，代表指定一个Web应用，它运行在某个指定的虚拟主机（Host）上；每个Web应用都是一个WAR文件，或文件的目录。

  ```
  <Context path="/test" docBase="D:\bjsxt\itbaizhan.war" />
  ```

  path：context path既浏览器访问项目的访问路径。

  docBase：相应的Web应用程序的存放位置；也可以使用相对路径，起始路径为此Context所属Host中appBase定义的路径；















#### 2.3 配置虚拟主机(Host)



修改Windows系统中的Host文件做域名与IP的绑定。

**Host文件位置**

C:\Windows\System32\drivers\etc

**修改内容**

```
1
127.0.0.1 itbz.com 
2
127.0.0.1 bjsxt.com 
```

 













### 3、 Servlet详解



#### 3.1、Servlet简介

ervlet是Server Applet的简称，称为服务端小程序，是JavaEE平台下的技术标准，基于Java语言编写的服务端程序。 Web 容器或应用服务器实现了Servlet标准所以Servlet需要运行在Web容器或应用服务器中。Servlet主要功能在于能够在服务器中执行并生成数据。

**Servlet技术特点**

![image-20211220192327125](https://www.itbaizhan.com/wiki/imgs/image-20211220192327125.png)

**Servlet在应用程序中的位置**

![image-20211220192356860](https://www.itbaizhan.com/wiki/imgs/image-20211220192356860.png)









#### 3.2 Tomcat运行过程

![image-20211129143626340](https://www.itbaizhan.com/wiki/imgs/image-20211129143626340-16381677871477.png)

1. 用户访问localhost:8888/test/helloword.do，请求被发送到Tomcat，被监听8888端口并处理 HTTP/1.1 协议的Connector获得。
2. Connector把该请求交给它所在的Service的Engine来处理，并等待Engine的回应。
3. Engine获得请求localhost/test/helloword.do，匹配所有的虚拟主机Host。
4. Engine匹配到名为localhost的Host虚拟主机来处理/test/helloword.do请求（即使匹配不到会请求交给默认Host处理）。
5. 匹配到的Context获得请求/helloword.do。
6. 构造HttpServletRequest对象和HttpServletResponse对象，作为参数调用HelloWorld的doGet（）或doPost（）.执行业务逻辑、数据存储等程序。
7. Context把执行完之后的结果通过HttpServletResponse对象返回给Host。
8. Host把HttpServletResponse返回给Engine。
9. Engine把HttpServletResponse对象返回Connector。
10. Connector把HttpServletResponse对象返回给客户Browser。











#### 3.3、Servlet继承结构

![image-20211129145236600](https://www.itbaizhan.com/wiki/imgs/image-20211129145236600-163816875786912.png)

**Servlet接口**

1. init()，创建Servlet对象后立即调用该方法完成一些初始化工作。
2. service()，处理客户端请求，执行业务操作，利用响应对象响应客户端请求。
3. destroy()，在销毁Servlet对象之前调用该方法，释放资源。
4. getServletConfig()，ServletConfig是容器向servlet传递参数的载体。
5. getServletInfo()，获取servlet相关信息。

**ServletConfig接口**

1. String getServletName()，返回 Servlet 的名字，即 web.xml 中 元素的值。
2. ServletContext getServletContext()，返回一个代表当前 Web 应用的 ServletContext 对象。
3. String getInitParameter(String name)，根据初始化参数名返回对应的初始化参数值。
4. Enumeration getInitParameterNames()，返回一个 Enumeration 对象，其中包含了所有的初始化参数名。

**GenericServle抽象类**

GenericServlet是实现了Servlet接口的抽象类。在GenericServlet中进一步的定义了Servlet接口的具体实现，其设计的目的是为了和应用层协议解耦，在GenericServlet中包含一个Service抽象方法。

**HttpServlet类**

继承自 GenericServlet，针对于处理 HTTP 协议的请求所定制。在 HttpServlet的service() 方法中已经把 ServletReuqest 和 ServletResponse 转为 HttpServletRequest 和 HttpServletResponse。 直接使用 HttpServletRequest 和 HttpServletResponse, 不再需要强转。实际开发中, 直接继承 HttpServlet, 并根据请求方式复写 doXxx() 方法即可。















#### 3.4、Servlet生命周期

Servlet的生命周期是由容器管理的，分别经历三各阶段：

init()：初始化

service()：服务

destroy()：销毁

当客户端浏览器第一次请求Servlet时，容器会实例化这个Servlet，然后调用一次init方法，并在新的线程中执行service方法处理请求。service方法执行完毕后容器不会销毁这个Servlet而是做缓存处理，当客户端浏览器再次请求这个Servlet时，容器会从缓存中直接找到这个Servlet对象，并再一次在新的线程中执行Service方法。当容器在销毁Servlet之前对调用一次destroy方法。





#### 3.5、Servlet处理请求的原理

当浏览器基于get方式请求我们创建Servlet时，我们自定义的Servlet中的doGet方法会被执行。doGet方法能够被执行并处理get请求的原因是，容器在启动时会解析web工程中WEB-INF目录中的web.xml文件，在该文件中我们配置了Servlet与URI的绑定，容器通过对请求的解析可以获取请求资源的URI，然后找到与该URI绑定的Servlet并做实例化处理(注意：只实例化一次，如果在缓存中能够找到这个Servlet就不会再做次实例化处理)。在实例化时会使用Servlet接口类型作为引用类型的定义，并调用一次init方法，由于GenericServlet中重写了该方法所以最终执行的是GenericServlet中init方法(GenericServlet中的Init方法是一个空的方法体)，然后在新的线程中调用service方法。由于在HttpServlet中重写了Service方法所以最终执行的是HttpServlet中的service方法。在service方法中通过request.getMethod()获取到请求方式进行判断如果是Get方式请求就执行doGet方法，如果是POST请求就执行doPost方法。如果是基于GET方式提交的，并且在我们的Servlet中又重写了HttpServlet中的doGet方法，那么最终会根据Java的多态特性转而执行我们自定义的Servlet中的doGet方法。









#### 3.6、Servlet作用

- 获取用户提交的数据



- 获取浏览器附加的信息



- 处理数据（访问数据库或调用接口）



- 给浏览器产生一个响应



- 在响应中添加附加信息









#### 3.7、自启动Servlet

**自启动Servlet特点**

自动启动Servlet表示在Tomcat启动时就会实例化这个Servlet，他的实例化过程不依赖于请求，而是依赖容器的启动。

可以通过在web.xml中的< servlet >标签中通过< load-on-startup >1< /load-on-startup > 配置自启动Servlet。



 









#### 3.8、Servlet线程问题

在Servlet中使用的是多线程方式来执行service()方法处理请求，所以我们在使用Servlet时需要考虑到线程安全问题，在多线程中对于对象中的成员变量是最不安全的，所以不要在Servlet中通过成员变量的方式来存放数据，如果一定要使用成员变量存储数据，在对数据进行操作时需要使用线程同步的方式来解决线程安全问题，避免出现数据张冠李戴现象。

 



















#### 3.9、Servlet 的url-pattern配置

**URL的匹配规则**

**精确匹配**

精确匹配是指中配置的值必须与url完全精确匹配。

```
<servlet-mapping>
    <servlet-name>demoServlet</servlet-name>
    <url-pattern>/demo.do</url-pattern></servlet-mapping>
```

[http://localhost:8888/demo/demo.do](http://localhost:8888/demo.do) 匹配

[http://localhost:8888/demo/suibian/demo.do](http://localhost:8888/demo.do) 不匹配

**扩展名匹配**

在允许使用统配符作为匹配规则，“*”表示匹配任意字符。在扩展名匹配中只要扩展名相同都会被匹配和路径无关。注意，在使用扩展名匹配时在中不能使用“/”，否则容器启动就会抛出异常。

```
<servlet-mapping>
    <servlet-name>demoServlet</servlet-name>
    <url-pattern>*.do</url-pattern>
</servlet-mapping>
```

[http://localhost:8888/demo/abc.do](http://localhost:8888/demo.do) 匹配

[http://localhost:8888/demo/suibian/haha.do](http://localhost:8888/demo.do) 匹配

[http://localhost:8888/demo/abc](http://localhost:8888/demo.do) 不匹配

**路径匹配**

根据请求路径进行匹配，在请求中只要包含该路径都匹配。“*”表示任意路径以及子路径。

```
<servlet-mapping>
    <servlet-name>demoServlet</servlet-name>
    <url-pattern>/suibian/*</url-pattern>
</servlet-mapping>
```

[http://localhost:8888/demo/suibian/haha.do](http://localhost:8888/demo.do) 匹配

[http://localhost:8888/demo/suibian/hehe/haha.do](http://localhost:8888/demo.do) 匹配

[http://localhost:8888/demo/hehe/heihei.do](http://localhost:8888/demo.do) 不匹配

**任意匹配**

匹配“/”。匹配所有但不包含JSP页面。

```
<url-pattern>/</url-pattern>
```

http://localhost:8888/demo/suibian.do 匹配

http://localhost:8888/demo/addUser.html 匹配

http://localhost:8888/demo/css/view.css 匹配

http://localhost:8888/demo/addUser.jsp 不匹配

http://localhost:8888/demo/user/addUser.jsp 不匹配

**匹配所有**

```
<url-pattern>/*</url-pattern>
```

http://localhost:8888/demo/suibian.do 匹配

http://localhost:8888/demo/addUser.html 匹配

http://localhost:8888/demo/suibian/suibian.do 匹配

**优先顺序**

当一个url与多个Servlet的匹配规则可以匹配时，则按照 “ 精确路径 > 最长路径 > 扩展名”这样的优先级匹配到对应的Servlet。

**考考你**

Servlet1 映射到 /abc/*

Servlet2 映射到 /*

Servlet3 映射到 /abc

Servlet4 映射到 *.do

当请求URL为“/abc/a.html”，“/abc/* ”和“/* ”都匹配，Servlet引擎将调用Servlet1。

当请求URL为“/abc”时，“/abc/* ”和“/abc”都匹配，Servlet引擎将调用Servlet3。

当请求URL为“/abc/a.do”时，“/abc/* ”和“ *.do”都匹配，Servlet引擎将调用Servlet1。

当请求URL为“/a.do”时，“/* ”和“*.do”都匹配，Servlet引擎将调用Servlet2。

当请求URL为“/xxx/yyy/a.do”时，“/* ”和“*.do”都匹配，Servlet引擎将调用Servlet2。

 





















#### 3.10、Servlet 的多URL映射方式

在web.xml文件中支持将多个URL映射到一个Servlet中，但是相同的URL不能同时映射到两个Servlet中。

**方式一**

```
<servlet-mapping>
    <servlet-name>demoServlet</servlet-name>
    <url-pattern>/suibian/*</url-pattern>
    <url-pattern>*.do</url-pattern>
</servlet-mapping>
```

**方式二**

```
1<servlet-mapping>
    <servlet-name>demoServlet</servlet-name>
    <url-pattern>/suibian/*</url-pattern>
</servlet-mapping>
<servlet-mapping>
    <servlet-name>demoServlet</servlet-name>
    <url-pattern>*.do</url-pattern>
</servlet-mapping>
```

 







#### 3.11、基于注解式开发Servlet

在Servlet3.0以及之后的版本中支持注解式开发Servlet。对于Servlet的配置不在依赖于web.xml配置文件，而是使用@WebServlet注解完成Servlet的配置。

**@WebServlet**

| 属性名         | 类型           | 作用                            |
| -------------- | -------------- | ------------------------------- |
| initParams     | WebInitParam[] | Servlet的init参数               |
| name           | String         | Servlet的名称                   |
| urlPatterns    | String[]       | Servlet的访问URL，支持多个      |
| value          | String[]       | Servlet的访问URL，支持多个      |
| loadOnStartup  | int            | 自启动Servlet                   |
| description    | String         | Servlet的描述                   |
| displayName    | String         | Servlet的显示名称               |
| asyncSupported | boolean        | 声明Servlet是否支持异步操作模式 |



* **@WebInitParam注解的使用**

  | 属性名      | 类型   | 作用        |
  | ----------- | ------ | ----------- |
  | name        | String | param-name  |
  | value       | String | param-value |
  | description | String | description |

   











 









### 4、HttpServletRequest

#### 4.1、获取请求信息

HttpServletRequest对象代表客户端浏览器的请求，当客户端浏览器通过HTTP协议访问服务器时，HTTP请求中的所有信息都会被Tomcat所解析并封装在这个对象中，通过这个对象提供的方法，可以获得客户端请求的所有信息。



req.getRequestURL()

返回客户端浏览器发出请求时的完整URL。



req.getRequestURI()

返回请求行中指定资源部分。



req.getRemoteAddr()

返回发出请求的客户机的IP地址。



req.getLocalAddr()

返回WEB服务器的IP地址。



req.getLocalPort()

返回WEB服务器处理Http协议的连接器所监听的端口











#### 4.2、获取请求数据

**根据key获取指定value**

req.getParameter("key");

根据key获取对应的value，返回一个字符串。

```
String str = req.getParameter("key");
```

 





#### 4.3、获取复选框(checkbox组件)中的值

eq.getParameterValues("checkboxkey");

获取复选框(checkbox组件)中的值，返回一个字符串数组。

```
String[] userlikes = req.getParameterValues("checkboxkey");
```

 









#### 4.4、获取所有提交数据的key

req.getParameterNames()

获取请求中所有数据的key，该方法返回一个枚举类型。

```
Enumeration<String> parameterNames = req.getParameterNames();
```

 













#### 4.5、使用Map结构获取提交数据

req.getParameterMap()

获取请求中所有的数据并存放到一个Map结构中，该方法返回一个Map，其中key为String类型value为String[]类型。

```
Map<String, String[]> parameterMap = req.getParameterMap()
```











#### 4.6、设置请求编码

req.setCharacterEncoding("utf-8")

请求的数据包基于字节在网络上传输，Tomcat接收到请求的数据包后会将数据包中的字节转换为字符。在Tomcat中使用的是ISO-8859-1的单字节编码完成字节与字符的转换，所以数据中含有中文就会出现乱码，可以通过req.setCharacterEncoding("utf-8")方法来对提交的数据根据指定的编码方式重新做编码处理。

 









#### 4.7、资源访问路径

**绝对路径**

绝对路径访问资源表示直接以”/”作为项目的Context Path。该方式适用于以”/”作为项目的Context Path。

```
<form action="/getInfo.do" method="post">
```

**相对路径**

相对路径访问资源表示会相对于项目的Context Path作为相对路径。该方式适用于为项目指定的具体的Context Path。

```
<form action="getInfo.do" method="post">
```

 







#### 4.8、获取请求头信息

req.getHeader("headerKey")

根据请求头中的key获取对应的value。

```
String headerValue = req.getHeader("headerKey");
```

req.getHeaderNames()

获取请求头中所有的key，该方法返回枚举类型。

```
Enumeration<String> headerNames = req.getHeaderNames();
```

 





#### 4.9HttpServletRequest对象的生命周期

当有请求到达Tomcat时，Tomcat会创建HttpServletRequest对象，并将该对象通过参数的方式传递到我们Servlet的方法中，当处理请求处理完毕并产生响应后该对象生命周期结束。

 

















### 5、HttpServletResponse



HttpServletResponse对象代表服务器的响应。这个对象中封装了响应客户端浏览器的流对象，以及向客户端浏览器响应的响应头、响应数据、响应状态码等信息。

#### 5.1、设置响应类型

resp.setContentType("MIME")

该方法可通过MIME-Type设置响应类型。

| **Type**                             | **Meaning**                      |
| ------------------------------------ | :------------------------------- |
| application/msword                   | Microsoft Word document          |
| application/octet-stream             | Unrecognized or binary data      |
| application/pdf                      | Acrobat (.pdf) file              |
| application/postscript               | PostScript file                  |
| application/vnd.lotus-notes          | Lotus Notes file                 |
| application/vnd.ms-excel             | Excel spreadsheet                |
| application/vnd.ms-powerpoint        | PowerPoint presentation          |
| application/x-gzip                   | Gzip archive                     |
| application/x-java-archive           | JAR file                         |
| application/x-java-serialized-object | Serialized Java object           |
| application/x-java-vm                | Java bytecode (.class) file      |
| application/zip                      | Zip archive                      |
| application/json                     | JSON                             |
| audio/basic                          | Sound file in .au or .snd format |
| audio/midi                           | MIDI sound file                  |
| audio/x-aiff                         | AIFF sound file                  |
| audio/x-wav                          | Microsoft Windows sound file     |
| image/gif                            | GIF image                        |
| image/jpeg                           | JPEG image                       |
| image/png                            | PNG image                        |
| image/tiff                           | TIFF image                       |
| image/x-xbitmap                      | X Windows bitmap image           |
| text/css                             | HTML cascading style sheet       |
| text/html                            | HTML document                    |
| text/plain                           | Plain text                       |
| text/xml                             | XML                              |
| video/mpeg                           | MPEG video clip                  |
| video/quicktime                      | QuickTime video clip             |

 











#### 5.2、设置字符型响应

常见的字符型响应类型：

resp.setContentType("text/html")

设置响应类型为文本型，内容含有html字符串，是默认的响应类型



resp.setContentType("text/plain")

设置响应类型为文本型，内容是普通文本。



resp.setContentType("application/json")

设置响应类型为JSON格式的字符串。



#### 5.3、设置字节型响应

常见的字节型响应：

resp.setContentType("image/jpeg")

设置响应类型为图片类型，图片类型为jpeg或jpg格式。



resp.setContentType("image/gif")

设置响应类型为图片类型，图片类型为gif格式。

 





#### 5.4、设置响应编码

![image-20211230111909354](https://www.itbaizhan.com/wiki/imgs/image-20211230111909354.png?v=1.0.0)

**设置响应编码有两种方式**

1. response.setContentType("text/html; charset=UTF-8");
2. response.setCharacterEncoding("UTF-8");



response.setContentType("text/html;charset=utf-8");

不仅发送到浏览器的内容会使用UTF-8编码，而且还通知浏览器使用UTF-8编码方式进行显示。所以总能正常显示中文

response.setCharacterEncoding("utf-8");

仅仅是发送的浏览器的内容是UTF-8编码的，至于浏览器是用哪种编码方式显示不管。 所以当浏览器的显示编码方式不是UTF-8的时候，就会看到乱码，需要手动指定浏览器编码。











#### 5.5、重定向响应

**重定向响应**

![image-20211230151821391](https://www.itbaizhan.com/wiki/imgs/image-20211230151821391.png?v=1.0.1)

response.sendRedirect(URL地址)

重定向响应会在响应头中添加一个Location的key对应的value是给定的URL。客户端浏览器在解析响应头后自动向Location中的URL发送请求。

**重定向响应特点：**

- 重定向会产生两次请求两次响应。
- 重定向的URL是由客户端浏览器发送的。
- 浏览器地址栏会有变化。

 

















#### 5.6、文件下载

在实现文件下载时，我们需要在响应头中添加附加信息。

```
response.addHeader("Content-Disposition", "attachment; filename="+文件名);
```

Content-Disposition:attachment

该附加信息表示作为对下载文件的一个标识字段。不会在浏览器中显示而是直接做下载处理。

filename=文件名

表示指定下载文件的文件名。

 









### 6、ServletContext

* **ServletContext对象的作用**
  * 相对路径转绝对路径
  * 获取容器的附加信息
  * 读取配置信息

#### 6.1、相对路径转绝对路径

**相对路径转绝对路径**

context.getRealPath("path")

该方法可以将一个相对路径转换为绝对路径，在文件上传与下载时需要用到该方法做路径的转换。

 













#### 6.2、获取容器的附加信息

servletContext.getServerInfo()

返回Servlet容器的名称和版本号



servletContext.getMajorVersion()

返回Servlet容器所支持Servlet的主版本号。



servletContext.getMinorVersion()

返回Servlet容器所支持Servlet的副版本号。

 











#### 6.3、获取web.xml文件中的信息



```
<context-param>
    <param-name>key</param-name>
    <param-value>value</param-value>
</context-param>
```

servletContext.getInitParameter("key")

该方法可以读取web.xml文件中标签中的配置信息。

servletContext.getInitParameterNames()

该方法可以读取web.xml文件中所有param-name标签中的值。

 



















#### 6.4、全局容器

当容器启动时会创建ServletContext对象并一直缓存该对象，直到容器关闭后该对象生命周期结束。ServletContext对象的生命周期非常长，所以在使用全局容器时不建议存放业务数据。









### 7、ServletConfig

ServletConfig对象对应web.xml文件中的节点。当Tomcat初始化一个Servlet时，会将该Servlet的配置信息，封装到一个ServletConfig对象中。我们可以通过该对象读取节点中的配置信息

```
<servlet>
 <servlet-name>servletName</servlet-name>
  <servlet-class>servletClass</servlet-class>
  <init-param>
<param-name>key</param-name>
      <param-value>value</param-value>
    </init-param>
</servlet>
```



servletConfig.getInitParameter("key")

该方法可以读取web.xml文件中标签中标签中的配置信息。



servletConfig.getInitParameterNames()

该方法可以读取web.xml文件中当前标签中所有标签中的值。

 







### 8、Cookie和HttpSession对象

Cookie对象与HttpSession对象的作用是维护客户端浏览器与服务端的会话状态的两个对象。由于HTTP协议是一个无状态的协议，所以服务端并不会记录当前客户端浏览器的访问状态，但是在有些时候我们是需要服务端能够记录客户端浏览器的访问状态的，如获取当前客户端浏览器的访问服务端的次数时就需要会话状态的维持。在Servlet中提供了Cookie对象与HttpSession对象用于维护客户端与服务端的会话状态的维持。二者不同的是Cookie是通过客户端浏览器实现会话的维持，而HttpSession是通过服务端来实现会话状态的维持



#### 8.1、Cookkie对象的特点

- Cookie使用字符串存储数据
- Cookie使用Key与Value结构存储数据
- 单个Cookie存储数据大小限制在4097个字节
- Cookie存储的数据中不支持中文，Servlet4.0中支持
- Cookie是与域名绑定所以不支持跨一级域名访问
- Cookie对象保存在客户端浏览器内存或系统磁盘中
- Cookie分为持久化Cooke与状态Cookie
- 浏览器在保存同一域名所返回Cookie的数量是有限的。不同浏览器支持的数量不同，Chrome浏览器为50个
- 浏览器每次请求时都会把与当前访问的域名相关的Cookie在请求中提交到服务端。

 



#### 8.2、Cookie对象的创建

Cookie cookie = new Cookie("key","value")

通过new关键字创建Cookie对象



response.addCookie(cookie)

通过HttpServletResponse对象将Cookie写回给客户端浏览器。

 





#### 8.3、获取Cookie中的数据

浏览器每次请求时都会把与当前访问的域名相关的Cookie在请求中提交到服务端。通过HttpServletRequest对象获取Cookie，返回Cookie数组。

Cookie[] cookies = request.getCookies()



* **解决Cookie不支持中文**

 在Cookie中name的值不能使用中文，Value是可以的。但是在Servlet4.0版本之前Cookie中的Value也是不支持中文存储的，如果存储的数据中含有中文，代码会直接出现异常。我们可以通过对含有中文的数据重新进行编码来解决该问题。在Servlet4.0中的Cookie的Value开始支持中文存储。

```
1
java.lang.IllegalArgumentException: Control character in cookie value or attribute.
```



* **URLEncoder.encode("content","code")**

将内容按照指定的编码方式做URL编码处理。



**URLDecoder.decode("content","code")**

将内容按照指定的编码方式做URL解码处理。

 





#### 8.4、Cookie跨域问题

域名分类：域名分为顶级域、顶级域名(一级域名)、二级域名。

![image-20211130152350077](https://www.itbaizhan.com/wiki/imgs/image-20211130152350077-16382570309732.png?v=1.0.0)

域名等级的区别：一级域名比二级域名更高级，二级域名是依附于一级域名之下的附属分区域名，即二级域名是一级域名的细化分级。例如：baidu.com 为一级域名，news.baidu.com为二级域名。

Cookie不支持一级域名的跨域，支持二级域名的跨域。

![image-20220109112222622](https://www.itbaizhan.com/wiki/imgs/image-20220109112222622.png?v=1.0.0)



* **状态Cookie与持久化Cookie**

状态Cookie：Cookie对象仅会被缓存在浏览器所在的内存中。当浏览器关闭后Cookie对象 也会被销毁。

持久化Cookie：浏览器会对Cookie做持久化处理，基于文件形式保存在系统的指定目录中。在Windows10系统中为了安全问题不会显示Cookie中的内容。

当Cookie对象创建后默认为状态Cookie。可以使用Cookie对象下的cookie.setMaxAge(60)方法设置失效时间，单位为秒。一旦设置了失效时间，那么该Cookie为持久化Cookie，浏览器会将Cookie对象持久化到磁盘中。当失效时间到达后文件删除。

 

* **通过Cookie实现客户端与服务端会话的维持**

  需求：当客户端浏览器第一次访问Servlet时响应“您好，欢迎您第一次访问！”，第二次访问时响应“欢迎您回来！”。

* ### Cookie总结

  Cookie对于存储内容是基于明文的方式存储的，所以安全性很低。不要在Cookie中存放敏感数据。在数据存储时，虽然在Servlet4.0中Cookie支持中文，但是建议对Cookie中存放的内容做编码处理，也可提高安全性。

   



#### 8.5、HttpSession对象的特点

- HttpSession保存在服务端
- HttpSession使用Key与Value结构存储数据
- HttpSession的Key是字符串类型，Value则是Object类型
- HttpSession存储数据大小无限制

 



* **HttpSession对象的创建**

  ​         HttpSession对象的创建是通过request.getSession()方法来创建的。客户端浏览器在请求服务端资源时，如果在请求中没有jsessionid，getSession()方法将会为这个客户端浏览器创建一个新的HttpSession对象，并为这个HttpSession对象生成一个jsessionid，在响应中通过状态Cookie写回给客户端浏览器，如果在请求中包含了jsessionid，getSession()方法则根据这个ID返回与这个客户端浏览器对应的HttpSession对象。

  ​         getSession()方法还有一个重载方法getSession(true|false)。当参数为true时与getSession()方法作用相同。当参数为false时则只去根据jsessionid查找是否有与这个客户端浏览器对应的HttpSession，如果有则返回，如果没有jsessionid则不会创建新的HttpSession对象。

   







#### 8.6、HttpSession对象的使用

session.setAttribute("key",value)

将数据存储到HttpSession对象中



Object value = session.getAttribute("key")

根据key获取HttpSession中的数据，返回Object



Enumeration attributeNames = session.getAttributeNames()

获取HttpSession中所有的key，返回枚举类型



session.removeAttribute("key")

根据key删除HttpSession中的数据



String id = session.getId()

根据获取当前HttpSession的SessionID，返回字符串类型

 



* **HttpSession的销毁方式**

  HttpSession的销毁方式有两种：

  - 通过web.xml文件指定超时时间
  - 通过HttpSession对象中的invalidate()方法销毁当前HttpSession对象

  

  我们可以在web.xml文件中指定HttpSession的超时时间，当到达指定的超时时间后，容器就会销该HttpSession对象，单位为分钟。该时间对整个web项目中的所有HttpSession对象有效。时间的计算方式是根据最后一次请求时间作为起始时间。只要用户继续访问，服务器就会更新HttpSession的最后访问时间，并维护该HttpSession。用户每访问服务器一次，无论是否读写HttpSession，服务器都认为该用户的HttpSession"活跃（active）"了一次,销毁时间则会重新计算。如果有哪个客户端浏览器对应的HttpSession的失效时间已到，那么与该客户端浏览器对应的HttpSession对象就会被销毁。其他客户端浏览器对应的HttpSession对象会继续保存不会被销毁。

  ```
  <session-config>
     <session-timeout>1</session-timeout>
  </session-config>
  ```

  我们也可以在Tomcat的web.xml文件中配置HttpSession的销毁时间。如果在Tomcat的web.xml文件中配置了HttpSession的超时时间对应的是Tomcat中所有的Web项目都有效。相当于配置了全局的HttpSession超时时间。如果我们在Web项目中配置了超时时间，那么会以Web项目中的超时时间为准。

  ![image-20211130152931416](https://www.itbaizhan.com/wiki/imgs/image-20211130152931416-16382573723243.png?v=1.0.0)

  invalidate()方法是HttpSession对象中所提供的用于销毁当前HttpSession的方法。我们通过调用该方法可以销毁当前HttpSession对象。

   

















#### 8.7、HttpSession生命周期

在HttpSession对象生命周期中没有固定的创建时间与销毁时间。何时创建取决于我们什么时候第一次调用了getSession()或getSession(true)的方法。HttpSession对象的销毁时间取决于超时时间的到达以及调用了invalidate()方法。如果没有超时或者没有调用invalidate()方法，那么HttpSession会一直存储。默认超时时间为30分钟(Tomcat的web.xml文件配置的时间就是默认超时时间)。









#### HttpSession对象总结

**HttpSession与Cookie的区别：**

- cookie数据存放在客户的浏览器或系统的文件中，而HttpSession中的数据存放在服务器中。
- cookie不安全，而HttSession是安全的。
- 单个cookie保存的数据不能超过4K，很多浏览器都限制一个域名保存cookie的数量。而HttpSession没有容量以及数量的限制。

**HttpSession的使用建议**

HttpSession对象是保存在服务端的，所以安全性较高。我们可以在HttpSession对象中存储数据，但是由于HttpSession对象的生命周期不固定，所以不建议存放业务数据。一般情况下我们只是存放用户登录信息。

 



### 9、文件上传

在Servlet3.0之前的版本中如果实现文件上传需要依赖apache的Fileupload组件，在Servlet3.0以及之后的版本中提供了Part对象处理文件上传，所以不在需要额外的添加Fileupload组件。

在Servlet3.0以及之后的版本中实现文件上传时必须要在Servlet中开启多参数配置：

web.xml

```
<multipart-config>
    <file-size-threshold></file-size-threshold>
    <location></location>
    <max-file-size></max-file-size>
    <max-request-size></max-request-size>
</multipart-config>
```

| 元素名 | 类型   | 描述                                                         |
| ------ | ------ | ------------------------------------------------------------ |
|        | int    | 当数据量大于该值时，内容将被写入临时文件。                   |
|        | String | 存放生成的临时文件地址                                       |
|        | long   | 允许上传的文件最大值（byte）。默认值为 -1，表示没有限制      |
|        | long   | 一个 multipart/form-data请求能携带的最大字节数（byte），默认值为 -1，表示没有限制。 |

@MultipartConfig

| 属性名            | 类型   | 描述                                                         |
| ----------------- | ------ | ------------------------------------------------------------ |
| fileSizeThreshold | int    | 当数据量大于该值时，内容将被写入临时文件。                   |
| location          | String | 存放生临时成的文件地址                                       |
| maxFileSize       | long   | 允许上传的文件最大值（byte）。默认值为 -1，表示没有限制      |
| maxRequestSize    | long   | 一个 multipart/form-data请求能携带的最大字节数（byte），默认值为 -1，表示没有限制。 |

Part对象中常用的方法

long getSize()

上传文件的大小



String getSubmittedFileName()

上传文件的原始文件名



String getName()

获取<input name="upload" ...>标签中name属性值



InputStream getInputStream()

获取上传文件的输入流



void write(String path)

保存文件至服务器

 







### 10、Filter过滤器

#### 10.1、过滤器作用

Filter过滤器是Servlet2.3中所提供的一个过滤请求与响应的对象。

Filter过滤器既可以对客户端向服务器端发送的请求进行过滤，也可以对服务器端向客户端产生的响应进行过滤处理。

![image-20211130154214316](https://www.itbaizhan.com/wiki/imgs/image-20211130154214316-16382581352534.png?v=1.0.0)

**Filter对象的创建**

创建一个Class实现Filter接口，并实现接口中三个抽象方法。

init()方法：初始化方法，在创建Filter后立即调用。可用于完成初始化动作。

doFilter()方法：拦截请求与响应方法，可用于对请求和响应实现预处理。

destroy()方法：销毁方法，在销毁Filter之前自动调用。可用于完成资源释放等动作。











#### 10.2、FilterConfig对象的使用

FilterConfig对象是用来读取中初始化参数的对象。该对象通过参数传递到init方法中，用于读取初始化参数。



filterConfig.getInitParameter("name")

通过name获取对应的value。



filterConfig.getInitParameterNames()

返回该Filter中所有中的值。

 









#### 10.3、FilterChain(过滤器链)

Filter技术的特点是在对请求或响应做预处理时，可实现“插拔式”的程序设计。我们可以根据自己需求添加多个Filter，也可以根据需求去掉某个Filter，通过修改web.xml文件即可实现。那么如果有多个过滤器对某个请求及响应进行过滤，那么这组过滤器就称为过滤器链。

**Filter执行顺序**

则按照在web.xml文件中配置的上下顺序来决定先后。在上的先执行，在下的后执行。

![image-20211130154726146](https://www.itbaizhan.com/wiki/imgs/image-20211130154726146-16382584471745.png?v=1.0.0)



























#### 10.4、基于注解式开发Filter

Filter支持注解式开发，通过@WebFilter注解替代web.xml中Filter的配置。

| 属性名      | 类型           | 作用                                    |
| ----------- | -------------- | --------------------------------------- |
| filterName  | String         | 指定过滤器的 name 属性                  |
| urlPatterns | String[]       | 拦截请求的URL，支持多个                 |
| value       | String[]       | 拦截请求的URL，支持多个                 |
| description | String         | 过滤器的描述                            |
| displayName | String         | 过滤器的显示名称                        |
| initParams  | WebInitParam[] | 指定一组过滤器初始化参数，等价于 标签。 |

使用注解式开发Filter时，执行顺序会根据Filter的名称进行排序的结果决定调用的顺序。

 









#### 10.5、Filter的生命周期

Filter的生命周期是由容器管理的。当容器启动时会实例化Filter并调用init方法完成初始化动作。当客户端浏览器发送请求时，容器会启动一个新的线程来处理请求，如果请求的URL能够被过滤器所匹配，那么则先调用过滤器中 的doFilter方法，再根据是否有chain.doFilter的指令，决定是否继续请求目标资源。当容器关闭时会销毁Filter对象，在销毁之前会调用destroy方法。







### 11、Listener监听器

监听器用于监听web应用中某些对象的创建、销毁、增加，修改，删除等动作的发生，然后作出相应的响应处理。当范围对象的状态发生变化的时候，服务器会自动调用监听器对象中的方法。

**监听器分类**

按监听的对象划分，可以分为：

- ServletContext对象生命周期监听器与属性操作监听器;
- HttpSession对象生命周期监听器与属性操作监听器;
- ServletRequest对象生命周期监听器与属性操作监听器;



#### 11.1、ServletContext对象的生命周期监听器

ServletContextListener接口定义了ServletContext对象生命周期的监听行为。



void contextInitialized(ServletContextEvent sce)

ServletContext对象创建之后会触发该监听方法，并将ServletContext对象传递到该方法中。



void contextDestroyed(ServletContextEvent sce)

ServletContext对象在销毁之前会触发该监听方法，并将ServletContext对象传递到该方法中。

 











#### 11.2、ServletContext对象的属性操作监听器

ServletContextAttributeListener接口定义了对于ServletContext对象属性操作的监听行为。



void attributeAdded(ServletContextAttributeEvent scae)

向ServletContext对象中添加属性时会触发该监听方法，并将ServletContext对象传递到该方法中。触发事件的方法servletContext.setAttribute("key","value")。

void attributeRemoved(ServletContextAttributeEvent scae)

当从ServletContext对象中删除属性时会触发该监听方法，并将ServletContext对象传递到该方法中。触发事件方法servletContext.removeAttribute("key")。



void attributeReplaced(ServletContextAttributeEvent scae)

当从ServletContext对象中属性的值发生替换时会触发该监听方法，并将ServletContext对象传递到该方法中。触发事件的方法servletContext.setAttribute("key","value")。











#### 11.3、HttpSession对象的生命周期监听器

HttpSessionListener接口定义了HttpSession对象生命周期的监听行为。



void sessionCreated(HttpSessionEvent se)

HttpSession对象创建后会触发该监听方法，并将已创建HttpSession对象传递到该方法中。



void sessionDestroyed(HttpSessionEvent se)

HttpSession对象在销毁之前会触发该监听方法，并将要销毁的HttpSession对象传递到该方法中。

 









#### 11.4、HttpSession对象的属性操作的监听器

HttpSessionAttributeListener接口定义了对于HttpSession对象属性操作的监听行为。



void attributeAdded(HttpSessionBindingEvent se)

向HttpSession对象中添加属性时会触发该监听方法，并将HttpSession对象传递到该方法中。触发事件的方法HttpSession.setAttribute("key","value")。





void attributeRemoved(HttpSessionBindingEvent se)

当从HttpSession对象中删除属性时会触发该监听方法，并将HttpSession对象传递到该方法中。触发事件方法HttpSession.removeAttribute("key")。





void attributeReplaced(HttpSessionBindingEvent se)

当从HttpSession对象中属性的值发生替换时会触发该监听方法，并将HttpSession对象传递到该方法中。触发事件的方法HttpSession.setAttribute("key","value")。



####  11.5、HttpServletRequest对象的生命周期监听器

ServletRequestListener接口定义了ServletRequest(是HttpServletRequest接口的父接口类型)对象生命周期的监听行为。



void requestInitialized(ServletRequestEvent sre)

HttpServletRequest对象创建后会触发该监听方法，并将已创建HttpServletRequest对象传递到该方法中。



void requestDestroyed(ServletRequestEvent sre)

HttpServletRequest对象在销毁之前会触发该监听方法，并将要销毁HttpServletRequest对象传递到该方法中。

 

####  11.6、HttpServletRequest对象的属性操作监听器

ServletRequestAttributeListener接口定义了对于HttpServletRequest对象属性操作的监听行为。



void attributeAdded(ServletRequestAttributeEvent srae)

向HttpServletRequest对象中添加属性时会触发该监听方法，并将HttpServletRequest对象传递到该方法中。触发事件的方法HttpServletRequest.setAttribute("key","value")。





void attributeRemoved(ServletRequestAttributeEvent srae)

当从HttpServletRequest对象中删除属性时会触发该监听方法，并将HttpServletRequest对象传递到该方法中。触发事件方法HttpServletRequest.removeAttribute("key")。





void attributeReplaced(ServletRequestAttributeEvent srae)

当从HttpServletRequest对象中属性的值发生替换时会触发该监听方法，并将HttpServletRequest对象传递到该方法中。

触发事件的方法HttpServletRequest.setAttribute("key","value")。

 









#### 11.7、基于注解式开发监听器



Listener支持注解式开发，通过@WebListener注解替代web.xml中Listener的配置。

 





















**Filter与Listener设计模式**

“知其然，知其所以然”。

* **Filter的设计模式**

在Servlet的Filter中使用的责任链设计模式。

**责任链模式特点**

责任链（Chain of Responsibility）：责任链模式也叫职责链模式，是一种对象行为模式。在责任链模式里，很多对象由每一个对象对其下一个对象的引用而连接起来形成一条链。请求在这个链上传递，直到链上的某一个对象决定处理此请求。发出这个请求的客户端并不需要知道链上的哪一个对象最终处理这个请求，这使得系统可以在不影响客户端的情况下动态地重新组织链和分配责任。

**责任链的优缺点**

优点：

- 降低了对象之间的耦合度。
- 增强了系统的可扩展性。
- 增强了给对象指派职责的灵活性。
- 责任链简化了对象之间的连接。
- 责任分担。每个类只需要处理自己该处理的工作。



缺点：

- 不能保证请求一定被接收。
- 对比较长的责任链，请求的处理可能涉及多个处理对象，系统性能将受到一定影响。
- 可能会由于责任链的错误设置而导致系统出错，如可能会造成循环调用。

* **Listener的设计模式**

在Servlet的Listener中使用的观察者设计模式。

**观察者模式的特点**

观察者模式（Observer Pattern）：观察者模式是一种对象行为模式。它定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。

**观察者模式的优缺点**

优点：

- 观察者和被观察者是抽象耦合的。
- 建立一套触发机制。



缺点：

- 如果一个被观察者对象有很多的直接和间接的观察者的话，将所有的观察者都通知到会花费很多时间。
- 如果在观察者和观察目标之间有循环依赖的话，观察目标会触发它们之间进行循环调用，可能导致系统崩溃。
- 观察者模式没有相应的机制让观察者知道所观察的目标对象是怎么发生变化的，而仅仅只是知道观察目标发生了变化。



















## 三、JSP技术详解



### 1、JSP基本使用

#### 1.1、JSP简介

JSP(全称Java Server Pages)Java服务端页面技术，是JavaEE平台下的技术规范。它允许使用特定的标签在HTML网页中插入Java代码，实现动态页面处理，所以JSP就是HTML与Java代码的复合体。JSP技术可以快速的实现一个页面的开发，相比在Servlet中实现页面开发将变得更加容易。

**常见的视图层技术**

HTML、JSP、Thymeleaf等。

**前后端分离开发方式**

在前后端分离的项目中真正可以做到“术业有专攻”（开发人员分离） 。前后端分离开发方式中前端页面由专业团队完成页面的开发，并通过请求调用后端的api接口进行数据交互。在开发前端页面的团队中更多关注的技术如：html、CSS、jQuery、Vue、Nodejs等前端技术。前端追求的是：页面表现，速度流畅，兼容性，用户体验等等。而后端团队则更多的是业务的具体实现。在后端开发的团队中更多关注的技术如：设计模式、分布式架构、微服务架构、数据库的操作、Java的性能优化以及数据库优化等技术。前后端分离已成为互联网项目开发的业界标准使用方式，特别是为大型分布式架构、弹性计算架构、微服务架构、多端化服务（多种客户端，例如：浏览器，车载终端，安卓，IOS等等）打下坚实的基础









#### 1.2、JSP运行原理

![image-20220119161230525](https://www.itbaizhan.com/wiki/img/image-20220119161230525.png?v=1.0.2)

**JSP技术特点**

JSP和Servlet是本质相同的技术。当一个JSP文件第一次被请求时，JSP引擎会将该JSP编译成一个Servlet，并执行这个Servlet。如果JSP文件被修改了，那么JSP引擎会重新编译这个JSP。

JSP引擎对JSP编译时会生成两个文件分别是.java的源文件以及编译后的.class文件，并放到Tomcat的work目录的Catalina对应的虚拟主机目录中的org\apache\jsp目录中。两个文件的名称会使用JSP的名称加”_jsp”表示。如：index_jsp.java、index_jsp.class

**JSP与Servlet区别**

- JSP以源文件形式部署到容器中。而Servlet需要编译成class文件后部署到容器中。
- JSP部署到web项目的根目录下或根目录下的其他子目录和静态同资源位于相同位置。而Servlet需要部署到WEB-INF/classes目录中。
- JSP中的HTML代码会被JSP引擎放入到Servlet的out.write()方法中。而在Servlet中我们需要自己通过对字符流输出流的操作生成响应的页面。
- JSP更擅长表现于页面显示，Servlet更擅长于逻辑控制。











#### 1.3、JSP声明标签

 JSP的原始标签在JSP的任何版本中都可以使用。

**<%! %> 声明标签**

声明标签用于在JSP中定义成员变量与方法的定义。标签中的内容会出现在JSP被编译后的Servlet的class的{}中。

 

















#### 1.4、JSP脚本标签

**<% %>脚本标签**

脚本标签用于在JSP中编写业务逻辑。标签中的内容会出现在JSP被编译后的Servlet的_jspService方法体中。

 



















#### 1.5、JSP赋值标签

**<%= %>赋值标签**

赋值标签用于在JSP中做内容输出。标签中的内容会出现在_jspService方法的out.print()方法的参数中。注意我们在使用赋值标签时不需要在代码中添加 ”；”。

 



































### 2、JSP指令标签标签

JSP指令标签的作用是声明JSP页面的一些属性和动作。

<%@指令名称 属性="值" 属性="值1,值2...."%>

* **JSP指令标签分类**

![image-20220119171340734](https://www.itbaizhan.com/wiki/img/image-20220119171340734.png)

* **Page指令标签**

contentType

设置响应类型和编码。

pageEncoding

设置页面的编码。

import

导入所需要的包。

language

当前JSP页面里面可以嵌套的语言。

session

设置JSP页面是否获取session内置对象。

buffer

设置JSP页面的流的缓冲区的大小。

autoFlush

是否自动刷新。

exends

声明当前JSP的页面继承于那个类.必须继承的是httpservlet 及其子类。

isELIgnored

是否忽略el表达式。

errorPage

当前JSP页面出现异常的时候要跳转到的JSP页面。

isErrorPage

当前JSP页面是否是一个错误页面。若值为true,可以使用JSP页面的一个内置对象 exception。

* **Include指令标签**

静态包含,可以将其他页面内容包含进来,一起进行编译运行.生成一个java文件.

<%@include file="被包含JSP的相对路径" %>

* **Taglib指令标签**

导入标签库。

<%@taglib prefix="前缀名" uri="名称空间" %>

 



### 3、JSP内置对象

JSP中一共预先定义了9个这样的对象，分别为：request、response、session、application、out、pagecontext、config、page、exception。

#### 3.1request对象

request 对象是 HttpServletRequest类型的对象。





#### 3.2、response对象

response 对象是 HttpServletResponse类型的对象。





#### 3.3、session对象

session 对象是HttpSession类型的对象。只有在包含 session=“true” 的页面中才可以被使用。





#### 3.4、application对象

application 对象是ServletContext类型的对象。





#### 3.5、out 对象

out 对象是JspWriter类型的对象。





#### 3.6、config 对象

config 对象是ServletConfig类型的对象。





#### 3.7、pageContext 对象

pageContext 对象是PageContext类型的对象。作用是取得任何范围的参数，通过它可以获取 JSP页面的out、request、reponse、session、application 等对象。pageContext对象的创建和初始化都是由容器来完成的，在JSP页面中可以直接使用 pageContext对象。





#### 3.8、page 对象

page 对象代表JSP本身。





#### 3.9、exception 对象

exception 对象的作用是显示异常信息，只有在包含 isErrorPage="true" 的页面中才可以被使用。

 









### 4、请求转发

**什么是请求转发**

请求转发是服务端的一种请求方式，相当于在服务端中直接请求某个资源。

RequestDispatcher dispatcher = request.getRequestDispatcher("/test.jsp");

dispatcher.forward(request,response);

简写方式：

request.getRequestDispatcher("/test.jsp").forword(request,response);

![image-20220119173733128](https://www.itbaizhan.com/wiki/img/image-20220119173733128.png)

**请求转发与重定向的区别**

- 请求转发对于客户端浏览器而言是在一次请求与响应中完成，而重定向是在两次请求两次响应中完成。
- 请求转发并不会改变客户端浏览器的地址栏中的内容。而重定向会改变客户端浏览器地址栏中的内容。
- 请求转发可以使用request对象传递数据，而重定向不能使用request对象传递数据。
- 如果是处理的DML操作，建议使用重定向方式为客户端浏览器产生响应，可以解决表单重复提交现象。

 





### 5、JSP中的四大作用域对象

作用域：“数据共享的范围”，也就是说数据能够在多大的范围内有效。

| 对象名称    | 作用范围         |
| ----------- | ---------------- |
| application | 整个应用都有效   |
| session     | 在当前会话中有效 |
| request     | 在当前请求中有效 |
| page        | 在当前页面有效   |

 



### 6、JSTL标签库介绍

#### 6.1、什么式JSTL标签库

STL（Java server pages standard tag library，即JSP标准标签库）JSTL标签是基于JSP页面的。这些标签可以插入在JSP代码中，本质上JSTL也是提前定义好的一组标签，这些标签封装了不同的功能，在页面上调用标签时，就等于调用了封装起来的功能。JSTL的目标是使JSP页面的可读性更强、简化JSP页面的设计、实现了代码复用、提高效率。

在JSP2.0版本后开始支持JSTL标签库。在使用JSTL标签库时需要在JSP中添加对应的taglib指令标签。

```
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```







#### 6.2、核心标签

最常用、最重要，也是最基本的标签

```
1
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```

![image-20220120152844065](https://www.itbaizhan.com/wiki/img/image-20220120152844065.png)

#### 6.3、格式化标签

JSTL格式化标签用来格式化并输出文本、日期、时间、数字。

```
1
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
```

![image-20220120152940430](https://www.itbaizhan.com/wiki/img/image-20220120152940430.png)

#### 6.4、SQL标签

JSTL SQL标签库提供了与关系型数据库（Oracle，MySQL，SQL Server等等）进行交互的标签。

```
1
<%@ taglib prefix="sql" uri="http://java.sun.com/jsp/jstl/sql" %>
```

![image-20220120153016090](https://www.itbaizhan.com/wiki/img/image-20220120153016090.png)

#### 6.5、XML标签

JSTL XML标签库提供了创建和操作XML文档的标签。

```
<%@ taglib prefix="x" uri="http://java.sun.com/jsp/jstl/xml" %>
```

![image-20220120153054421](https://www.itbaizhan.com/wiki/img/image-20220120153054421.png)

#### 6.6、JSTL函数

JSTL包含一系列标准函数，大部分是通用的字符串处理函数。

```
1
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>
```

![image-20220120153140226](https://www.itbaizhan.com/wiki/img/image-20220120153140226.png)

 











### 7、EL表达式介绍

**什么是EL表达式**

EL（Expression Language）是一种表达式语言。是为了使JSP写起来更加简单，减少java代码，可以使得获取存储在Java对象中的数据变得非常简单。在JSP2.0版本后开始支持EL表达式。

**语法结构**

${表达式}

${对象.属性名}

**EL表达式中的操作符

| 操作符     | 描述             |
| ---------- | ---------------- |
| ( )        | 优先级           |
| +          | 加               |
| -          | 减或负           |
| *          | 乘               |
| / 或div    | 除               |
| % 或 mod   | 取模             |
| == 或 eq   | 测试是否相等     |
| != 或 ne   | 测试是否不等     |
| < 或 lt    | 测试是否小于     |
| > 或gt     | 测试是否大于     |
| <= or le   | 测试是否小于等于 |
| >= 或 ge   | 测试是否大于等于 |
| && 或 and  | 测试逻辑与       |
| \|\| 或 or | 测试逻辑或       |
| ! 或not    | 测试取反         |
| empty      | 测试是否空值     |

**EL表达式的隐含对象**

| 隐含对象         | 描述                          |
| ---------------- | ----------------------------- |
| pageScope        | page 作用域                   |
| requestScope     | request 作用域                |
| sessionScope     | session 作用域                |
| applicationScope | application 作用域            |
| param            | Request 对象的参数，字符串    |
| paramValues      | Request对象的参数，字符串集合 |
| header           | HTTP 信息头，字符串           |
| headerValues     | HTTP 信息头，字符串集合       |
| initParam        | 上下文初始化参数              |
| cookie           | Cookie值                      |
| pageContext      | 当前页面的pageContext         |

**使用EL表达式取出作用域中的值**

${pageScope.name}

${requestScope.name}

${sessionScope.name}

${applicationScope.name}

获取作用域属性中的数据时，也可以只写属性名，EL表达式会按照pageScope、requestScope、sessionScope、applicationScope的顺序查找该属性的值。

${name}





### 8、JSTL标签库的与EL表达式的使用

#### 8.1、JSTL标签库的使用步骤

添加jstl.jar

在JSP页面中添加taglib指令标签。

**JSTL核心标签的使用**

~~~
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
~~~







#### 8.2、if标签的使用

**< c:if >**

标签判断表达式的值，如果表达式的值为 true 则执行其主体内容。









#### 8.3、c标签中的choose标签的使用

**< c:choose >, < c:when >, < c:otherwise >**

< c:choose >标签与Java switch语句的功能一样，用于在众多选项中做出选择。

switch语句中有case，而< c:choose >标签中对应有< c:when >，switch语句中有default，而< c:choose >标签中有< c:otherwise >。











#### 8.5、标签中的foreach标签的使用 

**< c:forEach >**

迭代器，用于迭代集合。

| 属性      | 描述                       |
| --------- | -------------------------- |
| items     | 被迭代的集合               |
| begin     | 迭代器的起始因子           |
| end       | 迭代器的结束因子           |
| step      | 迭代因子的增长数           |
| var       | 代表当前迭代元素的变量名称 |
| varStatus | 代表循环状态的变量名称     |

varStatus 属性

current: 当前这次迭代的（集合中的）项

index: 当前这次迭代从 0 开始的迭代索引

count: 当前这次迭代从 1 开始的迭代计数

first: 用来表明当前这轮迭代是否为第一次迭代的标志

last: 用来表明当前这轮迭代是否为最后一次迭代的标志

begin: 属性值

end: 属性值

step: 属性值











### 9、JSTL格式化标签的使用

<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>

**对日期的格式化处理**

```
<fmt:formatDate value="${data}" pattern="yyyy-MM-dd"/>
```

**对数字格的式化处理**

```
<fmt:formatNumber value="${balance}" type="currency"/>
```

 











### 10、MVC模式

**什么是MVC模式**

MVC模式：Model、View、Controller即模型、视图、控制器。是软件的一种架构模式（Architecture pattern）。MVC要实现的目标是将软件的用户界面和业务逻辑分离，可提高代码可扩展性、可复用性、可维护性、以及灵活性。

View(视图)：用户的操作界面。如：html、jsp。

Model(模型)：具体的业务模型与数据模型。如：service、dao、pojo。

Controller(控制)：处理从视图层发送的请求，并选取模型层的业务模型完成响应的业务实现，并产生响应。如：Servlet。

![image-20220117165538100](https://www.itbaizhan.com/wiki/img/image-20220117165538100-16424097395192.png)

**MVC模式与应用程序分层的区别**

MVC模式是一种软件的架构方式，而应用程序分层这是一种代码的组织方式。MVC模式与应用程序分层的目标都是一致的：为了解耦和、提高代码复用性。

![image-20220117170304629](https://www.itbaizhan.com/wiki/img/image-20220117170304629-16424101864863.png)





## 四、XML技术



### 1、XML介绍

#### 1.1、XML概述

**概念**

XML（Extensible Markup Language）：可扩展标记语言

可扩展：标签都是自定义的。

**发展历程**

HTML和XML都是W3C（万维网联盟）制定的标准，最开始HTML的语法过于松散，于是W3C制定了更严格的XML语法标准，希望能取代HTML。但是程序员和浏览器厂商并不喜欢使用XML，于是现在的XML更多的用于配置文件及传输数据等功能。

> 是谁造成的HTML语法松散？
>
> 浏览器厂商。最开始W3C制定HTML的时候语法还是比较严格的。但浏览器厂商为了抢占市场，语法错误也可以解析成功HTML，最后“内卷”到HTML即使语法非常混乱也是可以被浏览器解析。
>
> ![image-20220528165807275](https://www.itbaizhan.com/wiki/imgs/image-20220528165807275.png)
>
> tips：归根到底是语法的**制定者**和**使用者**不一致造成了HTML语法混乱，JAVA语法严格就是因为java语言的运行工具java虚拟机也是sun公司（现在是oracle）出品的，语法不通过不让运行。
>
> 为什么程序员不使用XML写前端页面？
>
> 因为程序员松散惯了，不想写很严格的代码。同样挣一万块钱，谁会从每月上一天班的公司跳槽到996的公司呢？









#### 1.2、XML的功能

1. ​		配置文件：在今后的开发过程当中我们会频繁使用框架（框架：半成品软件），使用框架时，需要写配置文件配置相关的参数，让框架满足我们的开发需求。而我们写的配置文件中就有一种文件类型是XML。



2. ​		传输数据：在网络中传输数据时并不能传输java对象，所以我们需要将JAVA对象转成字符串传输，其中一种方式就是将对象转为XML类型的字符串。

**XML和HTML的区别**

1. XML语法严格，HTML语法松

   

2. XML标签自定义，HTML标签预定义











#### 1.3、XML基本语法

- 文件后缀名是.xml
- 第一行必须是文档声明
- 有且仅有一个根标签
- 标签必须正确关闭
- 标签名区分大小写
- 属性值必须用引号（单双都可）引起来









#### 1.4、XML组成部分

* **文档声明**

文档声明必须放在第一行，格式为：

```
<?xml 属性列表 ?>
```

> 属性列表：
>
> - version：版本号（必须）
> - encoding：编码方式

* **标签**

XML中标签名是自定义的，标签名有以下要求：

- 包含数字、字母、其他字符
- 不能以数字和标点符号开头，可以以_(下划线)开头
- 不能包含空格

* **指令(了解)**

指令是结合css使用的，但现在XML一般不结合CSS，语法为：

```
<?xml-stylesheet type="text/css" href="a.css" ?>
```

* **属性**

属性值必须用引号（单双都可）引起来

* **文本**

如果想原样展示文本，需要设置CDATA区，格式为：

```
<![CDATA[文本]]>
```











### 2、约束

#### 2.1、约束概念

虽然XML标签是自定义的。但是作为配置文件时，也需要遵循一定的规则。就比如在主板上硬盘口只能插硬盘，不能插入其他硬件。约束就是定义XML书写规则的文件，约束我们按照框架的要求编写配置文件。

我们作为框架的使用者，不需要会写约束文件，只要能够在xml中引入约束文档，简单的读懂约束文档即可。XML有两种约束文件类型：DTD和Schema。



#### 2.2、DTD约束



DTD是一种较简单的约束技术，引入方式如下：

- 本地引入：

  ```
  1
  <!DOCTYPE 根标签名 SYSTEM "dtd文件的位置">
  ```

- 网络引入：

  ```
  1
  <!DOCTYPE 根标签名 PUBLIC "dtd文件的位置" "dtd文件路径">
  ```





#### 2.3、Schema约束

Schema比DTD对XML的约束更加详细，引入方式如下：

1. 写xml文档的根标签

2. 引入xsi前缀：确定Schema文件的版本。

   ```
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   ```

3. 引入Schema文件

   ```
   xsi:schemaLocation="Schema文件定义的命名空间 Schema文件的具体路径"
   ```

4. 为Schema约束的标签声明前缀

   ```
   xmlns:前缀="Schema文件定义的命名空间"
   ```







### 3、Jsoup解析器

#### 3.1、XML解析思想

 XML解析即读写XML文档中的数据。框架的开发者通过XML解析读取框架使用者配置的参数信息，开发者也可以通过XML解析读取网络传来的数据。XML有如下解析思想：

* **DOM**

将标记语言文档一次性加载进内存，在内存中形成一颗dom树

- 优点：操作方便，可以对文档进行CRUD的所有操作
- 缺点：占内存

![image-20220528183641943](https://www.itbaizhan.com/wiki/imgs/image-20220528183641943.png)

* **SAX**

逐行读取，基于事件驱动的。

- 优点：不占内存，一般用于手机APP开发中读取XML
- 缺点：只能读取，不能增删改

![image-20220528183825547](https://www.itbaizhan.com/wiki/imgs/image-20220528183825547.png)







#### 3.2、XML常见解析器

- JAXP：SUN公司提供的解析器，支持DOM和SAX两种思想
- DOM4J：一款非常优秀的解析器
- Jsoup：Jsoup是一款Java的HTML解析器，支持DOM思想。可直接解析某个URL地址、HTML文本内容。它提供了一套非常省力的API，可通过CSS以及类似于jQuery的操作方法来取出和操作数据
- PULL：Android操作系统内置的解析器，支持SAX思想







####  3.3、Jsoup快速入门

1. 导入jar包
2. 加载XML文档进内存，获取DOM树对象Document
3. 获取对应的标签Element对象
4. 获取数据













#### 3.4、Jsoup

Jsoup：可以解析xml或html，形成dom树对象。

常用方法：

- static Document parse(File in, String charsetName)：解析本地文件
- static Document parse(String html)：解析html或xml字符串
- static Document parse(URL url, int timeoutMillis)：解析网页源文件













#### 3.5、Document

Document：xml的dom树对象

常用方法：

- Element getElementById(String id)：根据id获取元素
- Elements getElementsByTag(String tagName)：根据标签名获取元素
- Elements getElementsByAttribute(String key)：根据属性获取元素
- Elements getElementsByAttributeValue(String key,String value)：根据属性名=属性值获取元素。
- Elements select(Sting cssQuery)：根据选择器选取元素。















#### 3.6、Element

Element: 元素对象

常用方法：

- String text()：获取元素包含的纯文本。
- String html()：获取元素包含的带标签的文本。
- String attr(String attributeKey)：获取元素的属性值。















#### 3.7XPath解析

XPath即为XML路径语言，它是一种用来确定标记语言文档中某部分位置的语言。

使用方法：

1. 导入`Xpath`的jar包
2. 获取`Document`对象
3. 将`Document`对象转为`JXDocument`对象
4. `JXDocument`调用`selN(String xpath)`，获取`List<JXNode>`对象。
5. 遍历`List<JXNode>`，调用`JXNode`的`getElement()`，转为`Element`对象。
6. 处理`Element`对象。





### 4、网络爬虫案例

**网络爬虫（web crawler）：自动抓取互联网信息的程序。**

Jsoup可以通过URL获取网页的HTML源文件，源文件中包含着网站数据，我们可以解析HTML源文件的数据来获取我们需要的信息。

爬虫步骤：

1. 引入jar包。
2. 使用Jsoup获取网页HTML源文件，转为Document对象
3. 通过Document对象，获取需要的Element对象
4. 获取Element对象的数据。
5. 设置循环自动爬取





















































## 五、Ajax技术详解



### 1、Ajax基础

#### 1.1、Ajax简介

Ajax 即“Asynchronous Javascript And XML”（异步 JavaScript 和 XML），是指一种创建 交互式、快速动态应用的网页开发技术，无需重新加载整个网页的情况下，能够更新页面局 部数据的技术。通过在后台与服务器进行少量数据交换，Ajax 可以使页面实现异步更新。这意味着可以在不重新加载整个页面的情况下，对页面的某部分进行更新。

![image-20220617102741319](https://www.itbaizhan.com/wiki/imgs/image-20220617102741319.png)

![image-20220617103125929](https://www.itbaizhan.com/wiki/imgs/image-20220617103125929.png)













### 2、Jcakson详解

















































# 项目管理与SSM框架

## 一、Maven

### 1.1 Maven 介绍

**Maven简介**

Maven是一个项目管理工具。它可以帮助程序员构建工程，管理jar包，编译代码，完成测试，项目打包等等。

> - Maven工具是基于POM（Project Object Model，项目对象模型）实现的。在Maven的管理下每个项目都相当于是一个对象。
> - Maven标准化了项目的构建。即对项目结构，构建命令等进行了标准化定义。
> - Maven提供了一个免费的中央仓库，在其中几乎可以找到任何的流行开源类库。
> - Maven是跨平台的，在Windows、Linux、Mac上，都可以使用同样的命令。



**Maven作用**

**一键构建**

我们的项目往往都要经历编译、测试、运行、打包、安装 ，部署等一系列过程，这些过程称之为构建。通过Maven工具，可以使用简单的命令轻松完成构建工作。

**依赖管理**

传统的Web项目中，我们必须将工程所依赖的jar包复制到工程中，导致了工程的变得很大。如果一个公司具有相同架构的项目有十个，那么就需要将这一份jar包复制到十个不同的工程中，非常浪费资源。



maven工程中不直接将jar包导入到工程中，而是有一个专门存放jar包的仓库，仓库中的每个jar包都有自己的坐标。maven工程中只要配置jar包坐标即可，运行项目需要使用jar包时，根据坐标即可从maven仓库中拿到jar包即可运行。





### 1.2 Maven工程的类型和结构

**Maven工程类型**

- POM工程

  POM工程是逻辑工程，Maven并不会对该类型工程做打包处理，这些工程往往不包含具体的业务，而是用来整合其他工程的。

- JAR工程

  普通Java工程，在打包时会将项目打成jar包。

- WAR工程

  JAVA Web工程，在打包时会将项目打成war包。





**Maven工程结构**

- src：源代码
- target：编译生成的文件
- pom.xml：Maven工程配置文件，如坐标信息等。

- src/main/java：存放项目的java 文件
- src/main/resources：存放项目资源文件，如配置文件
- src/test/java：存放项目的测试文件
- src/test/resources：存放测试时的资源文件













### 1.3 一键构建

#### 1.31、项目的生命周期

使用maven完成项目的构建的过程中，包括：验证、编译、测试、打包、部署等过程，maven将这些过程规范为项目构建的生命周期。

![image-20211111180257011](https://www.itbaizhan.com/wiki/imgs/image-20211111180257011.png?v=1.0.0)

| 生命周期      | 所做工作                                                   |
| ------------- | ---------------------------------------------------------- |
| 验证 validate | 验证项目是否正确                                           |
| 编译 compile  | 源代码编译                                                 |
| 测试 Test     | 使用适当的单元测试框架（例如junit）运行测试。              |
| 打包 package  | 创建JAR/WAR包                                              |
| 检查 verify   | 对集成测试的结果进行检查，以保证质量达标。                 |
| 安装 install  | 安装打包的项目到本地仓库，以供其他项目使用。               |
| 部署 deploy   | 拷贝最终的工程包到远程仓库中，以共享给其他开发人员和工程。 |

> maven有三套相互独立的生命周期。分为是构建生命周期，clean生命周期（清理构建后的文件）、site生命周期（生成项目报告）。作为开发人员我们一般重点学习构建生命周期即可。







#### 1.32、Maven常用命令

在Maven构建项目的每一步都可以使用一句简单的命令完成，接下来我们学习这些命令：

| 命令            | 作用                                                |
| --------------- | --------------------------------------------------- |
| mvn clean       | 清除编译的class文件，即删除target目录。             |
| mvn validate    | 验证项目是否正确                                    |
| mvn compile     | 编译maven项目                                       |
| mvn test        | 编译maven项目及运行测试文件                         |
| mvn package     | 编译maven项目及运行测试文件，并打包                 |
| mvn install     | 编译maven项目及运行测试文件并打包，并发布到本地仓库 |
| mvn deploy      | 部署项目到远程仓库                                  |
| mvn tomcat7:run | 使用tomcat运行项目                                  |

> Maven依赖插件来执行命令，比如clean、validate等命令是maven自带的，tomcat7命令是引入的第三方插件。











### 1.4 依赖管理



#### 1.4.1、Maven仓库类型

* **本地仓库**

本地仓库指用户计算机中的文件夹。用来存储从远程仓库或中央仓库下载的jar包，只有下载到本地仓库的jar包才能使用，项目使用jar包时优先从本地仓库查找。



* **远程仓库**

远程仓库一般指私服，它是架设在局域网的仓库服务，可以从中央仓库下载资源，供局域网使用，从而减少每个程序员都从中央仓库下载浪费的带宽。

如果项目需要的jar包本地仓库没有，则会去远程仓库下载，下载到本地仓库即可使用。

> 远程仓库不是必须配置的，如果本地仓库没有jar包，也没有配置远程仓库，则会直接从中央仓库下载。





* **中央仓库**

中央仓库是互联网上的服务器，是Maven提供的最大的仓库，里面拥有最全的jar包资源。

如果项目需要的jar包，本地仓库和远程仓库都没有，则会去中央仓库下载，下载到本地仓库使用。

Maven中央仓库访问页面：https://mvnrepository.com/

> 中央仓库访问速度较慢，我们一般都会配置镜像代理中央仓库的下载请求，如阿里镜像、华为镜像等。







#### 1.4.2、Maven配置文件

本地仓库的默认位置是`${user.dir}/.m2/repository`，`${user.dir}`表示 windows用户目录，我们可以通过修改`${MAVEN_HOME}\conf\settings.xml`，修改本地仓库的位置。

**配置本地仓库**

在`<settings>`中添加如下标签：

```
<!-- 本地仓库路径 -->
<localRepository>F://repository</localRepository>
```















### 1.6 Maven工程开发

pom文件最上方是项目基本信息：

![image-20211122151003114](https://www.itbaizhan.com/wiki/imgs/image-20211122151003114.png?v=1.0.0)

> - groupId
>
>   groupId一般定义项目组名，命名规则使用反向域名。例如com.itbaizhan
>
> - artifactId
>
>   artifactId一般定义项目名，命名使用小写字母。项目发布后，它的坐标是groupId+artifactId。
>
> - version
>
>   version定义版本号。版本号一般有三段，第一段：革命性的产品升级。第二段：新功能版本。第三段：修正一些bug。
>
> - packaging
>
>   packaging定义打包方式。

`<properties>`中定义一些配置信息：

![image-20211112154945589](https://www.itbaizhan.com/wiki/imgs/image-20211112154945589.png?v=1.0.0)

`<dependencies>`中定义依赖的jar包坐标：

由于项目是web项目，需要写Servlet和JSP，所以需要引入Servlet和JSP的依赖。查找依赖坐标的网站：https://mvnrepository.com/

![image-20211115162817221](https://www.itbaizhan.com/wiki/imgs/image-20211115162817221.png?v=1.0.0)

```
<dependencies>
  <dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.11</version>
    <scope>test</scope>
  </dependency>
  <!-- jsp -->
  <dependency>
    <groupId>javax.servlet.jsp</groupId>
    <artifactId>jsp-api</artifactId>
    <version>2.2</version>
  </dependency>
  <!-- servlet -->
  <dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.0.1</version>
  </dependency>
  </dependencies>
```

> 为什么之前的web项目中没有引入jsp和servlet的jar包？
>
> 因为之前项目中使用的是tomcat中的jsp和servlet中的jar包，在项目中没有引入。

`<plugins>`中定义第三方插件：

web项目依赖tomcat运行，所以添加tomcat7插件

```
<plugins>
  <!-- tomcat插件 -->
  <plugin>
    <groupId>org.apache.tomcat.maven</groupId>
    <artifactId>tomcat7-maven-plugin</artifactId>
    <version>2.1</version>
    <configuration>
      <port>8080</port>
      <path>/</path>
    <uriEncoding>UTF-8</uriEncoding>
     <server>tomcat7</server>
    </configuration>
  </plugin>
</plugins>
```































### 1.6 Maven工程测试



















### 1.7 依赖冲突调解



















### 1.8 Maven聚合开发



















## 二、Mybatis

























































## 三、Spring



























## 四、SpringMVC













































































































































































# web基础

## Web核心

* Web：全球广域网，也成为万维网(www),能够通过浏览器访问的网站
* JavaWeb：是用Java技术来解决相关web互联网领域的技术栈

### JavaWeb 技术栈

1. B/S架构：Browser/Server，浏览器/服务器架构模式，它的特点是，客户端只需要浏览器，应用程序的逻辑和数据都存储在服务器端。浏览器只需要请求服务器，获取Web资源，服务器把Web资源发送给浏览器即可
   * 好处：易于维护升级：服务器端升级后，客户端无需任何部署就可以使用到新的版本
2. 静态资源：HTML、CSS、JavaScript、图片等。负责页面展示
3. 动态组员：Servlet、JSP等。负责逻辑处理
4. 数据库：负责存储数据
5. HTTP协议：定义通信规则
6. Web服务器：负责解析HTTP协议，解析请求数据、并发送响应数据

### HTTP

* 概念：**H**yper **T**ext **T**ransfer **P**rotocol,超文本传输协议，规定了浏览器和服务器之间传输数据的规则
* HTTP协议特点
  1. 基于TCP协议：面向连接，**安全** 
  2. 基于请求-响应模型的：一次请求对应一次响应
  3. HTTP协议是无状态的协议：对于事物处理没有记忆能力。每次的请求-响应都是独立的
     * 缺点：多次请求间不能共享数据。Java中使用会话的技术(Cookie、Session)来解决这个问题
     * 优点：速度快

#### HTTP-请求数据格式

* 请求数据格式分为3部分：

  1. **请求行**：请求数据的第一行。其中GET表示请求方式，/表示请求资源路径，HTTP/1.1表示协议版本
  2. **请求体**：第二行开始，格式为key：value格式
  3. **请求头**：POST请求的最后一部分，存放请求参数

  ~~~
  GET / HTTP/1.1
  Host:www.itcast.cn
  Connection:keep-alive
  Cache-Control:max-age=0 Upgrade-Insecure-Request:1
  User-Agent:Mozilla/5.0 Chrome/91.0.4472.106
  
  ...
  ~~~

* 常见的HTTP请求头

  1. Host：表示请求主机名
  2. User-Agent：浏览器版本
  3. Accept：表示浏览器能接受的资源类型
  4. Accept-Language：表示浏览器偏好的语言，服务器可以据此返回不同语言的网页
  5. Accept-Encoding：表示浏览器可以支持压缩类型

**GET请求和POST请求区别：**

1. GET请求请求参数在请求行中，没有请求体。POST请求请求参数在请求体中
2. GET请求请求参数大小有限制，POST没有







#### HTTP-响应数据格式

* 响应数据分为三部分

  1. **响应行**：响应数据的第一行。其中HTTP/1.1标识协议版本，200表示响应状态码，OK表示状态码描述
  2. **响应头**：第二行开始，格式为key：value形式
  3. **响应体**：最后一部分。存放响应数据

  ~~~
  HTTP/1.1 200 OK
  Server: Tengine
  Content-Type:text/html
  Transfer-Encoding:chunked...
  
  <html>
  <head>
  	<title></title>
  </head>
  <body></body>
  </html>
  ~~~

  

* **常见的HTTP响应头**
  * Content-Type：表示该响应内容的类型
  * Content-Length：表示该响应内容的长度
  * Content-Encoding：表示响应压缩算法
  * Cache-Control：指示客户端应如何缓存
* 响应状态码

| 状态码分类 | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| 1xx        | **响应中**——临时状态码，表示请求已经接受，告诉客户端应该继续请求或者如果它已经完成则忽略它 |
| 2xx        | **成功**——表示请求已经被成功接收，处理已完成                 |
| 3xx        | **重定向**——重定向到其它地方：它让客户端再发起一个请求以完成整个处理。 |
| 4xx        | **客户端错误**——处理发生错误，责任在客户端，如：客户端的请求一个不存在的资源，客户端未被授权，禁止访问等 |
| 5xx        | **服务器端错误**——处理发生错误，责任在服务端，如：服务端抛出异常，路由出错，HTTP版本不支持等 |

状态码大全：https://cloud.tencent.com/developer/chapter/13553 






