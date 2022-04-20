# Java基础

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









#### 5、Map























#### 6、Collections





























## I/O



## 并发 

 



## 反射





## 网络编程





# 数据库

## SQL语法

### DDL

### DML

### DQL

## JDBC API

## 数据库四大特性

## 数据库连接池

 





# web基础

## Http/TCP协议



# web主流框架





# web框架进阶



# 其他

