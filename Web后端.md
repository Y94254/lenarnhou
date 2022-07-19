Java

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



































































### 2、多线程的创建





















































### 3、多线程的使用























### 4、线程的优先级以及守护线程











































































###  5、线程同步









































































### 6、线程并发协作















































## 反射



### 1、反射介绍

























### 2、获取Class对象































### 3、获取类的构造方法

























### 4、获取方法





































## Lambda表达式

### 1、Lambda表达式介绍







### 2、Lambda表达式语法





































### 3、Lambda表达式入门案例



















































### 4、Lambda表达式的使用

## 网络编程(百战)

### 1、网络编程基本概念





















### 2、网络编程常用类

















### 3、TCP通信的实现





















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

~~~
    语法：jdbc:mysql://ip地址（域名）:端口号/数据库名称?参数键值对对1&参数键值对2....
    示例：jdbc:mysql://127.0.0.1:3306/itheima
    细节：
    		如果连接的是本机的mysql服务器，并且mysql默认端口是3306，则url可以简写为:jdbc:mysql:///数据库名称？参数键值对
    		配置useSSL=false参数，禁用安全连接方式，解决警告提示
    		
    ~~~

 2. user:用户名

 3. password:密码
~~~




























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

     ~~~
     //循环判断游标是否是最后一行末尾
     while(rs.next()){
     	//获取数据
     	rs.getXxx(参数);
     }
     ~~~

     















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

#### MyBatis 快速入门













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











### Web服务器



# web主流框架





# web框架进阶



# 其他

