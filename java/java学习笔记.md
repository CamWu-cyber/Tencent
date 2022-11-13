# 继承（父类-子类）

## 继承概述

继承是面向对象三大特征之一。可以使得子类具有父类的属性和方法，还可以在子类中重新定义，追加属性和方法。

继承的格式
* 格式：public class 子类名 **extends** 父类名
* 范例：public class Zi **extends** Fu {}
* Fu: 是父类，也被称为基类、超类
* Zi: 是子类，也被称为派生类

继承中子类的特点
* 子类可以有父类的内容
* 子类还可以有自己特有的内容

Fu.java

        package com.itheima_01;

        public class Fu {
          public void show() {
            System.out.println("show方法被调用");
          }
        }
        
Zi.java

        package com.itheima_01;

        public class Zi extends Fu{
                public void method() {
                        System.out.println("method方法被调用");
                }
        }
        
 Demo.java
 
        package com.itheima_01;
        /*
         测试类
         */
        public class Demo {
                public static void main(String[] args) {
                        // 创建对象，调用方法
                        Fu f = new Fu();
                        f.show();

                        Zi z = new Zi();
                        z.method();
                        z.show();
                }
        }
        
        运行结果：
        show方法被调用
        method方法被调用
        show方法被调用

## 继承的好处和弊端
继承的好处
* 提高了代码的复用性（多个类相同的成员可以放到同一个类中）
* 提高了代码的维护性（如果方法的代码需要修改，修改一处即可）

继承的弊端
* 继承让类与类之间产生了关系，类的耦合性增强了，当父类发生变化时子类实现也不得不跟着变化，削弱了子类的独立性

什么时候使用继承？
* 假设法：我有两个类A和B，如果他们满足A是B的一种，或者B是A的一种，就说明他们存在继承关系，这个时候就可以考虑使用继承来体现，否则就不能滥用继承
* 举例：苹果和水果，猫和动物

## 继承中变量的访问特点
在子类方法中访问一个变量
* 子类局部范围找
* 子类成员范围找
* 父类成员范围找
* 如果都没有就报错（不考虑父类的父类）

## super
super关键字的用法和this关键字的用法相似
* this: 代表本类对象的引用。this关键字指向调用该方法的对象，一搬我们是在当前类中使用this关键字，所以我们常说this代表本类对象的引用。
* super: 代表父类存储空间的标识（可以理解为父类对象引用）

Fu.java

        package com.itheima_02;

        public class Fu {
                public int age = 40;
        }
        
 Zi.java
 
        package com.itheima_02;

        public class Zi extends Fu{
                public int age = 20;

                public void show() {
                        int age = 30;
                        System.out.println(age);
                        // 我要访问本类的成员变量age，怎么办呢？
                        System.out.println(this.age);
                        // 我要访问父类的成员变量age，怎么办呢？
                        System.out.println(super.age);
                }
        }
        
Demo.java

        package com.itheima_02;

        public class Demo {
                public static void main(String[] args) {
                        //创建对象，调用方法
                        Zi z = new Zi();
                        z.show();
                }
        }
        
        运行结果
        30
        20
        40

## 继承中构造方法的访问特点
子类中所有的构造方法默认都会访问父类中无参的构造方法
为什么呢?
* 因为子类会继承父类中的数据，可能还会使用父类的数据。所以，子类初始化之前，一定要先完成父类数据的初始化
* 每一个子类构造方法的第一条语句默认都是：**super()**，即父类的无参构造方法

如果父类中没有无参构造方法，只有带参构造方法，该怎么办呢？
* 通过使用super关键字去显示的调用父类的带参构造方法
* 在父类中自己提供一个无参构造方法
- 推荐：自己给出无参构造方法

Zi.java

        package com.itheima_03;

        public class Zi extends Fu{
                public Zi() {
        //		super();
        //		super(20);
                        System.out.println("Zi中无参构造方法被调用");
                }

                public Zi(int age) {
        //		super();
        //		super(20);
                        System.out.println("Zi中有参构造方法被调用");
                }
        }
        
Fu.java

        package com.itheima_03;

        public class Fu {
                /*
                public Fu() {
                        System.out.println("Fu中无参构造方法被调用");
                }
                */
                public Fu() {}

                public Fu(int age) {
                        System.out.println("Fu中有参构造方法被调用");
                }
        }
        
Demo.java

        package com.itheima_03;

        public class Demo {
                public static void main(String[] args) {
                        //创建对象
                        Zi z = new Zi();

                        Zi z2 = new Zi(20);
                }
        }
        
        运行结果
        Zi中无参构造方法被调用
        Zi中有参构造方法被调用

## 继承中成员方法的访问特点
通过子类对象访问一个方法
* 子类成员范围找
* 父类成员范围找
* 如果都没有就报错（不考虑父类的父类）

## 方法重写
方法重写概述
* 子类中出现了和父类中一模一样的方法声明

方法重写的应用
* 当子类需要父类的功能，而功能主体子类有自己特有内容时，可以重写父类中的方法，这样，即沿袭了父类的功能，又定义了子类特有的内容
* 练习：手机类和新手机类

@Override
* 是一个注解（注解后面会学习到）
* 可以帮助我们检查重写方法的方法声明的正确性

注意事项
* 私有方法不能被重写（父类私有成员子类是不能继承的）
* 子类方法访问权限不能更低（public > 默认 > 私有）

Phone.java

        package com;
        /*
         手机类
         * */

        public class Phone {
                public void call(String name) {
                        System.out.println("给" + name + "打电话");
                }

        }
        
NewPhone.java

        package com;

        public class NewPhone extends Phone{

                @Override   // 注解作用：检测方法声明的正确性
                public void call(String name) {
                        System.out.println("开启视频功能");
        //		System.out.println("给" + name + "打电话");
                        super.call(name);
                }


        }

Demo.java

        package com;
        /*
         * 测试类
         * */
        public class Demo {
                public static void main(String[] args) {
                        //创建对象，调用方法
                        Phone p = new Phone();
                        p.call("林青霞");
                        System.out.println("----------------");

                        NewPhone np = new NewPhone();
                        np.call("林青霞");
                }
        }

        运行结果：
        给林青霞打电话
        ----------------
        开启视频功能
        给林青霞打电话

## Java中继承的注意事项
* Java中类只支持单继承，不支持多继承
* Java中类支持多层继承（比如：granddad类, father类, son类）

![1](https://github.com/CamWu-cyber/Tencent/blob/main/java/%E5%9B%BE%E7%89%87/1.png)

# 包
## 包的概述和使用
包其实就是文件夹
作用：对类进行分类管理

包的定义格式
* 格式：package 包名; （多级包用.分开）
* 范例：package com.itheima;

带包的Java类编译和执行
* 手动建包：

        按照以前的格式编译java文件         javac HelloWorld.java
        手动创建包                        在E盘建立文件夹com，然后在com下建立文件夹itheima
        把class文件放到包的最里面          把HelloWorld.class文件放到com下的itheima这个文件夹下
        带包执行                          java com.itheima.HelloWorld
        
 
* 自动建包：

        javac -d . HelloWorld.java               java com.itheima.HelloWorld

## 导包的概述和使用
使用不同包下的类时，使用的时候要写类的全路径，写起来太麻烦了

为了简化带包的操作，Java就提供了导包的功能

导包的格式：
* 格式：import包名;
* 范例：import cn.itcast.Teacher

# 修饰符
## 权限修饰符（private, 默认, protected, public）
![2](https://github.com/CamWu-cyber/Tencent/blob/main/java/%E5%9B%BE%E7%89%87/2.png)

## 状态修饰符
* final(最终态)
* static(静态)

### final
final 关键字是最终的意思，可以修饰成员方法，成员变量，类

final 修饰的特点
* 修饰方法：表明该方法是最终方法，**不能被重写**
* 修饰变量：表明该变量是常量，**不能再次被赋值**
* 修饰类：表明该类是最终类，**不能被继承**

### final 修饰局部变量
* 变量是基本类型：final修饰指的是基本类型的**数据值**不能发生改变
* 变量是引用类型（其他类的成员变量）：final修饰指的是引用类型的**地址值**不能发生改变，但是地址里面的内容是可以发生改变的

FinalDemo.java

        package com.itheima_05;

        public class FinalDemo {
                public static void main(String[] args) {
                        // final修饰基本类型变量
                        final int age = 20;
        //		age = 100;  不允许修改age的值
                        System.out.println(age);

                        // final修饰引用类型变量
                        final Student s = new Student();
                        s.age = 100;    // 地址的内容可以变
                        System.out.println(s.age);

        //		s = new Student();   // 地址不能变
                }
        }
        
Student.java

        package com.itheima_05;

        public class Student {
                public int age = 20;
        }

### static
static 关键字是静态的意思，可以修饰成员方法，成员变量

static 修饰的特点
* 被类的所有对象共享，这也是我们判断是否使用静态关键字的条件
* 可以通过类名调用，当然也可以通过对象名调用，**推荐使用类名调用**

Student.java

        package com.itheima_06;

        public class Student {
                public String name;
                public int age;
                public static String university;

                public void show() {
                        System.out.println(name + "," + age + "," + university);
                }
        }
        
StaticDemo.java

        package com.itheima_06;

        /*
         * 测试类
         * */

        public class StaticDemo {
                public static void main(String[] args) {
                        //推荐使用类名调用static修饰的变量
                        Student.university = "传智大学";

                        Student s1 = new Student();
                        s1.name = "林青霞";
                        s1.age = 30;
        //		s1.university = "传智大学";
                        s1.show();

                        Student s2 = new Student();
                        s2.name = "风清扬";
                        s2.age = 32;
        //		s2.university = "传智大学";
                        s2.show();
                }
        }
        
        运行结果：
        林青霞,30,传智大学
        风清扬,32,传智大学

### static访问特点
非静态的成员方法
* 能访问 静态的 and 非静态的 成员变量
* 能访问 静态的 and 非静态的 成员方法

静态成员方法
* 能访问静态的成员变量
* 能访问静态的成员方法

**总结成一句话就是：静态成员方法只能访问静态成员**

# 多态
同一个对象，在不同时刻表现出来的不同形态

举例：猫

我们可以说猫是猫：猫 cat = new 猫（）

我们也可以说猫是动物：动物 animal = new 猫（）

这里猫在不同的时刻表现出来了不同形态，这就是多态

多态的前提和体现（有下面三类现象就说明出现了**多态**）
* 有继承/实现关系
* 有方法重写
* 有父类引用指向子类对象（第二个例子里面 animal就是父类引用，猫就是子类对象）

Animal.java

        package com.itheima_07;

        public class Animal {
                public void eat() {
                        System.out.println("动物吃东西");
                }
        }
        
Cat.java

        package com.itheima_07;

        public class Cat extends Animal {
                @Override
                public void eat() {
                        System.out.println("猫吃鱼");
                }
        }
        
AnimalDemo.java

        package com.itheima_07;
        /*
         * 多态
         * */
        public class AnimalDemo {
                public static void main(String[] args) {
                        // 有父类引用指向子类对象
                        Animal a = new Cat();
                        a.eat();
                }
        }
        运行结果：
        猫吃鱼

## 多态中成员访问特点
* 成员变量：编译看左边，执行看左边
* 成员方法：编译看左边，执行看右边

为什么成员变量和成员方法的访问不一样呢？

&emsp;&emsp;&emsp;&emsp;因为成员方法有重写，而成员变量没有

Animal.java

        package com.itheima_08;

        public class Animal {
                public int age = 40;
                public void eat() {
                        System.out.println("动物吃东西");
                }
        }
        
Cat.java

        package com.itheima_08;

        public class Cat extends Animal {
                public int age = 20;
                public int weight = 10;

                @Override
                public void eat() {
                        System.out.println("猫吃鱼");
                }

                public void playGame() {
                        System.out.println("猫捉迷藏");
                }
        }
        
AnimalDemo.java

        package com.itheima_08;
        /*
         * 测试类
         * */

        public class AnimalDemo {
                public static void main(String[] args) {
                        // 有父类引用指向子类对象
                Animal a = new Cat();

                System.out.println(a.age);
        //        System.out.println(a.weight);  编译报错，因为Animal类没有weight变量

                a.eat();
        //        a.playGame();  编译报错，因为Animal类没有playGame()方法
                }
        }
        
        运行结果：
        40
        猫吃鱼

## 多态的好处和弊端
* 多态的好处：提高了程序的扩展性

&emsp;&emsp;&emsp;&emsp;具体体现：定义方法的时候，使用父类型作为参数，将来在使用的时候，使用具体的子类型参与操作

* 多态的弊端：不能使用子类的特有功能 

## 多态中的转型
为了解决上述的弊端，可以采用“向下转型”的方法。

注意：子类对象之间不允许强制转换（向下转型）

* 向上转型
        
&emsp;&emsp;&emsp;&emsp;从子到父
        
&emsp;&emsp;&emsp;&emsp;父类引用指向子类对象
        
* 向下转型

&emsp;&emsp;&emsp;&emsp;从父到子
        
&emsp;&emsp;&emsp;&emsp;父类引用转为子类对象
        
Animal.java

        package com.itheima_09;

        public class Animal {
                public void eat() {
                        System.out.println("动物吃东西");
                }
        }
        
Cat.java

        package com.itheima_09;

        public class Cat extends Animal{
                @Override
                public void eat() {
                        System.out.println("猫吃鱼");
                }

                public void playGame() {
                        System.out.println("猫捉迷藏");
                }
        }
        
AnimalDemo.java

        package com.itheima_09;

        /*
         * 向上转型
                从子到父
                父类引用指向子类对象

        * 向下转型
                从父到子
                父类引用转为子类对象
         * */

        public class AnimalDemo {
                public static void main(String[] args) {
                        // 多态
                        Animal a = new Cat(); // 向上转型
                        a.eat();
        //		a.playGame();  编译报错

                        // 向下转型，就能访问到playGame()这个子类的特有方法啦
                        Cat c = (Cat)a;
                        c.eat();
                        c.playGame();
                }
        }
        运行结果：
        猫吃鱼
        猫吃鱼
        猫捉迷藏

# 抽象类
## 抽象类的概述
在Java中，一个**没有方法体**的方法应该定义为**抽象方法**，而类中如果有**抽象方法**，该类必须定义为**抽象类**

## 抽象类的特点
* 抽象类和抽象方法必须使用abstract关键字修饰

&emsp;&emsp;&emsp;&emsp;public abstract class 类名 {}
        
&emsp;&emsp;&emsp;&emsp;public abstract void eat();

* 抽象类中不一定有抽象方法，有抽象方法的类一定是抽象类
* 抽象类不能实例化，抽象类如何实例化呢？参展多态的方式，通过子类对象实例化，这叫抽象类多态
* 抽象类的子类

&emsp;&emsp;&emsp;&emsp;要么重写抽象类中的所有抽象方法
        
&emsp;&emsp;&emsp;&emsp;要么本身就是抽象类
        
Animal.java

        package com.itheima_10;
        /*
         * 抽象类
         * */
        public abstract class Animal {
                // 抽象方法
                public abstract void eat();

                public void sleep() {
                        System.out.println("睡觉");
                }

        }

Cat.java

        package com.itheima_10;

        public class Cat extends Animal{
                @Override
                public void eat() {
                        System.out.println("猫吃鱼");
                }
        }
        
AnimalDemo.java

        package com.itheima_10;
        /*
         * 测试类
         * */
        public class AnimalDemo {
                public static void main(String[] args) {
        //		Animal a = new Animal();  抽象类不能实例化
                        Animal a = new Cat();
                        a.eat();
                        a.sleep();
                }
        }
        运行结果：
        猫吃鱼
        睡觉
        
Dog.java

        package com.itheima_10;

        public abstract class Dog extends Animal{

        }

## 抽象类的成员特点
* 成员变量

&emsp;&emsp;&emsp;&emsp;可以是变量

&emsp;&emsp;&emsp;&emsp;也可以是常量
        
* 构造方法

&emsp;&emsp;&emsp;&emsp;有构造方法，但是不能实例化

&emsp;&emsp;&emsp;&emsp;那么，构造方法的作用是什么呢？用于子类访问父类数据的初始化
        
* 成员方法

&emsp;&emsp;&emsp;&emsp;可以有抽象方法：限定子类必须完成某些动作
        
&emsp;&emsp;&emsp;&emsp;也可以有非抽象方法：提高代码复用性（由继承来保证）

Animal.java

        package com.itheima_11;
        /*
         * 抽象类
         */
        public abstract class Animal {
                private int age = 20;
                private final String city = "北京";

                public Animal() {}  // 构造方法(无参)

                public Animal(int age) {  // 构造方法（有参）
                        this.age = age;
                }

                public void show() {
                        age = 40;
                        System.out.println(age);
        //		city = "上海";   常量无法被重新赋值
                        System.out.println(city);
                }

                public abstract void eat();   // 可以有抽象方法：限定子类必须完成某些动作
        }

Cat.java

        package com.itheima_11;

        public class Cat extends Animal{
                @Override
                public void eat() {
                        System.out.println("猫吃鱼");
                }
        }

AnimalDemo.java

        package com.itheima_11;
        /*
         * 测试类
         */
        public class AnimalDemo {
                public static void main(String[] args) {
                        Animal a = new Cat();
                        a.eat();
                        a.show();
                }
        }
        
        运行结果：
        猫吃鱼
        40
        北京

# 接口
## 接口概述
接口就是一种**公共的规范标准**，只要符合规范标准，大家都可以通用

Java中的接口更多的体现在对**行为的抽象**

## 接口的特点
* 接口用关键字interface修饰

&emsp;&emsp;&emsp;&emsp;public interface 接口名 {}

* 类实现接口用implements表示

&emsp;&emsp;&emsp;&emsp;public class 类名 implements 接口名 {}

* 接口不能实例化

&emsp;&emsp;&emsp;&emsp;接口如何实例化呢？参照多态的方式，通过实现类对象实例化，这叫接口多态。

&emsp;&emsp;&emsp;&emsp;多态的形式：具体类多态，**抽象类多态，接口多态.** (后两种最常用，所以加粗了)

&emsp;&emsp;&emsp;&emsp;多态的前提：有继承或者实现关系；有方法重写；有父（类/接口）引用指向（子/实现）类对象

* 接口的实现类

&emsp;&emsp;&emsp;&emsp;要么重写接口中的所有抽象方法

&emsp;&emsp;&emsp;&emsp;要么是抽象类（看Dog.java）

Jumpping.java

        package com.itheima_12;
        /*
         * 定义了一个接口
         */
        public interface Jumpping {
                public abstract void jump();   // 定义了一个抽象方法
        }
        
Cat.java

        package com.itheima_12;

        public class Cat implements Jumpping{
                @Override
                public void jump() {
                        System.out.println("猫可以跳高了");
                }
        }
        
Dog.java

        package com.itheima_12;

        public abstract class Dog implements Jumpping{

        }
        
JumppingDemo.java

        package com.itheima_12;

        public class JumppingDemo {
                public static void main(String[] args) {
        //		Jumpping j = new Jumpping();   接口不能被实例化，可以通过多态来实例化
                        Jumpping j = new Cat();
                        j.jump();
                }
        }
        运行结果：
        猫可以跳高了

## 接口的成员特点
* 成员变量

&emsp;&emsp;&emsp;&emsp;只能是常量

&emsp;&emsp;&emsp;&emsp;默认修饰符：**public static final**

* 构造方法

&emsp;&emsp;&emsp;&emsp;接口没有构造方法，因为接口主要是对行为进行抽象的，是没有具体存在

&emsp;&emsp;&emsp;&emsp;一个类如果没有父类，默认继承自Object类

* 成员方法

&emsp;&emsp;&emsp;&emsp;只能是抽象方法

&emsp;&emsp;&emsp;&emsp;默认修饰符：public abstract

&emsp;&emsp;&emsp;&emsp;关于接口中的方法，JDK8和JDK9中有一些新特性，后面再讲解

Inter.java

        package com.itheima_13;

        public interface Inter {
                public int num = 10;
                public final int num2 = 20;
        //	public static final int num3 = 30;  前面的可以不写，直接写 int num3 = 30就行
                int num3 = 30;

        //	public Inter() {}  编译报错，因为接口没有构造方法

        //	public void show() {}  编译报错，因为接口没有非抽象方法

                public abstract void method();  // 接口都是抽象方法，不写public abstract也行
                void show();
        }
        
InterImpl.java

        package com.itheima_13;
        /*
         * 接口的 实现类
         */
        public class InterImpl extends Object implements Inter {
                public InterImpl() {
                        super();  // 这个父类指的是Object
                }

                @Override
                public void method() {
                        System.out.println("method");
                }

                @Override
                public void show() {
                        System.out.println("show");
                }
        }

InterfaceDemo.java

        package com.itheima_13;
        /*
         * 测试类
         */
        public class InterfaceDemo {
                public static void main(String[] args) {
                        Inter i = new InterImpl();
                        System.out.println(i.num);
        //		i.num2 = 40;  被final修饰，不能赋值
        //		i.num = 20;   没有被final修饰也不能赋值，是为什么呢？因为接口内没有变量，所有变量默认为常量
                        System.out.println(i.num2);
                        System.out.println(Inter.num);  // 能用类名访问，说明变量默认被static修饰的
                }
        }
        运行结果：
        10
        20
        10

## 猫和狗（接口版）

纯demo，综合运用前面的知识点

Animal.java

        package com.itheima_14;
        // 父类
        public abstract class Animal {
                private String name;
                private int age;

                public Animal() {

                }

                public Animal(String name, int age) {
                        this.name = name;
                        this.age = age;
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

                public abstract void eat();
        }

Jumpping.java

        package com.itheima_14;
        // 接口
        public interface Jumpping {
                public abstract void jump();
        }
        
Cat.java

        package com.itheima_14;
        // 子类
        public class Cat extends Animal implements Jumpping{
                public Cat() {

                }

                public Cat(String name, int age) {
                        super(name, age);
                }

                @Override
                public void eat() {
                        System.out.println("猫吃鱼");
                }

                @Override
                public void jump() {
                        System.out.println("猫可以跳高了");
                }
        }

AnimalDemo.java

        package com.itheima_14;
        // 测试类
        public class AnimalDemo {
                public static void main(String[] args) {
                        // 创建对象，调用方法
                        Jumpping j = new Cat();
                        j.jump();
                        System.out.println("-------");

                        Animal a = new Cat();
                        a.setName("加菲");
                        a.setAge(5);
                        System.out.println(a.getName()+","+a.getAge());
                        a.eat();
                        System.out.println("--------");

                        // 实际用法，直接构造Cat类的对象
                        Cat c = new Cat();
                        c.setName("波斯");
                        c.setAge(6);
                        System.out.println(c.getName()+","+c.getAge());
                        c.eat();
                        c.jump();
                }
        }
        
        运行结果：
        猫可以跳高了
        -------
        加菲,5
        猫吃鱼
        --------
        波斯,6
        猫吃鱼
        猫可以跳高了

## 类和接口的关系
* 类和类的关系

&emsp;&emsp;&emsp;&emsp;继承关系，只能单继承，但是可以多层继承

* 类和接口的关系

&emsp;&emsp;&emsp;&emsp;实现关系，可以单实现，也可以多实现，还可以在继承一个类的同时实现多个接口

        public class InterImpl extends Object implements Inter1, Inter2, Inter3 {
        
        }

* 接口和接口的关系

&emsp;&emsp;&emsp;&emsp;继承关系，可以单继承，也可以多继承（java中的多继承体现在接口中，c++多继承体现在类中）

        public interface Inter3 extends Inter1, Inter2 {
        
        }

## 抽象类和接口的区别
前两个是语法层面的区别，第三个才是重点

* 成员区别

&emsp;&emsp;&emsp;&emsp;抽象类&emsp;&emsp;&emsp;&emsp;有变量，有常量；有构造方法；有抽象方法，也有非抽象方法

&emsp;&emsp;&emsp;&emsp;接口&emsp;&emsp;&emsp;&emsp;只有常量；只有抽象方法

* 关系区别

&emsp;&emsp;&emsp;&emsp;类与类&emsp;&emsp;&emsp;&emsp;继承关系，只能单继承

&emsp;&emsp;&emsp;&emsp;类与接口&emsp;&emsp;&emsp;&emsp;实现关系，可以单实现，也可以多实现

&emsp;&emsp;&emsp;&emsp;接口与接口&emsp;&emsp;&emsp;&emsp;继承关系，可以单继承，也可以多继承

* 设计理念区别

&emsp;&emsp;&emsp;&emsp;抽象类&emsp;&emsp;&emsp;&emsp;对类抽象，包括属性、行为

&emsp;&emsp;&emsp;&emsp;接口&emsp;&emsp;&emsp;&emsp;对行为抽象，主要是行为

门和警报的例子

门：都有open()和close()两个动作，这个时候，我们可以分别使用抽象类和接口来定义这个抽象概念

        // 抽象类
        public abstract class Door {
                public abstract void open();
                public abstract void close();
        }
        
        // 接口
        public interface Door {
                void open();
                void close();
        }

这个时候需要新增一个报警的功能，可以用接口定义报警的行文，同时用抽象类保留门的Open和close的属性

        public interface Alram {
                void alarm();
        }
        public abstract class Door {
                public abstract void open();
                public abstract void close();
        }
        public class AlarmDoor extends Door implements Alarm {
                public void open() {
                        //...
                }
                public void close() {
                        //...
                }
                public void alarm() {
                        //...
                }
        }
        
在这里，我们再次强调抽象类是对事物的抽象，而接口是对行为的抽象

# 形参和返回值
## 类名作为形参和返回值
* 方法的形参是类名，其实需要的是该类的对象
* 方法的返回值是类名，其实返回的是该类的对象

Cat.java

        package com.itheima_15;

        public class Cat {
                public void eat() {
                        System.out.println("猫吃鱼");
                }
        }
        
CatOperator.java

        package com.itheima_15;

        public class CatOperator {
                // 方法的形参是类名
                public void useCat(Cat c) { // Cat c = new Cat();
                        c.eat();
                }

                // 方法的返回值是类名
                public Cat getCat() {
                        Cat c = new Cat();
                        return c;
                }
        }

CatDemo.java

        package com.itheima_15;
        /*
         * 测试类
         */
        public class CatDemo {
                public static void main(String[] args) {
                        //创建操作类对象，并调用方法
                        CatOperator co = new CatOperator();
                        Cat c = new Cat();
                        co.useCat(c);

                        Cat c2 = co.getCat();
                        c2.eat();
                }
        }
        
        运行结果：
        猫吃鱼
        猫吃鱼

## 抽象类名作为形参和返回值
* 方法的形参是抽象类名，其实需要的是该抽象类的子类对象
* 方法的返回值是抽象类名，其实返回的是该抽象类的子类对象

Animal.java

        package com.itheima_16;

        public abstract class Animal {
                public abstract void eat();  //抽象方法
        }
        
AnimalOperator.java

        package com.itheima_16;

        public class AnimalOperator {
                public void useAnimal(Animal a) { //相当于Animal a = new Cat();
                        a.eat();
                }

                public Animal getAnimal() {
                        Animal a = new Cat();
                        return a;
                }
        }
        
Cat.java

        package com.itheima_16;

        public class Cat extends Animal{
                @Override
                public void eat() {
                        System.out.println("猫吃鱼");
                }
        }
        
AnimalDemo.java

        package com.itheima_16;

        public class AnimalDemo {
                public static void main(String[] args) {
                        //创建操作类对象，并调用方法
                        AnimalOperator ao = new AnimalOperator();
                        Animal a = new Cat();  // 抽象类无法创建对象，只能通过多态的方法创建子类的对象
                        ao.useAnimal(a);

                        Animal a2 = ao.getAnimal(); //相当于new Cat()
                        a2.eat();
                }
        }
        
        运行结果：
        猫吃鱼
        猫吃鱼

## 接口名作为形参和返回值
* 方法的形参是接口名，其实需要的是该接口的实现类对象
* 方法的返回值是接口名，其实返回的是该接口的实现类对象

Jumpping.java

        package com.itheima_17;

        public interface Jumpping {
                void jump();
        }
        
JumppingOperator.java

        package com.itheima_17;

        public class JumppingOperator {
                public void useJumpping(Jumpping j) { //相当于Jumpping j = new Cat();
                        j.jump();
                }

                public Jumpping getJumpping() {
                        Jumpping j = new Cat();
                        return j;
                }
        }
        
Cat.java

        package com.itheima_17;

        public class Cat implements Jumpping{
                @Override
                public void jump() {
                        System.out.println("猫可以跳高了");
                }
        }
        
JumppingDemo.java

        package com.itheima_17;
        /*
         * 测试类
         */
        public class JumppingDemo {
                public static void main(String[] args) {
                        //创建操作类对象，并调用方法
                        JumppingOperator jo = new JumppingOperator();
                        Jumpping j = new Cat();
                        jo.useJumpping(j);

                        Jumpping j2 = jo.getJumpping(); //相当于new Cat()
                        j2.jump();
                }
        }
        运行结果：
        猫可以跳高了
        猫可以跳高了

# 内部类
## 内部类概述
内部类：就是在一个类中定义一个类。举例：在一个类A的内部定义一个类B，类B就被称为内部类

* 内部类的定义格式：

        public class 类名{
                修饰符 class 类名{
                }
        }
        
* 范例：

        public class Outer{
                public class Inner{
                }
        }
        
内部类的访问特点
* 内部类可以直接访问外部类的成员，包括私有
* 外部类要访问内部类的成员，必须创建对象

Outer.java

        package com.itheima_18;

        public class Outer {
                private int num = 10;

                public class Inner {
                        public void show() {
                                System.err.println(num);  // 内部类可以直接访问外部类的成员，包括私有
                        }
                }

                public void method() {
        //		show(); 编译报错，因为外部类无法直接访问内部类的成员，必须创建对象

                        Inner i = new Inner();
                        i.show();
                }
        }

## 成员内部类
按照内部类在类中定义的位置不同，可以分为如下两种形式
* 在类的成员位置：成员内部类
* 在类的局部位置：局部内部类

成员内部类，外界如何创建对象使用呢？
* 格式：外部类名.内部类名 对象名 = 外部类对象.内部类对象;
* 范例：Outer.Inner oi = new Outer().new Inner();
* **注意：** 当内部类改为private修饰的时候，此方法失效。
* 一般的做法是在外部类中，创建一个方法，方法里面创建内部类的对象，从而访问内部类中的方法。外界只需要创建外部类的对象，使用外部类中的方法，达到访问内部类中方法的目的。

Outer.java

        package com.itheima_19;

        public class Outer {
                private int num = 10;
                // 内部类一般用private修饰符
                // 可是这样会导致外界不能创建该内部类的对象了
                // 一般的做法是在外部类中，创建一个方法，方法里面创建内部类的对象，从而访问内部类中的方法。外界只需要创建外部类的对象，使用外部类中的方法，达到访问内部类中方法的目的。
                private class Inner {
                        public void show() {
                                System.out.println(num);
                        }
                }

                public void method() {
                        Inner i = new Inner();
                        i.show();
                }
        }

InnerDemo.java

        package com.itheima_19;
        /*
         * 测试类
         */
        public class InnerDemo {
                public static void main(String[] args) {
                        //创建内部类对象，并调用方法
        //		Inner i = new Inner();  外界无法直接创建内部类对象

                        // 当内部类改为private修饰的时候，此方法就失效了（其实，一般也不会使用这种方法）
        //		Outer.Inner oi = new Outer().new Inner();
        //		oi.show();
                        
                        // 一般做法
                        Outer o = new Outer();
                        o.method();
                }
        }
        运行结果：
        10

## 局部内部类
局部内部类是在方法中定义的类，所以外界是无法直接使用的，需要在方法内部创建对象并使用。

该类可以直接访问外部类的成员，也可以访问方法内部的局部变量。

Outer.java

        package com.itheima_01;

        public class Outer {
            private int num = 10;
            public void method() {
                int num2 = 20;
                // 在方法中定义的类，叫做“局部内部类”
                class Inner {
                    public void show() {
                        System.out.println(num);
                        System.out.println(num2);
                    }
                }

                Inner i = new Inner();
                i.show();
            }
        }

OuterDemo.java

        package com.itheima_01;
        /*
            测试类
         */
        public class OuterDemo {
            public static void main(String[] args) {
                Outer o = new Outer();
                o.method();
            }
        }
        
        运行结果：
        10
        20

## 匿名内部类
前提：存在一个类或者接口，这里的类可以是具体类也可以是抽象类

* 格式：
        
        new 类名或者接口名() {
                重写方法;
        };
        
* 范例：

        new Inter() {
                public void show() {
                }
        };   // 注意这里有一个分号
 
**本质：是一个继承了该类或者实现了该接口的子类匿名对象**

Outer.java

        package com.itheima_02;

        public class Outer {
            public void method() {
                // 匿名内部类，实际上是一个对象
                // 1.调用一次直接在对象后面加.show()即可，
                new Inter() {
                    @Override
                    public void show() {
                        System.out.println("匿名内部类");
                    }
                }.show();

                // 2.如果想多次调用，需要用多态的形式赋值给Iner这个接口
                Inter i = new Inter() {
                    @Override
                    public void show() {
                        System.out.println("匿名内部类");
                    }
                };

                // 3.接下来就可以多次调用show()方法了
                i.show();
                i.show();
            }
        }

Inter.java

        package com.itheima_02;

        public interface Inter {
            void show();
        }
        
OuterDemo.java

        package com.itheima_02;
        /*
            测试类
         */
        public class OuterDemo {
            public static void main(String[] args) {
                Outer o = new Outer();
                o.method();
            }
        }
        
        运行结果：
        匿名内部类
        匿名内部类
        匿名内部类

## 匿名内部类在开发中的使用
使用 匿名内部类 代替 不断新增的接口实现类对象，因为 匿名内部类 本质上就是一个**实现接口的匿名对象**。

Jumpping.java

        package com.itheima_03;
        /*
            跳高接口
         */
        public interface Jumpping { // new Cat(); new Dog();
            void jump();
        }
        
JumppingOperator.java

        package com.itheima_03;
        /*
            接口操作类，里面有一个方法，方法的参数是接口名
         */
        public class JumppingOperatior {
            public void method(Jumpping j) {
                j.jump();
            }
        }
        
Cat.java

        package com.itheima_03;

        public class Cat implements Jumpping{
            @Override
            public void jump() {
                System.out.println("猫可以跳高了");
            }
        }

Dog.java

        package com.itheima_03;

        public class Dog implements Jumpping{
            @Override
            public void jump() {
                System.out.println("狗可以跳高了");
            }
        }
        
JumppingDemo.java

        package com.itheima_03;
        /*
            测试类
         */
        public class JumppingDemo {
            // 需求：创建接口操作类的对象，调用method方法
            public static void main(String[] args) {
                JumppingOperatior jo = new JumppingOperatior();
                Jumpping j = new Cat();
                jo.method(j);
                Jumpping j2 = new Dog();
                jo.method(j2);
                System.out.println("----------");
                // 上面代码有个问题
                // 随着接口实现类对象的不断增多，需要构造更多的接口实现类，非常繁琐
                // 解决方案：用匿名内部类代替实现类对象，因为匿名内部类本质上就是一个对象

                jo.method(new Jumpping() {
                    @Override
                    public void jump() {
                        System.out.println("猫可以跳高了");
                    }
                });

                jo.method(new Jumpping() {
                    @Override
                    public void jump() {
                        System.out.println("狗可以跳高了");
                    }
                });

            }

        }
        
        运行结果：
        猫可以跳高了
        狗可以跳高了
        ----------
        猫可以跳高了
        狗可以跳高了
        
# 基本类型包装类
## 基本类型包装类概述
将基本数据类型封装成对象的好处在于可以在对象中定义更多的功能方法操作该数据

常用的操作之一：用基于数据类型与字符串之间的转换

IntegerDemo.java

        package itheima_04;
        /*
            基本类型包装类
         */
        public class IntegerDemo {
            public static void main(String[] args) {
                //需求：我要判断一个数据是否在 int 范围内
                System.out.println(Integer.MIN_VALUE);
                System.out.println(Integer.MAX_VALUE);
            }
        }
        运行结果：
        -2147483648
        2147483647

## Integer 类的概述和作用
Inter: 包装一个对象中的原始类型int的值

* 注意观察代码里面，有过时和未过时两种方法。
* 过时采用的是构造方法，使用的时候需要用 new 关键字来构建对象；
* 未过时采用的是静态方法，使用的时候不需要new，直接使用即可；
* static 修饰的方法，立即推 “直接用类名调用”，比如使用valueOf()的方式，就是Integer.valueof();

IntegerDemo.java

        package itheima_04;
        /*
            构造方法：
                public Integer(int value)：根据 int 值创建 Integer 对象（过时）
                public Integer(String s): 根据 String 值创建 Integer 对象（过时）

            上面两种方法已经过时了，采用下面两种方法替代。

            静态方法获取对象：
                public static Integer valueOf (int i): 返回表示指定的 int 值的 Integer 实例
                public static Integer valueOf (String s): 返回一个保存指定值的 Integer 对象 String
         */
        public class IntegerDemo {
            public static void main(String[] args) {
                /*
                // 过时
                // 因为给的是构造方法，所以使用 new 关键字，来构建对象
                Integer i1 = new Integer(100);
                System.out.println(i1);
                Integer i2 = new Integer("100");
                System.out.println(i2);
                 */

                // 未过时
                // 因为给的是静态方法构造对象，所以不需要new，直接使用该方法就行
                Integer i1 = Integer.valueOf(100);
                System.out.println(i1);
                Integer i2 = Integer.valueOf("100");
                System.out.println(i2);
            }
        }
        运行结果：
        100
        100

## int 和 String 的相互转换
基本类型包装类的最常见操作就是：用于基本类型和字符串之间的相互转换

1. int 转换为 String

&emsp;&emsp;&emsp;&emsp;public static String valueOf(int i): 返回 int 参数的字符串表示形式。该方法是String类中的方法。

2. String 转换为 int

&emsp;&emsp;&emsp;&emsp;public static int parseInt(String s): 将字符串解析为 int 类型。该方法是Integer类中的方法。

IntegerDemo.java

        package com.itheima_05;
        /*
            int和String的互相转换
         */
        public class IntegerDemo {
            public static void main(String[] args) {
                // int to String
                int num = 100;
                // 方式1
                String s1 = "" + num;
                System.out.println(s1);
                // 方式2
                //public static String valueOf (int i)
                String s2 = String.valueOf(num);
                System.out.println(s2);
                System.out.println("-----------");

                // String to int
                String s = "100";
                // 方式1
                // String --- Integer --- int
                Integer i = Integer.valueOf(s);  // String to Integer
                int x = i.intValue();            // Integer to int
                System.out.println(x);
                // 方式2
                int y = Integer.parseInt(s);
                System.out.println(y);
            }
        }
        
        运行结果：
        100
        100
        -----------
        100
        100

## 自动装箱和拆箱
* 装箱：把基本数据类型转换为对应的包装类类型
* 拆箱：把包装类类型转换为对应的基本数据类型

注意：在使用包装类类型的时候，如果做操作，最好先判断是否为null

&emsp;&emsp;&emsp;&emsp;我们推荐是，只要有对象，在使用前就必须进行不为null的判断

Integer.java

        package com.itheima_06;

        public class IntegerDemo {
            public static void main(String[] args) {
                // 装箱：把基本数据类型转换为对应的包装类类型
                Integer i = Integer.valueOf(100);
                Integer ii = 100;  // 相当于Integer.valueOf(100);

                // 拆箱：把包装类类型转换为对应的基本数据类型
                ii += 200;   // 自动拆箱
                ii = ii.intValue() + 200;   // 编译器会自动拆箱，不需要用intValue()进行手动拆箱
                System.out.println(ii);

                Integer iii = null;
                if (iii != null) {
                    iii += 300;  // 包装类在自动拆箱时不允许为空，所以开发时必须提前判断是否为空
                }
            }
        }
        
        运行结果：
        500

## 异常
### 异常概述
异常：就是程序出现了不正常的情况

![3](https://github.com/CamWu-cyber/Tencent/blob/main/java/%E5%9B%BE%E7%89%87/3.png)

Error: 严重问题，不需要处理

Exception: 称为异常类，它表示程序本身可以处理的问题

* RuntimeException: 在编译期是不检查的，出现问题后，需要我们回来修改代码
* 非RuntimeException: 编译期就必须处理的，否则程序不能通过编译，就更不能正常运行了

### JVM的默认处理方案
如果程序出了问题，我们在没有做任何处理，最终JVM会做默认的处理
* 把异常的名称，异常原因及异常出现的位置等信息输出在了控制台
* 程序停止执行

### 异常处理
如果程序出现了问题，我们需要自己来处理，有两种方案：
* try ... catch ...
* throws

### 异常处理直 try...catch...
* 格式：

        try {
                可能出现异常的代码；
        } catch (异常类名 变量名) {
                异常的处理代码；
        }

执行流程：

程序从try里面的代码开始执行

出现异常，会自动生成一个异常类对象，该异常对象将被提交给Java运行时系统

当Java运行时系统接收到异常对象时，会到 catch 中去找匹配的异常类，找到后进行异常的处理

执行完毕之后，程序还可以继续往下执行

ExceptionDemo02.java

        package com.itheima_01;
        /*
            try {
                  可能出现异常的代码；
              } catch (异常类名 变量名) {
                      异常的处理代码；
              }
         */
        public class ExceptionDemo02 {
            public static void main(String[] args) {
                System.out.println("开始");
                method();
                System.out.println("结束");
            }

            public static void method() {
                try {
                    int[] arr = {1,2,3};
                    System.out.println(arr[3]);  //new ArrayIndexOutOfBoundsException
                } catch (ArrayIndexOutOfBoundsException e) {
        //            System.out.println("你访问的数组索引不存在");
                    e.printStackTrace();  //输出异常信息，程序不会中断
                }
            }
        }
        
        运行结果：
        开始
        结束
        java.lang.ArrayIndexOutOfBoundsException: Index 3 out of bounds for length 3
                at com.itheima_01.ExceptionDemo02.method(ExceptionDemo02.java:19)
                at com.itheima_01.ExceptionDemo02.main(ExceptionDemo02.java:12)

### Throwable的成员方法

![4](https://github.com/CamWu-cyber/Tencent/blob/main/java/%E5%9B%BE%E7%89%87/4.png)

ExceptionDemo02.java

        package com.itheima_01;
        /*
            public String getMessage(): 返回此 throwable 的详细消息字符串
            public String toString(): 返回此可抛出的简短描述
            public void printStackTrace(): 把异常的错误信息输出在控制台
         */
        public class ExceptionDemo02 {
            public static void main(String[] args) {
                System.out.println("开始");
                method();
                System.out.println("结束");
            }

            public static void method() {
                try {
                    int[] arr = {1,2,3};
                    System.out.println(arr[3]);  //如果出现异常会产生一个异常对象 new ArrayIndexOutOfBoundsException();
                } catch (ArrayIndexOutOfBoundsException e) {
        //            e.printStackTrace();  //输出异常信息，程序不会中断

        //            System.out.println(e.getMessage());
                    //Index 3 out of bounds for length 3 异常的原因
                    //选中方法 + ctrl b = 查看源码

        //            System.out.println(e.toString());
                    //java.lang.ArrayIndexOutOfBoundsException: Index 3 out of bounds for length 3
                    //异常的类名+异常的原因，也就是说包含了getMessage()的内容

                    e.printStackTrace();  // 输出最全面的异常信息，所以一般都用此方法
                }
            }
        }

        /*
            1.通过查看源码可知，getMessage方法返回的detailMessage是由Throwable构造方法传入的message得到的。
            2.实现原理简化如下：
            public class Throwable {
                private String detailMessage;
                public Throwable(String message) { // 构造方法
                    detailMessage = message;
                }
                public String getMessage() {
                    return detailMessage;
                }
            }
         */
         
         运行结果：
         开始
        结束
        java.lang.ArrayIndexOutOfBoundsException: Index 3 out of bounds for length 3
                at com.itheima_01.ExceptionDemo02.method(ExceptionDemo02.java:17)
                at com.itheima_01.ExceptionDemo02.main(ExceptionDemo02.java:10)


### 异常处理之 throws
虽然我们通过try...catch...可以对异常进行处理，但是并不是所有的情况我们都有权限进行异常的处理

也就是说，有些时候可能出现的异常我们是处理不了的，这个时候怎么办呢？

针对这种情况，Java提供了 throws 的处理方案

格式：

        throws 异常类名;
        
**注意：这个格式是跟在方法的括号后面的**

* 编译时异常必须要进行处理，有两种处理方案：try...catch...或者 throws，如果采用 throws 这种方案，将来谁调用谁处理
* 运行时异常可以不处理，出现问题后，需要我们回来修改代码

### 自定义异常
Java中异常的父类有 RuntimeException 和 Exception，所以自定义异常的实现需要继承Exception就行

格式：

        public class 异常类名 extends Exception {
                无参构造
                有参构造
        }
        
范例：

        public class ScoreException extends Exception {
                public ScoreException() {}
                public ScoreException(String message) {
                        super(message);
                }
        }

ScoreException.java

        package com.itheima_02;

        public class ScoreException extends Exception {
            public ScoreException() {}
            public ScoreException(String message) {
                super(message);
            }
        }        
        
Teacher.java

        package com.itheima_02;

        public class Teacher {
            public void checkScore(int score) throws ScoreException{
                if(score<0 || score>100) {
        //            throw new ScoreException();
                    throw new ScoreException("你给的信息有误，应该在0-100之间");  // 使用 throw 手动抛出异常信息
                } else {
                    System.out.println("分数正常");
                }
            }
        }
        
TeacherTest.java

        package com.itheima_02;

        import java.util.Scanner;

        public class TeacherTest {
            public static void main(String[] args) {
                Scanner sc = new Scanner(System.in);
                System.out.println("请输入分数：");
                int score = sc.nextInt();

                Teacher t = new Teacher();
                try {
                    t.checkScore(score);
                } catch (ScoreException e) {
                    e.printStackTrace();
                }
            }
        }
        
        运行结果：
        请输入分数：
        120
        com.itheima_02.ScoreException: 你给的信息有误，应该在0-100之间
                at com.itheima_02.Teacher.checkScore(Teacher.java:7)
                at com.itheima_02.TeacherTest.main(TeacherTest.java:13)

### throws 和 throw 的区别
throws
* 用在方法声明后面，跟的是异常类名
* 表示抛出异常，由该方法的调用者来处理
* 表示出现异常的一种可能性，并不一定会发生这些异常

throw
* 用在方法体内，跟的是异常对象名
* 表示抛出异常，由方法体内的语句处理
* 执行throw 一定抛出了某种异常

## 集合体系（数据结构）
### 集合体系结构
![5](https://github.com/CamWu-cyber/Tencent/blob/main/java/%E5%9B%BE%E7%89%87/5.jpg)

蓝色框框表示接口，红色框框表示实现类。

先学习接口，这样后面学习实现类的时候只用学习特定的方法就行。

接口无法创建对象，所以使用的时候就用 实现类创建对象并实例化。

## 进程和线程
### 实现多线程
#### 进程
进程：是正在运行的程序
* 是系统进行资源分配和调用的独立单位
* 每一个进程都有它自己的内存空间和系统资源

#### 线程
线程：是进程中的单个顺序控制流，是一条执行路径
* 单线程：一个进程如果只有一条执行路径，则称为单线程程序
* 多线程：一个进程如果有多条执行路径，则称为多线程程序

#### 多线程的实现方式
方式1：继承Thread类
* 定义一个类MyThread继承Thread类
* 在MyThread类中重写run()方法
* 创建MyThread类的对象
* 启动线程

两个小问题：
* 为什么要重写run()方法？

&emsp;&emsp;&emsp;&emsp;因为run()是用来封装被线程执行的代码，每个程序的功能不同，run()方法的内容当然也不同，所以需要重写。

* run()方法和start()方法的区别？

&emsp;&emsp;&emsp;&emsp;run()：封装线程执行的代码，直接调用，相当于普通的方法的调用

&emsp;&emsp;&emsp;&emsp;start()：启动线程；然后由JVM调用此线程的run()方法

MyThread.java

        package com.itheima_02;

        public class MyThread extends Thread{
            @Override
            public void run() {
                for(int i=0;i<100;i++){
                    System.out.println(i);
                }
            }
        }
        
MyThreadDemo.java

        package com.itheima_02;
        /*
            方式1：继承Thread类
                1.定义一个类MyThread继承Thread类
                2.在MyThread类中重写run()方法
                3.创建MyThread类的对象
                4.启动线程
         */
        public class MyThreadDemo {
            public static void main(String[] args) {
                MyThread my1 = new MyThread();
                MyThread my2 = new MyThread();

        //        my1.run();
        //        my2.run();

                //void start() 导致此线程开始执行，Java虚拟机调用此线程的run方法
                my1.start();
                my2.start();
            }
        }
        
        运行结果：
        交叉输出0-99的数字

#### 设置和获取线程名称
Thread类中设置和获取线程名称的方法
* void setName(String name): 将此线程的名称更改为等于参数name
* String getName(): 返回此线程的名称

如何获取main()方法所在的线程名称？
* public static Thread currentThread(): 返回对当前正在执行的线程对象的引用

IDEA 弹出 Structure 用于查看代码结构：view(工具栏) -> Tool Windows -> Structure

part 1: getName()方法（附上源码解析）

MyThread.java

        package com.itheima_02;

        public class MyThread extends Thread{
            @Override
            public void run() {
                for(int i=0;i<100;i++){
                    System.out.println(getName()+":"+i);
                }
            }
        }

        /*
        1. 通过ctrl+d查看源码，找到Thread类的无参构造方法。发现调用了nextThreadNum()方法。ctrl + 点击该方法，即可跳转
        public Thread() {
            this(null, null, "Thread-" + nextThreadNum(), 0);
        }

        2. 跳转到nextThreadNum()方法后，发现return了threadInitNumber变量，双击thredInitNumber变量，发现它被定义为静态的整型变量，所以是从0开始计数的。又是后++，所以返回值也是从0开始的。
        private static int threadInitNumber;//0，1，2
        private static synchronized int nextThreadNum() {
            return threadInitNumber++;//0，1，...
        }

        3. 查看第一步的this究竟是那种构造方法，给字符串赋值的，ctrl+点击this，跳转到了另一个构造方法，如下：
        public Thread(ThreadGroup group, Runnable target, String name,
                          long stackSize) {
            this(group, target, name, stackSize, null, true);
        }


        4. 发现这个构造方法里面还有this，说明里面又套了另外一个构造方法，继续cttl+点击this，跳转下一个构造方法，如下：
        private Thread(ThreadGroup g, Runnable target, String name,
                           long stackSize, AccessControlContext acc,
                           boolean inheritThreadLocals) {
            this.name = name;
        }
        5. 此构造方法内容有点多，我们只关注想要的部分，所以从变量入手，在第一个this中发现传入的参数，只有第三个是有意义的，对应到目前的构造方法里面，就是name这个变量。
        6. 找到跟name有关的语句，就是this.name = name; 说明从外面传入的字符串，最终通过name变量传递给了Thread类中的成员变量name，下一步我们去看看name是如何定义的。
        7. ctrl+点击this.name，跳转到成员变量name,发现定义为：private volatile String name;
        8. 自此成员变量name如何被赋值的路径就被我们找到了。现在返回去看getName()方法，究竟做了什么操作呢？
        9. 点击getName()方法，ctrl+b，跳转到getName()方法的源码部分。如下：
        public final String getName() {
            return name;
        }
        10. 原来getName()方法返回了刚才赋值的成员变量name，这也说明了为什么getName()方法会有“Thread-0”这种默认输出了。
         */
         
MyThreadDemo.java

        package com.itheima_02;

        public class MyThreadDemo {
            public static void main(String[] args) {
                MyThread my1 = new MyThread();
                MyThread my2 = new MyThread();

                my1.start();
                my2.start();
            }
        }
        运行结果：
        Thread-1:99
        Thread-0:98
        (Thread-0, Thread-1交替出现)

part 2: setName()方法

MyThread.java

        package com.itheima_02;

        public class MyThread extends Thread{
            @Override
            public void run() {
                for(int i=0;i<100;i++){
                    System.out.println(getName()+":"+i);
                }
            }
        }

        11. 再来看看setName()方法，如下：
        public final synchronized void setName(String name) {
            this.name = name;
        }
        更改了成员变量name的值，所以就更改了线程的名称。
        
MyThreadDemo.java

package com.itheima_02;

        public class MyThreadDemo {
            public static void main(String[] args) {
                MyThread my1 = new MyThread();
                MyThread my2 = new MyThread();

                my1.setName("高铁");
                my2.setName("飞机");

                my1.start();
                my2.start();
            }
        }
        运行结果：
        高铁、飞机交替出现
        
part 3: 不使用setName()方法，使用带参构造方法来给线程赋值

MyThread.java

        package com.itheima_02;

        public class MyThread extends Thread{
            public MyThread() {}

            //自己写一个带参的构造方法，才能访问父类的带参构造方法
            public MyThread(String name) {
                super(name);
            }

            @Override
            public void run() {
                for(int i=0;i<100;i++){
                    System.out.println(getName()+":"+i);
                }
            }
        }
        
        12. 父类的带参构造方法，如下：
        public Thread(String name) {
            this(null, null, name, 0);
        }
        这里的this指向的第三步的那个构造方法，后面的跳转步骤是一样的，就不复写了。

MyThreadDemo.java

        package com.itheima_02;

        public class MyThreadDemo {
            public static void main(String[] args) {
        //      MyThread my1 = new MyThread();
        //      MyThread my2 = new MyThread();

        //        my1.setName("高铁");
        //        my2.setName("飞机");

                //Thread(String name)
                MyThread my1 = new MyThread("高铁");
                MyThread my2 = new MyThread("飞机");

                my1.start();
                my2.start();
            }
        }
        运行结果：
        高铁、飞机交替出现
        
**引申：已知父类有带参的构造方法，如何通过子类调用呢？**

回答：

        1. 定义子类时使用 extends 父类名，来继承父类；

        2. 在子类中写一个带参的构造方法，通过super(参数)指向父类的带参构造方法

part 4: currentThread()方法

        MyThreadDemo.java

        package com.itheima_02;

        public class MyThreadDemo {
            public static void main(String[] args) {        
                //static Thread currentThread() 返回对当前正在执行的线程对象的引用
                System.out.println(Thread.currentThread().getName());
            }
        }
        运行结果：
        main

#### 线程调度
线程有两种调度模型
* 分时调度模型：所有线程轮流使用CPU的使用权，平均分配每个线程占用CPU的时间片
* 抢占式调度模型：优先让优先级高的线程使用CPU，如果线程的优先级相同，那么会随机选择一个，优先级高的线程获取的CPU时间片相对多一些。

Java使用的是抢占式调度模型

假如计算机只有一个CPU，那么CPU在某一个时刻只能执行一条命令，线程只有得到CPU时间片，也就是使用权，才可以执行命令。所以说多线程程序执行是有**随机性**的，因为谁抢到CPU的使用权是不一定的。

Thread类中设置和获取线程优先级的方法：
* public final int getPriority(): 返回此线程的优先级
* public final setPriority(int newPriority): 更改此线程的优先级

ThreadPriority.java

        package com.itheima_03;

        public class ThreadPriority extends Thread{
            @Override
            public void run() {
                for (int i=0; i<100;i++){
                    System.out.println(getName() + ":" + i);
                }
            }
        }

ThreadPriorityDemo.java

        package com.itheima_03;
        /*
            Thread类中设置和获取线程优先级的方法：
                public final int getPriority(): 返回此线程的优先级
                public final setPriority(int newPriority): 更改此线程的优先级
         */
        public class ThreadPriorityDemo {
            public static void main(String[] args) {
                ThreadPriority tp1 = new ThreadPriority();
                ThreadPriority tp2 = new ThreadPriority();
                ThreadPriority tp3 = new ThreadPriority();

                tp1.setName("高铁");
                tp2.setName("飞机");
                tp3.setName("汽车");

                //public final int getPriority(): 返回此线程的优先级
                //默认优先级都为5
        //        System.out.println(tp1.getPriority()); //5
        //        System.out.println(tp2.getPriority()); //5
        //        System.out.println(tp3.getPriority()); //5

                //public final setPriority(int newPriority): 更改此线程的优先级
                //newPriority的取值范围在[1,10]
                tp1.setPriority(5);
                tp2.setPriority(10);
                tp3.setPriority(1);

                tp1.start();
                tp2.start();
                tp3.start();
            }
        }
        
        运行结果：
        高铁、飞机、汽车交替出现，飞机最先出现，其次是高铁，最后是汽车

优先级越高表示该线程占用CPU的时间越久，所以优先输出它的结果，不代表一定只输出它的结果。

#### 线程控制

![6](https://github.com/CamWu-cyber/Tencent/blob/main/java/%E5%9B%BE%E7%89%87/6.png)

* Sleep

ThreadSleep.java

        package com.itheima_03;

        public class ThreadSleep extends Thread {
            @Override
            public void run() {
                for (int i = 0; i < 100; i++) {
                    System.out.println(getName() + ":" + i);
                    // try catch 快捷键 ctrl+alt+t
                    try {
                        Thread.sleep(1000);
                    } catch (InterruptedException e) {
                        throw new RuntimeException(e);
                    }
                }
            }
        }
        
ThreadSleepDemo.java

        package com.itheima_03;
        /*
            static void sleep(long millis): 使当前正在执行的线程停留（暂停执行）指定的毫秒数
         */
        public class ThreadSleepDemo {
            public static void main(String[] args) {
                ThreadSleep ts1 = new ThreadSleep();
                ThreadSleep ts2 = new ThreadSleep();
                ThreadSleep ts3 = new ThreadSleep();

                ts1.setName("曹操");
                ts2.setName("刘备");
                ts3.setName("孙权");

                ts1.start();
                ts2.start();
                ts3.start();
            }
        }
        运行结果：
        原本三个线程时无序的，加了sleep后，三个线程出现的比较均衡了

* Join

ThreadJoin.java

        package com.itheima_03;

        public class ThreadJoin extends Thread {
            @Override
            public void run() {
                for (int i = 0; i < 100; i++) {
                    System.out.println(getName() + ":" + i);
                }
            }
        }
        
ThreadJoinDemo.java

        package com.itheima_03;
        /*
            void join(): 让调用它的线程执行完毕之后，其他线程才开始执行
         */
        public class ThreadJoinDemo {
            public static void main(String[] args) {
                ThreadJoin tj1 = new ThreadJoin();
                ThreadJoin tj2 = new ThreadJoin();
                ThreadJoin tj3 = new ThreadJoin();

                tj1.setName("康熙");
                tj2.setName("四阿哥");
                tj3.setName("八阿哥");

                tj1.start();
                try {
                    tj1.join();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
                tj2.start();
                tj3.start();
            }
        }
        运行结果：
        等康熙执行完毕后，其他两个线程才开始执行
        
* Deamo

ThreadDeamo.java

        package com.itheima_03;

        public class ThreadDeamon extends Thread {
            @Override
            public void run() {
                for (int i = 0; i < 100; i++) {
                    System.out.println(getName() + ":" + i);
                }
            }
        }

ThreadDeamoDemo.java

        package com.itheima_03;
        /*
            void setDaemon (boolean on)，将此线程标记为守护线程，当运行的线程都是守护线程时，Java虚拟机将退出
         */
        public class ThreadDeamonDemo {
            public static void main(String[] args) {
                ThreadDeamon td1 = new ThreadDeamon();
                ThreadDeamon td2 = new ThreadDeamon();

                td1.setName("关羽");
                td2.setName("张飞");

                //设置主线程为刘备
                Thread.currentThread().setName("刘备");

                //设置守护线程
                //主线程执行完毕后，守护线程也会很快结束
                td1.setDaemon(true);
                td2.setDaemon(true);

                td1.start();
                td2.start();

                for (int i = 0; i < 10; i++) {
                    System.out.println(Thread.currentThread().getName()+":"+i);
                }
            }
        }
        
        运行结果：
        刘备执行完毕后，关羽和张飞也很快结束了

#### 线程生命周期

![7](https://github.com/CamWu-cyber/Tencent/blob/main/java/%E5%9B%BE%E7%89%87/7.png)
)

#### 多线程的实现方式（推荐）
方式2：实现Runnable接口
* 定义一个类MyRunnable实现Runnable接口
* 在MyRunnable类中重写run()方法
* 创建MyRunnable类的对象
* 创建Thread类的对象，把MyRunnable对象作为构造方法的参数
* 启动线程

多线程的实现方案有两种
* 继承Thread类
* 实现Runnable接口

相比继承Thread类，实现Runnable接口的好处
* 避免了Java单继承的局限性
* 适合多个相同程序的代码去使用同一资源的情况，把线程和程序的代码、数据有效分离，较好的体现了面向对象的设计思想

MyRunnable.java

        package com.iteima_04;

        public class MyRunnable implements Runnable{
            @Override
            public void run() {
                for (int i = 0; i<100; i++) {
                    System.out.println(Thread.currentThread().getName()+":"+i);
                }
            }
        }

MyRunnableDemo.java

        package com.iteima_04;

        public class MyRunnableDemo {
            public static void main(String[] args) {
                // 创建MyRunnable类的对象
                MyRunnable my = new MyRunnable();

                // 创建Thread类的对象，把MyRunnable对象作为构造方法的参数
                Thread t1 = new Thread(my, "高铁");
                Thread t2 = new Thread(my, "飞机");

                // 启动线程
                t1.start();
                t2.start();
            }
        }
        运行结果：
        高铁、飞机交替出现

### 线程同步
#### 同步代码块解决数据安全问题
问题：不同的线程会有共享数据的情况，这是不被允许的，否则变量的值会混乱。

解决：首先搞清楚为什么出现问题？（这也是我们判断多线程程序是否会有数据安全问题的标准）
* 是否是多线程环境
* 是否有共享数据
* 是否有多条语句操作共享数据

如何解决多线程安全问题呢？
* 基本思想：让程序没有安全问题的环境

怎么实现呢？
* 把多条语句操作共享数据的代码给锁起来，让任意时刻只能有一个线程执行即可。

锁多条语句操作共享数据，可以使用同步代码块实现
* 格式：

        synchronized(任意对象){
                多条语句操作共享数据的代码
        }
        
* synchronized(任意对象)：就相当于给代码加锁了，任意对象就可以看成是一把锁。

SellTicket.java

        package com.ithema_05;

        public class SellTicket implements Runnable{
            private int ticket = 100;
            private Object obj = new Object();

            @Override
            public void run() {
                while(true) {
                    synchronized (obj) {
                        if (ticket > 0) {
                            try {
                                Thread.sleep(100);
                            } catch (InterruptedException e) {
                                e.printStackTrace();
                            }
                            System.out.println(Thread.currentThread().getName() + "正在出售第" + ticket + "张票");
                            ticket--;
                        }
                    }
                }
            }
        }
        
SellTicketDemo.java

        package com.ithema_05;

        public class SellTicketDemo {
            public static void main(String[] args) {
                SellTicket st = new SellTicket();

                Thread t1 = new Thread(st, "窗口1");
                Thread t2 = new Thread(st, "窗口2");
                Thread t3 = new Thread(st, "窗口3");

                t1.start();
                t2.start();
                t3.start();
            }
        }
        运行结果：
        窗口1，2，3交替出现，且票数正常递减，没有数据安全问题了

#### 同步方法
同步方法：就是把synchronized关键字加到方法上

* 格式：
&emsp;&emsp;&emsp;&emsp;修饰符 synchronized 返回值类型 方法名（方法参数）{ }

同步方法的锁对象是什么呢？
* this

同步静态方法：就是把synchronized关键字加到静态方法上
* 格式：
&emsp;&emsp;&emsp;&emsp;修饰符 static synchronized 返回值类型 方法名（方法参数）{ }

同步静态方法的锁对象是什么呢？
* 类名.class

SellTicket.java

        package com.itheima_04;

        public class SellTicket implements Runnable{
            private static int tickets = 100;
            private Object obj = new Object();
            private int x = 0;

            @Override
            public void run() {
                while(true) {
                    if (x%2 == 0) {
        //                synchronized (this) {
                        synchronized (SellTicket.class) {
                            if (tickets > 0) {
                                try {
                                    Thread.sleep(100);
                                } catch (InterruptedException e) {
                                    e.printStackTrace();
                                }
                                System.out.println(Thread.currentThread().getName() + "正在出售" + tickets + "张票");
                                tickets--;
                            }
                        }
                    } else {
                        sellTicket();
                    }
                    x++;
                }
            }

            private static synchronized void sellTicket() {
                if (tickets > 0) {
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread().getName() + "正在出售" + tickets + "张票");
                    tickets--;
                }
            }
        }

SellTicketsDemo.java

        package com.itheima_04;

        public class SellTicketsDemo {
            public static void main(String[] args) {
                SellTicket st = new SellTicket();

                Thread t1 = new Thread(st, "窗口1");
                Thread t2 = new Thread(st, "窗口2");
                Thread t3 = new Thread(st, "窗口3");

                t1.start();
                t2.start();
                t3.start();
            }
        }
        
        运行结果：
        窗口1 2 3 交替出现，并且没有数据安全问题

#### 线程安全的类

下面三个类不存在线程安全的问题，因为它们内部的方法或者方法体均采用了synchronized关键字修饰。在多线程环境下可以直接使用的

* StringBuffer
* Vector
* Hashtable

与上面对应的三个线程不安全的类：
* StringBuilder
* ArrayList
* HashMap

在实际应用中，StringBuffer会使用，Vector和Hashtable一般不会使用，它们被Collections.synchronicedList()替代了.
        
        
        # 创建一个线程安全的List
        List<String> list = Collections.synchronizedList(new ArrayList<String>());
        
### Lock 锁
虽然我们可以理解同步代码块和同步方法的锁对象问题，但是我们并没有直接看到在哪里加上了锁，在哪里释放了锁，为了更清晰的表达如何加锁和释放锁，JDK5以后提供了一个新的锁对象Lock.

Lock实现提供比使用synchronized方法和语句可以获得更广泛的锁定操作

Lock中提供了获得锁和释放锁的方法
* void lock(): 获得锁
* void unlock(): 释放锁

Lock是接口不能直接实例化，这里采用它的实现类ReentrantLock来实例化

ReentrantLock的构造方法
* ReentrantLock(): 创建一个ReentrantLock的实例

SellTicket.java

        package com.ithema_05;

        import java.util.concurrent.locks.Lock;
        import java.util.concurrent.locks.ReentrantLock;

        public class SellTicket implements Runnable{
            private int ticket = 100;
            private Lock lock = new ReentrantLock();

            @Override
            public void run() {
                while(true) {
                    try {
                        lock.lock();  # 加锁
                        if (ticket > 0) {
                            try {
                                Thread.sleep(100);
                            } catch (InterruptedException e) {
                                e.printStackTrace();
                            }
                            System.out.println(Thread.currentThread().getName() + "正在出售第" + ticket + "张票");
                            ticket--;
                        }
                    } finally {
                        lock.unlock();  # 解锁
                    }
                }
            }
        }

SellTicketDemo.java

        package com.ithema_05;

        public class SellTicketDemo {
            public static void main(String[] args) {
                SellTicket st = new SellTicket();

                Thread t1 = new Thread(st, "窗口1");
                Thread t2 = new Thread(st, "窗口2");
                Thread t3 = new Thread(st, "窗口3");

                t1.start();
                t2.start();
                t3.start();
            }
        }
        运行结果：
        窗口1、2、3交替出现，且没有数据安全问题

### 生产者消费者
#### 生产者消费者模式概述
生产者消费者模式是一个十分经典的多线程协作的模式，弄懂生产者消费者问题能够让我们对多线程编程的理解更加深刻。

所谓生产者消费者问题，实际上主要是包含了两类线程：
* 一类是生产者线程用于生产数据
* 一类是消费者线程用于消费数据

为了解耦生产者和消费者之间的关系，通常会采用共享的数据区域，就像一个仓库
* 生产者生产数据之后直接放置在共享数据区中，并不需要关系消费者的行为
* 消费者只需要从共享数据区中去获取数据，并不需要关心生产者的行为

为了体现生产和消费过程中等待和唤醒，Java提供了几个方法拱我们使用，这几个方法在Object类中：
* void wait(): 导致当前线程等待，直到另一个线程调用该对象的notify()方法或notifyAll()方法
* void notify(): 唤醒正在等待对象监视器的单个线程
* void notifyAll(): 唤醒正在等待对象监视器的所有线程

Box.java

        package com.ithema_06;

        public class Box {
            // 定义一个成员变量，表示第x瓶牛奶
            private int milk;
            // 定义一个成员变量，表示奶箱的状态
            private boolean state = false;

            //提供存储牛奶和获取牛奶的操作
            public synchronized void put (int milk) {
                // 如果有牛奶，等待消费
                if(state) {
                    try {
                        wait();
                    } catch (InterruptedException e) {
                        throw new RuntimeException(e);
                    }
                }

                //如果没有牛奶，就生产牛奶
                this.milk = milk;
                System.out.println("送奶工将第"+this.milk+"瓶奶放入奶箱");

                //生产完毕之后，修改奶箱状态
                state = true;

                //唤醒其他等待的线程
                notifyAll();
            }
            public synchronized void get() {
                // 如果没有牛奶，等待生产
                if(!state) {
                    try {
                        wait();
                    } catch (InterruptedException e) {
                        throw new RuntimeException(e);
                    }
                }

                //如果有牛奶，就消费牛奶
                System.out.println("用户拿到第"+this.milk+"瓶奶");

                //消费完毕后，修改奶箱状态
                state = false;

                //唤醒其他等待的线程
                notifyAll();
            }
        }

Producer.java
        
        package com.ithema_06;

        public class Producer implements Runnable{
            private Box b;

            public Producer(Box b) {
                this.b = b;
            }

            @Override
            public void run() {
                for (int i=1; i<=5; i++) {
                    b.put(i);
                }
            }
        }

Customer.java

        package com.ithema_06;

        public class Customer implements Runnable{
            private Box b;
            public Customer (Box b) {
                this.b = b;
            }

            @Override
            public void run() {
                while (true) {
                    b.get();
                }
            }
        }

BoxDemo.java

        package com.ithema_06;

        public class BoxDemo {
            public static void main(String[] args) {
                //创建奶箱对象，这是共享数据区域
                Box b = new Box();

                //创建生产者对象，把奶箱对象作为构造方法参数传递，因为在这个类中要调用存储牛奶的操作
                Producer p = new Producer(b);
                //创建消费者对象，把奶箱对象作为构造方法参数传递，因为在这个类中要调用获取牛奶的操作
                Customer c = new Customer(b);

                //创建2个线程对象，分别把生产者对象和消费者对象作为构造方法参数传递
                Thread t1 = new Thread(p);
                Thread t2 = new Thread(c);

                //启动线程
                t1.start();
                t2.start();
            }
        }
        
        运行结果：
        送奶工将第1瓶奶放入奶箱
        用户拿到第1瓶奶
        送奶工将第2瓶奶放入奶箱
        用户拿到第2瓶奶
        送奶工将第3瓶奶放入奶箱
        用户拿到第3瓶奶
        送奶工将第4瓶奶放入奶箱
        用户拿到第4瓶奶
        送奶工将第5瓶奶放入奶箱
        用户拿到第5瓶奶

## 网络编程入门
### 网络编程三要素
* IP地址

&emsp;&emsp;要想让网络中的计算机能够互相通信，必须为每台计算机指定一个标识号，通过这个标识号来指定要接受数据的计算机和识别发送的计算机，而IP地址就是这个标识号。也就是设备的标识。

* 端口

&emsp;&emsp;网络的通信，本质上是两个应用程序的通信。每台计算机都有很多的应用程序，那么在网络通信中，如何区分这些应用程序呢？如果说IP地址可以唯一标识网络中的设备，那么端口号就可以唯一标识设备中的应用程序了。也就是应用程序的标识。

* 协议

&emsp;&emsp;通过计算机网络可以使多台计算机实现链接，位于同一网络中的计算机在进行连接和通信时需要遵循一定的规则。在计算机网络中，这些连接和通信规则被称为网络通信协议，它对数据的传输格式、传输速率、传输步骤等做了统一规定，通信双方必须同时遵守才能完成数据交换。常见的协议有UDP协议和TCP协议。

### InetAddress 的使用
为了方便我们对IP地址的获取和操作，Java提供了一个类InetAddress供我们使用

InetAddress: 此类表示Internet协议（IP）地址

1. static InetAddress getByName(String host): 确定主机名称的IP地址。主机名称可以是机器名称，也可以是IP地址
2. String getHostName(): 获取此IP地址的主机名
3. String getHostAddress(): 返回文本显示的IP地址字符串

InetAddress.java

        package com.ithema_07;
        import java.net.InetAddress;
        import java.net.UnknownHostException;

        public class InetAddressDemo {
            public static void main(String[] args) throws UnknownHostException {
                InetAddress address = InetAddress.getByName("192.168.0.108"); // 创建ip地址对象
                String name = address.getHostName();
                String ip = address.getHostAddress();
                System.out.println("主机名："+name);
                System.out.println("IP地址："+ip);
            }
        }
        
        运行结果：
        主机名：CAMWU-NB3
        IP地址：192.168.0.108

### 端口
端口：设备上应用程序的唯一标识

端口号：用两个字节表示的整数，它的取值范围是0-65535。其中，0-1023之间的端口号用于一些知名的网络服务和应用，普通的应用程序需要使用1024以上的端口号。如果端口号被另外一个服务或应用所占用，会导致当前程序启动失败。（换一个端口就行）

### 协议
协议：计算机网络中，连接和通信的规则被称为网络通信协议

UDP协议
* 用户数据协议（User Datagram Protocol）
* UDP是无连接通信协议，即在数据传输时，数据的发送端和接收端不建立逻辑连接。
* 由于使用UDP协议消耗资源小，通信效率高，所以通常都会用于音频、视频和普通数据的传输。
* 例如视频会议通常采用UDP协议，因为这种情况即使偶尔丢失一两个数据包，也不会对接收结果产生太大影响。但是在使用UDP协议传送数据时，由于UDP的面向无连接性，不能保证数据的完整性，因此在传输重要数据时不建议使用UDP协议。

TCP协议
* 传输控制协议（Transmission Control Protocol）
* TCP协议是面向连接的通信协议，即传输数据之前，在发送端和接收端建立逻辑连接，然后再传输数据，它提供了两台计算机之间可靠无差错的数据传输。在TCP连接中必须要明确客户端与服务器端，由客户端向服务器端发出连接请求，每次连接的创建都需要经过“三次握手”。
* 三次握手：TCP协议中，在发送数据的准备阶段，客户端与服务器之间的三次交互，以保证连接的可靠

        第一次握手：客户端向服务器端发出连接请求，等待服务器确认
        第二次握手：服务器端向客户端回送一个响应，通知客户端收到了连接请求
        第三次握手：客户端再次向服务端发送确认信息，确认连接

* 完成了三次握手，连接建立后，客户端和服务端就可以开始进行数据传输了。由于这种面向连接的特性，TCP协议可以保证传输数据的安全，所以应用十分广泛。例如上传文件、下载文件、浏览网页等。

### UDP通信程序
#### UDP通信原理
UDP协议是一种不可靠的网络协议，它在通信的两端各建立一个Socket对象，但是这两个Socket只是发送，接收数据的对象。

因此对于UDP协议的通信双方而言，没有所谓的客户端和服务器的概念。

Java提供了DatagramSocket类作为基于UDP协议的Socket.

#### UDP发送数据
发送数据的步骤：
* 创建发送端的Socket对象（DatagramSocket）

&emsp;&emsp;DatagramSocket()

* 创建数据，并把数据打包

&emsp;&emsp;DatagramPacket(byte[] bys, int length, InetAddress address, int port)

* 调用DatagramSocket对象的方法发送数据

&emsp;&emsp;void send(DatagramPacket p)

* 关闭发送端

&emsp;&emsp;void close()

SendDemo.java

        package com.itheima_05;

        import java.io.IOException;
        import java.net.*;

        public class SendDemo {
            public static void main(String[] args) throws IOException {
                //创建发送端的Socket对象（DatagramSocket）
                //技巧：创建一个对象需要使用对应类的构造方法，在API手册里面查询此类的构造方法，选择一个使用就好
                DatagramSocket ds = new DatagramSocket();

                //创建数据，并把数据打包
                //使用DatagramPacket构造方法来创建数据包对象
                //DatagramPacket (byte[] buf, int length, InetAddress address, int port)
                //构造一个数据包，发送长度为length的数据包到指定主机上的指定端口号。
                byte[] bys = "hello,udp".getBytes();
                int length = bys.length;
                InetAddress address = InetAddress.getByName("10.19.140.42");
                int port = 10086;
                DatagramPacket dp = new DatagramPacket(bys,length,address,port);

                //调用DatagramSocket对象的方法发送数据
                ds.send(dp);

                //关闭发送端
                ds.close();
            }
        }
        运行结果：
        啥都没有，因为此时接收端还未构造出来

### UDP接收数据
接受数据的步骤
* 创建接收数据的Socket对象（DatagramSocket）

&emsp;&emsp;DatagramSocket(int port)

* 创建一个数据包，用于接收数据

&emsp;&emsp;DatagramPacket(byte[] buf, int length)

* 调用DatagramSocket对象的方法接收数据

&emsp;&emsp;void receive(DatagramPacket p)

* 解析数据包，并把数据在控制台显示

&emsp;&emsp;byte[] getData()
&emsp;&emsp;int getLength()

* 关闭接收端

&emsp;&emsp;void close()

ReceiveDemo.java

        package com.itheima_05;

        import java.io.IOException;
        import java.net.DatagramPacket;
        import java.net.DatagramSocket;
        import java.net.SocketException;

        public class ReceiveDemo {
            public static void main(String[] args) throws IOException {
                //创建接收端的Socket对象（DatagramSocket）
                //技巧：选择DatagramSocket类的带参构造方法
                //DatagramSocket(int port) 构造数据报套接字并将其绑定到本地主机上的指定端口
                DatagramSocket ds = new DatagramSocket(10086);

                //创建一个数据包，用于接收数据
                //DatagramPacket(byte[] buf, int length) 构造一个 DatagramPacket用于接收长度为 length 数据包
                byte[] bys = new byte[1024];
                DatagramPacket dp = new DatagramPacket(bys,bys.length);

                //调用DatagramSocket对象的方法接收数据
                ds.receive(dp);

                //解析数据包，并把数据在控制台显示
                //byte[] getData() 返回数据缓冲区
                byte[] datas = dp.getData();
                //int getLength() 返回要发送的数据的长度或接收到的数据的长度
                int len = dp.getLength();
                String dataString = new String(datas,0,len);
                System.out.println("数据是："+dataString);

                //关闭接收端
                ds.close();
            }
        }

        运行结果：
        （打开接收端后，再打开发送端，得到结果）
        数据是：hello,udp

### UDP通信程序练习

SendDemo.java

        package com.ithema_08;

        import java.io.BufferedReader;
        import java.io.IOException;
        import java.io.InputStreamReader;
        import java.net.DatagramPacket;
        import java.net.DatagramSocket;
        import java.net.InetAddress;
        import java.net.SocketException;

        /*
            UDP发送数据：
                数据来自于键盘录入，直到输入的数据是886，发送数据结束
         */
        public class SendDemo {
            public static void main(String[] args) throws IOException {
                // 创建发送端的Socket对象（DatagramSocket）
                DatagramSocket ds = new DatagramSocket();

                // 自己封装键盘录入数据
                BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
                String line;
                while ((line = br.readLine()) != null) {
                    // 输入数据是886，发送数据结束
                    if ("886".equals(line)) {
                        break;
                    }

                    // 创建数据，并把数据打包
                    byte[] bys = line.getBytes();
                    DatagramPacket dp = new DatagramPacket(bys, bys.length, InetAddress.getByName("192.168.0.109"), 12345);

                    // 调用DatagramSocket对象的方法发送数据
                    ds.send(dp);
                }

                // 关闭数据
                ds.close();
            }
        }
        运行结果：
        1234（键盘输入）

Receive.java

        package com.ithema_08;

        import java.io.IOException;
        import java.net.DatagramPacket;
        import java.net.DatagramSocket;
        import java.net.SocketException;

        /*
            UDP接收数据：
                因为接收端不知道发送端什么时候停止发送，故采用死循环接收
         */
        public class ReceiveDemo {
            public static void main(String[] args) throws IOException {
                // 创建者接受端的Socket对象（DatagramSocket）
                DatagramSocket ds = new DatagramSocket(12345);

                while(true) {
                    // 创建一个数据包，用于接收数据
                    byte[] bys = new byte[1024];
                    DatagramPacket dp = new DatagramPacket(bys, bys.length);

                    // 调用DatagramSocket对象的方法接收数据
                    ds.receive(dp);

                    // 解析数据包，并把数据在控制台显示
                    System.out.println("数据是："+new String(dp.getData(),0,dp.getLength()));
                }
            }
        }
        运行结果：
        数据是：1234

## TCP通信程序
### TCP通信原理
TCP通信协议是一种可靠的网络协议，它在通信的两端各建立一个Socekt对象，从而在通信的两端形成网络虚拟链路，一旦建立了虚拟的网络链路，两端的程序就可以通过虚拟链路进行通信。

Java对基于TCP协议的网络提供了良好的封装，使用Socekt对象来代表两端的通信端口，并通过Socekt产生IO流来进行网络通信。

Java为客户端提供了Socket类，为服务端提供了ServiceSocket类

### TCP发送数据
发送数据步骤：
1. 创建客户端的Socket对象（Socket）

&emsp;&emsp;&emsp;&emsp;Socket(String host, int port)

2. 获取输出流，写数据

&emsp;&emsp;&emsp;&emsp;OutputStream getOutputStream()

3. 释放资源

&emsp;&emsp;&emsp;&emsp;void close()

ClientDemo.java

        package com.itheima_06;

        import java.io.IOException;
        import java.io.OutputStream;
        import java.net.Socket;

        public class ClientDemo {
            public static void main(String[] args) throws IOException {
                // 创建客户端的Socket对象（Socket）
                // Socket (String hots, int port) 创建流套接字并将其连接到指定主机上的指定端口号
                Socket s = new Socket("100.64.100.6", 10000);

                // 获取输出流，写数据
                // OutputStream getOutputStream() 返回此套接字的输出流
                OutputStream os = s.getOutputStream();
                os.write("hello,tcp,我来了".getBytes());

                // 释放资源
                s.close();
            }
        }
        运行结果：
        报错，因为目前还没有创建服务端

### TCP接收数据
1. 创建服务器的Socket对象（SeverSocket）

&emsp;&emsp;&emsp;&emsp;ServerSocket(int port)

2. 监听客户端连接，返回一个Socket对象

&emsp;&emsp;&emsp;&emsp;Socket accept()

3. 获取输入流，读数据，并把数据显示在控制台

&emsp;&emsp;&emsp;&emsp;InputStream getInputStream()

4. 释放资源

&emsp;&emsp;&emsp;&emsp;void close()

ServerDemo.java

        package com.itheima_06;

        import java.io.IOException;
        import java.io.InputStream;
        import java.net.ServerSocket;
        import java.net.Socket;

        public class ServerDemo {
            public static void main(String[] args) throws IOException {
                // 创建服务器端的Socket对象（ServerSocket）
                // ServerSocket(int port) 创建绑定到指定端口的服务器套接字
                ServerSocket ss = new ServerSocket(50087);

                // Socket accept()侦听要连接到此套接字并接受它
                Socket s = ss.accept();

                // 获取输入流，读数据，并把数据显示在控制台
                InputStream is = s.getInputStream();
                byte[] bys = new byte[1024];
                int len = is.read(bys);
                String data = new String(bys, 0, len);
                System.out.println("数据是："+data);

                // 释放资源
                s.close();
                ss.close();
            }
        }

        运行结果：
        数据是：hello,tcp,我来了

### TCP通信程序练习
练习1
* 客户端：发送数据，接收服务器反馈
* 服务器：接收数据，给出反馈

Client.java

        package com.ithema_09;

        import java.io.IOException;
        import java.io.InputStream;
        import java.io.OutputStream;
        import java.net.Socket;

        public class ClientDemo {
            public static void main(String[] args) throws IOException {
                //创建客户端的Socket对象（Socket）
                Socket s = new Socket("192.168.0.109", 50000);

                //获取输出流，写数据
                OutputStream os = s.getOutputStream();
                os.write("hello,tcp,我来了".getBytes());

                //接收服务器反馈
                InputStream is = s.getInputStream();
                byte[] bys = new byte[1024];
                int len = is.read(bys);
                String data = new String(bys,0,len);
                System.out.println("客户端："+data);

                //释放资源
                s.close();
            }
        }
        运行结果：
        客户端：数据已经收到

ServerDemo.java

        package com.ithema_09;

        import java.io.IOException;
        import java.io.InputStream;
        import java.io.OutputStream;
        import java.net.ServerSocket;
        import java.net.Socket;

        public class ServerDemo {
            public static void main(String[] args) throws IOException {
                //创建服务器的Socket对象（ServerSocket）
                ServerSocket ss = new ServerSocket(50000);

                //监听客户端连接，返回一个Socket对象
                Socket s = ss.accept();

                //获取输入流，读数据，并把数据显示在控制台
                InputStream is = s.getInputStream();
                byte[] bys = new byte[1024];
                int len = is.read(bys);
                String data = new String(bys,0,len);
                System.out.println("服务器："+data);

                //给出反馈
                OutputStream os = s.getOutputStream();
                os.write("数据已经收到".getBytes());

                //释放资源
                ss.close();
            }
        }
        运行结果：
        服务器：hello,tcp,我来了

练习2
* 客户端：数据来自于键盘录入，直到输入的数据是886，发送数据结束
* 服务器：接收到的数据在控制台输出

Client.java

        package com.ithema_10;

        import java.io.*;
        import java.net.Socket;

        public class ClientDemo {
            public static void main(String[] args) throws IOException {
                //创建客户端Socket对象
                Socket s = new Socket("192.168.0.109", 50000);

                //数据来自于键盘录入，直到输入的数据是886，发送数据结束
                BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
                //用BufferWriter封装输出流对象s.getOutputStream()
                //s.getOutputStream读取的是字符流，这句话本质上是把“字节流” 转成了 “字符流”
                //同时采用BufferedWriter和BufferedReader对应起来，这样读写都是“字符流”
                BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));
                String line;
                while((line=br.readLine())!=null) {
                    if("886".equals(line)) {
                        break;
                    }
                    //获取输出流对象
                    bw.write(line);
                    bw.newLine();  //换一行
                    bw.flush();   //再刷新一下
                }

                //释放资源
                s.close();

            }
        }
        运行结果：
        tcp
        java
        886
        
ServerDemo.java

        package com.ithema_10;

        import java.io.BufferedReader;
        import java.io.IOException;
        import java.io.InputStreamReader;
        import java.net.ServerSocket;
        import java.net.Socket;

        public class ServerDemo {
            public static void main(String[] args) throws IOException {
                //创建服务器Socket对象
                ServerSocket ss = new ServerSocket(50000);

                //监听客户端的连接，返回一个对应的Socket对象
                Socket s = ss.accept();

                //获取输入流
                //同样采用BufferedReader，把“字符流”转成“字节流”
                BufferedReader br = new BufferedReader(new InputStreamReader(s.getInputStream()));
                String line;
                while ((line = br.readLine()) != null) {
                    System.out.println(line);
                }

                //释放资源
                ss.close();
            }
        }
        运行结果：
        tcp
        java
        
练习3
* 客户端：数据来自于键盘录入，直到输入的数据是886，发送数据结束
* 服务端：接收到的数据写入文本文件

ClientDemo.java

        package com.ithema_11;

        import java.io.*;
        import java.net.Socket;

        public class ClientDemo {
            public static void main(String[] args) throws IOException {
                //创建客户端Socket对象
                Socket s = new Socket("192.168.0.109", 50000);

                //数据来自于键盘录入，直到输入的数据是886，发送数据结束
                BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
                //用BufferWriter封装输出流对象s.getOutputStream()
                //s.getOutputStream读取的是字符流，这句话本质上是把“字节流” 转成了 “字符流”
                //同时采用BufferedWriter和BufferedReader对应起来，这样读写都是“字符流”
                BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));
                String line;
                while((line=br.readLine())!=null) {
                    if("886".equals(line)) {
                        break;
                    }
                    //获取输出流对象
                    bw.write(line);
                    bw.newLine();  //换一行
                    bw.flush();   //再刷新一下
                }

                //释放资源
                s.close();

            }
        }
        运行结果：
        tcp
        java
        886
        
ServerDemo.java

        package com.ithema_11;

        import java.io.*;
        import java.net.ServerSocket;
        import java.net.Socket;

        public class ServerDemo {
            public static void main(String[] args) throws IOException {
                //创建服务器Socket对象
                ServerSocket ss = new ServerSocket(50000);

                //监听客户端连接，返回一个对应的Socket对象
                Socket s = ss.accept();

                //接收数据
                BufferedReader br = new BufferedReader(new InputStreamReader(s.getInputStream()));
                //把数据写入文本文件
                BufferedWriter bw = new BufferedWriter(new FileWriter("./s.txt"));

                String line;
                while((line = br.readLine()) != null) {
                    bw.write(line);
                    bw.newLine();
                    bw.flush();
                }

                //释放资源
                bw.close();
                ss.close();
            }
        }
        运行结果：
        本目录下出现了s.txt, 里面的内容就是tcp java

练习4
* 客户端：数据来自于文本文件
* 服务器：接收到的数据写入文本文件

ClientDemo.java

        package com.ithema_12;

        import java.io.*;
        import java.net.Socket;

        public class ClientDemo {
            public static void main(String[] args) throws IOException {
                //创建客户端Socket对象
                Socket s = new Socket("192.168.0.109", 50000);

                //封装文本文件的数据
                BufferedReader br = new BufferedReader(new FileReader("./s.txt"));
                //封装输出流写数据
                BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));

                String line;
                while ((line=br.readLine()) != null) {
                    bw.write(line);
                    bw.newLine();
                    bw.flush();
                }

                //释放资源
                br.close();
                s.close();
            }
        }
        
ServerDemo.java

        package com.ithema_12;

        import java.io.*;
        import java.net.ServerSocket;
        import java.net.Socket;

        public class ServerDemo {
            public static void main(String[] args) throws IOException {
                //创建服务器Socket对象
                ServerSocket ss = new ServerSocket(50000);

                //监听客户端连接，返回一个对应的Socket对象
                Socket s = ss.accept();

                //接收数据
                BufferedReader br = new BufferedReader(new InputStreamReader(s.getInputStream()));
                //把数据写入文本文件
                BufferedWriter bw = new BufferedWriter(new FileWriter("./s_copy.txt"));

                String line;
                while((line = br.readLine()) != null) {
                    bw.write(line);
                    bw.newLine();
                    bw.flush();
                }

                //释放资源
                bw.close();
                ss.close();
            }
        }
        运行结果：
        本地目录生成了一个s_copy.txt文件，内容跟客户端读取的s.txt文件内容一致

练习5
* 客户端：数据来自于文本文件，接收服务器反馈
* 服务器：接收到的数据写入文本文件，给出反馈
* 出现问题：程序一直等待
* 原因：读取数据的方法是阻塞式的，所以服务器不知道客户端什么时候读完了文件
* 解决办法：使用shutdownOutput()方法

ClientDemo.java

        package com.ithema_13;

        import java.io.*;
        import java.net.Socket;

        public class ClientDemo {
            public static void main(String[] args) throws IOException {
                //创建客户端Socket对象
                Socket s = new Socket("192.168.0.109", 50056);

                //封装文本文件的数据
                BufferedReader br = new BufferedReader(new FileReader("./s.txt"));
                //封装输出流写数据
                BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));

                String line;
                while ((line=br.readLine()) != null) {
                    bw.write(line);
                    bw.newLine();
                    bw.flush();
                }


                //采用Socket固有方法(表示输出结束)，告诉服务器文件已经读取完毕
                s.shutdownOutput();
                //接收反馈
                BufferedReader brClient = new BufferedReader(new InputStreamReader(s.getInputStream()));
                String data = brClient.readLine();  //等待读取服务器的数据
                System.out.println("服务器反馈："+data);

                //释放资源
                br.close();
                s.close();
            }
        }
        运行结果：
        服务器反馈：文件上传成功

ServerDemo.java

        package com.ithema_13;

        import java.io.*;
        import java.net.ServerSocket;
        import java.net.Socket;

        public class ServerDemo {
            public static void main(String[] args) throws IOException {
                //创建服务器Socket对象
                ServerSocket ss = new ServerSocket(50056);

                //监听客户端连接，返回一个对应的Socket对象
                Socket s = ss.accept();

                //接收数据
                BufferedReader br = new BufferedReader(new InputStreamReader(s.getInputStream()));
                //把数据写入文本文件
                BufferedWriter bw = new BufferedWriter(new FileWriter("./s_copy.txt"));

                String line;
                while((line=br.readLine()) != null) {
                    bw.write(line);
                    bw.newLine();
                    bw.flush();
                }

                //给出反馈
                BufferedWriter bwServer = new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));
                bwServer.write("文件上传成功");
                bwServer.newLine();
                bwServer.flush();
            }
        }
        运行结果：
        本地目录下出现了s_copy.txt文件，内容跟客户端s.txt一样的

练习6：
* 客户端：数据来自于文本文件，接收服务器反馈
* 服务端：接收到的数据写入文本文件，给出反馈，代码用线程进行封装，为每一个客户端开启一个线程

ClientDemo.java

        package com.ithema_14;

        import java.io.*;
        import java.net.Socket;

        public class ClientDemo {
            public static void main(String[] args) throws IOException {
                //创建客户端Socket对象
                Socket s = new Socket("192.168.0.109", 40056);

                //封装文本文件的数据
                BufferedReader br = new BufferedReader(new FileReader("./s.txt"));
                //封装输出流写数据
                BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));

                String line;
                while ((line=br.readLine()) != null) {
                    bw.write(line);
                    bw.newLine();
                    bw.flush();
                }


                //采用Socket固有方法(表示输出结束)，告诉服务器文件已经读取完毕
                s.shutdownOutput();
                //接收反馈
                BufferedReader brClient = new BufferedReader(new InputStreamReader(s.getInputStream()));
                String data = brClient.readLine();  //等待读取服务器的数据
                System.out.println("服务器反馈："+data);

                //释放资源
                br.close();
                s.close();
            }
        }
        运行结果：
        运行一次客户端，就多一个s_copy[count].txt文件
        
ServerDemo.java

        package com.ithema_14;

        import java.io.IOException;
        import java.net.ServerSocket;
        import java.net.Socket;

        public class ServerDemo {
            public static void main(String[] args) throws IOException {
                //创建服务器Socket对象
                ServerSocket ss = new ServerSocket(40056);

                while(true) {
                    //监听客户端连接，返回一个对应的Socket对象
                    Socket s = ss.accept();
                    //为每一个客户端开启一个线程
                    new Thread(new ServerThread(s)).start();
                }

                //服务器是不关闭的，所以没有close的操作
        //        ss.close();
            }
        }

ServerThread.java

        package com.ithema_14;

        import java.io.*;
        import java.net.Socket;

        public class ServerThread implements Runnable {
            private Socket s;
            public ServerThread(Socket s) {
                this.s = s;
            }

            @Override
            public void run() {
                try {
                    BufferedReader br = new BufferedReader(new InputStreamReader(s.getInputStream()));
                    //BufferedWriter bw = new BufferedWriter(new FileWriter("./s_copy.txt"));
                    //解决命名冲突
                    int count = 0;
                    File file = new File("s_copy[" + count + "].txt");
                    while (file.exists()) {
                        count++;
                        file = new File("s_copy[" + count + "].txt");
                    }
                    BufferedWriter bw = new BufferedWriter(new FileWriter(file));

                    String line;
                    while ((line=br.readLine())!=null) {
                        bw.write(line);
                        bw.newLine();
                        bw.flush();
                    }

                    //给出反馈
                    BufferedWriter bwServer = new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));
                    bwServer.write("文件上传成功");
                    bwServer.newLine();
                    bwServer.flush();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
        }

## Lambda表达式
### 体验Lambda表达式
需求：启动一个线程，在控制台输出一句话：多线程程序启动了

方式1：
* 定义一个类MyRunnable实现Runnable接口，重写run()方法
* 创建MyRunnable类的对象
* 创建Thread类的对象，把MyRunnable的对象作为构造参数传递
* 启动线程

方式2
* 匿名内部类的方式改进

方式3
* Lambda表达式的方式改进

LambdaDemo.java

        package com.ithema_15;

        public class LambdaDemo {
            public static void main(String[] args) {
                //方法1：实现类的方式实现需求
                MyRunnable my = new MyRunnable();
                Thread t = new Thread(my);
                t.start();
        
                //方法2：匿名内部类的方式改进
                new Thread(new Runnable() {
                    @Override
                    public void run() {
                        System.out.println("多线程程序启动了");
                    }
                }).start();

                //方法3：Lambda 表达式的方式改进
                new Thread( () -> {
                    System.out.println("多线程程序启动了");
                }).start();
            }
        }
        运行结果：
        多线程程序启动了
        多线程程序启动了
        多线程程序启动了

MyRunnable.java

        package com.ithema_15;

        public class MyRunnable implements Runnable{
            @Override
            public void run() {
                System.out.println("多线程程序启动了");
            }
        }

### Lambda表达式的标准格式
组成Lambda表达式的三要素：形式参数，箭头，代码块

Lambda表达式的格式
* 格式：（形式参数）->(代码块)
* 形式参数：如果有多个参数，参数之间用逗号隔开；如果没有参数，留空即可
* ->: 由英文中画线和大于符号组成，固定写法。代表指向动作
* 代码块：是我们具体要做的事情，也就是以前我们写的方法体内容

### Lambda表达式练习
Lambda表达式的使用前提
* 有一个接口
* 接口中有且仅有一个抽象方法

练习1（抽象方法无参无返回）：
* 定义一个接口（Eatable）,里面定义一个抽象方法：void eat()
* 定义一个测试类（EatableDemo）,在测试类中提供两个方法

&emsp;&emsp一个方法是useEatable(Eatable e)

&emsp;&emsp一个方法是main方法，在main方法中调用useEatable方法

Eatable.java

        package com.ithema_16;

        public interface Eatable {
            void eat();
        }
        
EatableImpl.java

        package com.ithema_16;

        public class EatableImpl implements Eatable{
            @Override
            public void eat() {
                System.out.println("一天一苹果，医生远离我");
            }
        }
        
EatableDemo.java

        package com.ithema_16;

        public class EatableDemo {
            public static void main(String[] args) {
                //方法1：在主方法中调用useEatable方法
                Eatable e = new EatableImpl();
                useEatable(e);

                //方法2：匿名内部类
                useEatable(new Eatable() {
                    @Override
                    public void eat() {
                        System.out.println("一天一苹果，医生远离我");
                    }
                });

                //方法3：Lambda表达式
                useEatable(() -> {
                    System.out.println("一天一苹果，医生远离我");
                });
            }

            public static void useEatable(Eatable e) {
                e.eat();
            }
        }
        运行结果：
        一天一苹果，医生远离我
        一天一苹果，医生远离我
        一天一苹果，医生远离我

练习2（抽象方法带参无返回）：
* 定义一个接口（Flyable），里面定义一个抽象方法：void fly(String s);
* 定义一个测试类（FlyableDemo），在测试类中提供两个方法
* 与上一个练习的区别是，此抽象方法里面带有形参，所以Lambda表达式里面，也会有形参输入；

&emsp;&emsp;一个方法是：useFlyable(Flyable f)

&emsp;&emsp;一个是main方法，在main方法中调用useFlyable方法

Flyable.java

        package com.ithema_17;

        public interface Flyable {
            void fly(String s);
        }

FlyableDemo.java

        package com.ithema_17;

        public class FlyableDemo {
            public static void main(String[] args) {
                //在main方法中调用useFlyable方法
                //方法1：匿名内部类
                useFlyable(new Flyable() {
                    @Override
                    public void fly(String s) {
                        System.out.println(s);
                    }
                });

                //方法2：Lambda表达式
                useFlyable((String s) -> {
                    System.out.println(s);
                });
            }

            public static void useFlyable(Flyable f) {
                f.fly("风和日丽");
            }
        }
        运行结果：
        风和日丽
        风和日丽

练习3（抽象方法带参带返回值）：
* 定义一个接口（Addable），里面定义一个抽象方法：int add(int x, int y);
* 定义一个测试类（AddableDemo），在测试类中提供两个方法

&emsp;&emsp;一个方法是：useAddable(Addable a)

&emsp;&emsp;一个方法是main方法，在main方法中调用useAddable方法

Addable.java

        package com.ithema_18;

        public interface Addable {
            int add(int x, int y);
        }
        
AddableDemo.java

        package com.ithema_18;

        public class AddableDemo {
            public static void main(String[] args) {
                useAddable((int x, int y) -> {
                    return x + y;
                });
            }

            public static void useAddable(Addable a) {
                int sum = a.add(10, 20);
                System.out.println(sum);
            }
        }
        运行结果：
        30

### Lambda表达式的省略模式
省略规则：
* 参数类型可以省略。但是有多个参数的情况下，不能只省略一个
* 如果参数有且仅有一个，那么小括号可以省略
* 如果代码块的语句只有一条，可以省略大括号和分号，甚至是return

Addable.java

        package com.itheima_19;

        public interface Addable {
            int add (int x, int y);
        }
        
Flyable.java

        package com.itheima_19;

        public interface Flyable {
            void fly(String s);
        }
        
LambdaDemo.java

        package com.itheima_19;

        public class LambdaDemo {
            public static void main(String[] args) {
                //参数类型可以省略
                //如果代码块的语句只有一条，可以省略大括号和分号，如果有return, return也要省略
                useAddable(((x, y) -> x + y));

                //如果代码块的语句只有一条，可以省略大括号和分号
                useFlyable(s -> System.out.println(s));
            }

            private static void useFlyable(Flyable f) {
                f.fly("风和日丽");
            }

            private static void useAddable(Addable a) {
                int sum = a.add(10, 20);
                System.out.println(sum);
            }
        }
        运行结果：
        30
        风和日丽

### Lambda表达式的注意事项
注意事项：
* 使用Lambda必须要有接口，并且要求接口中有且仅有一个抽象方法
* 必须有上下文环境，编译器才能推导出Lambda对应的接口

&emsp;&emsp;根据**局部变量的赋值**得知Lambda对应的接口：Runnable r = () -> System.out.println("Lambda表达式");

&emsp;&emsp;根据**调用方法的参数**得知Lambda对应的接口：new Thread(() -> System.out.println("Lambda表达式")).start();

### Lambda表达式和匿名内部类的区别
所实现的类型不同：
* 匿名内部类：可以是接口、抽象类和具体类
* Lambda表达式：只能是接口

使用限制不同
* 如果接口中有且仅有一个抽象方法，可以使用Lambda表达式，也可以使用匿名内部类
* 如果接口中多于一个抽象方法，只能使用匿名内部类，而不能使用Lambda表达式

实现原理不同
* 匿名内部类：编译之后，产生一个单独的 .class 字节码文件
* Lambda表达式：编译之后，没有一个单独的 .class 字节码文件。对应的字节码会在运行的时候动态生成

Inter.java

        package com.itheima_20;

        public interface Inter {
            void show();
        }
        
Abstract.java

        package com.itheima_20;

        public abstract class Abstract {
            public abstract void cx();
        }
        
Specific.java

        package com.itheima_20;

        public class Specific {
            public void specific() {
                System.out.println("事业有成步步高升");
            }
        }
        
LambdaDemo.java

        package com.itheima_20;

        public class LambdaDemo {
            public static void main(String[] args) {
                // 匿名内部类实现接口、抽象类、具体类
                useInter(new Inter() {
                    @Override
                    public void show() {
                        System.out.println("接口");
                    }
                });

                useAbstract(new Abstract() {
                    @Override
                    public void cx() {
                        System.out.println("抽象类");
                    }
                });

                useSpecific(new Specific(){
                    @Override
                    public void specific() {
                        System.out.println("具体类");
                    }
                });

                //Lambda表达式
                useInter(()-> System.out.println("接口"));
            }

            public static void useInter(Inter i) {
                i.show();
            }

            public static void useAbstract(Abstract a) {
                a.cx();
            }

            public static void useSpecific(Specific s) {
                s.specific();
            }
        }
        运行结果：
        接口
        抽象类
        具体类
        接口

## 接口组成（更新）
### 接口中默认方法
从之前的课程中，我们学到了，一个接口是由不同的抽象方法组成的，同时每个接口还需配备一个实现类来实现该接口中的抽象方法，但是这样会引出一个问题。
* 问题：当接口中新增一个抽象方法时，所有的接口实现类都需要重写该方法，在实际业务中即麻烦也没必要
* 原因：接口中定义的抽象方法，在接口实现类中必须被强制重写，漏写一个都会报错
* 解决方法：Java8 之后，在接口中允许使用 “默认方法” 来避免大量重写的问题，因为 “默认方法” 相当于是一个具体方法，是可以不被强制重写的，并且一旦定义好后，任意接口实现类都能调用该方法（当然也可以重写它）。

接口中默认方法的定义格式：
* 格式：public default 返回值类型 方法名（参数列表） { }
* 范式：public default void show3() { }

接口中默认方法注意事项：
* 默认方法不是抽象方法，所以不能强制被重写。但是要是想重写它也是可以的，重写的时候在接口实现类中去掉 default 关键字
* public 可以省略，default 不能省略

MyInterface.java

        package com.itheima_21;

        public interface MyInterface {
            //两个抽象方法
            void show1();
            void show2();

            //默认方法
            public default void show3() {
                System.out.println("show3");
            }
        }

MyInterfaceImplOne.java

        package com.itheima_21;

        public class MyInterfaceImplOne implements MyInterface{
            @Override
            public void show1() {
                System.out.println("One show1");
            }

            @Override
            public void show2() {
                System.out.println("One show2");
            }

            //这个接口实现类没有重写 默认方法 show3 也不会报错
        }

MyInterfaceImplTwo.java

        package com.itheima_21;

        public class MyInterfaceImplTwo implements MyInterface{
            @Override
            public void show1() {
                System.out.println("Two show1");
            }

            @Override
            public void show2() {
                System.out.println("Two show2");
            }

            // 重写 默认方法 show3

            @Override
            public void show3() {
                System.out.println("Two show3");
            }
        }

MyInterfaceDemo.java

        package com.itheima_21;

        public class MyInterfaceDemo {
            public static void main(String[] args) {
                // 按照多态的方式创建对象并使用
                MyInterface my_1 = new MyInterfaceImplOne();
                my_1.show1();
                my_1.show2();
                my_1.show3();

                System.out.println("===================");

                MyInterface my_2 = new MyInterfaceImplTwo();
                my_2.show1();
                my_2.show2();
                my_2.show3();
            }
        }
        
        运行结果：
        One show1
        One show2
        show3
        ===================
        Two show1
        Two show2
        Two show3

### 接口中静态方法
接口静态方法的定义格式：
* 格式：public static 返回值类型 方法名（参数列表）{ }  不难发现，和一个类中的静态方法是一样的
* 范例：public static void show() { }

接口中静态方法的注意事项：
* 静态方法只能通过接口名调用，不能通过实现类名或者对象调用
* public 可以省略，static 不能省略

Inter.java

        package com.itheima_22;

        public interface Inter {
            void show();

            default void method() {
                System.out.println("默认方法");
            }

            public static void test() {
                System.out.println("静态方法");
            }
        }

InterImpl.java

        package com.itheima_22;

        public class InterImpl implements Inter{
            @Override
            public void show() {
                System.out.println("抽象方法");
            }
        }

InterDemo.java

        package com.itheima_22;
        /*
            需求：
                1. 定义一个接口Inter，里面有三个方法：一个是抽象方法，一个是默认方法，一个是静态方法
                    void show();
                    default void method() {}
                    public static void test() {}
                2. 定义接口的一个实现类
                    InterImpl
                3. 定义测试类
                    InterDemo
                    在main方法中，按照多态的方式创建对象并使用
         */
        public class InterDemo {
            public static void main(String[] args) {
                //按照多态的方式创建对象并使用
                Inter i = new InterImpl();
                i.show();
                i.method();
        //        i.test(); 报错，无法调用

                Inter.test();  // 静态方法，只能通过接口名调用
            }
        }
        运行结果：
        抽象方法
        默认方法
        静态方法

### 接口中私有方法
Java 9 中新增了帶方法体的私有方法，这其实在Java 8 中就埋下了伏笔：Java 8允许在接口中定义带方法体的默认方法和静态方法。这样可能会引发一个问题：当两个默认方法或者静态方法中包含一段相同的代码实现时，程序必然考虑将这段代码抽取成一个共性方法，而这个共性方法是不需要别人使用的，因此用私有给隐藏起来，这就是Java 9增加私有方法的必然性。

接口中私有方法的定义格式：
* 格式1：private 返回值类型 方法名（参数列表）{ }
* 范例1：private void show(){ }
* 格式2：private static 返回值类型 方法名（参数列表）{ }
* 范例2：private static void method(){ }

不难发现，在接口中定义私有方法 和 在一个类中定义私有方法格式是一样的。

接口中私有方法的**注意事项：**
* 默认方法可以调用私有的静态方法和非静态方法
* 静态方法只能调用私有的静态方法
* 所以，虽然私有方法可以有静态和非静态两种写法，不如直接统一写成静态方法，这样默认方法和静态方法都能调用私有方法了

Inter.java

        package com.itheima_23;

        public interface Inter {
            default void show1() {
                System.out.println("show1开始");
                show();
                method();
                System.out.println("show1结束");
            }

            default void show2() {
                System.out.println("show2开始");
                show();
                method();
                System.out.println("show2结束");
            }

            // 私有方法
            private void show() {
                System.out.println("1111111111");
                System.out.println("2222222222");
            }

            static void method1() {
                System.out.println("method1开始");
                method();
                System.out.println("method2结束");
            }

            static void method2() {
                System.out.println("method2开始");
                method();
                System.out.println("method2结束");
            }

            // 私有静态方法
            private static void method() {
                System.out.println("1111111111");
                System.out.println("2222222222");
            }
        }

InterImpl.java

        package com.itheima_23;

        public class InterImpl implements Inter{
            // 因为接口中没有定义具体方法，所以此接口实现类，什么都不用做
        }

InterDemo.java

        package com.itheima_23;
        /*
            需求：
                1.定义一个接口Inter，里面有四个方法：两个默认方法，两个静态方法
                    default void show1(){ }
                    default void show2(){ }
                    static void method1(){ }
                    static void method2(){ }
                2.定义接口的一个实现类
                    InterImpl
                3.定义测试类
                    InterDemo
                    在main方法中，按照多态的方式创建对象并使用
         */
        public class InterDemo {
            public static void main(String[] args) {
                Inter i = new InterImpl();
                i.show1();
                System.out.println("------------");
                i.show2();
                System.out.println("------------");
                Inter.method1();
                System.out.println("------------");
                Inter.method2();
            }
        }
        
        运行结果：
        show1开始
        1111111111
        2222222222
        1111111111
        2222222222
        show1结束
        ------------
        show2开始
        1111111111
        2222222222
        1111111111
        2222222222
        show2结束
        ------------
        method1开始
        1111111111
        2222222222
        method2结束
        ------------
        method2开始
        1111111111
        2222222222
        method2结束

## 方法引用
### 体验方法引用
在使用Lambda表达式的时候，我们实际上传递进去的代码就是一种解决方案：拿参数做操作

那么考虑一种情况：如果我们在Lambda中所指定的操作方案，已经有地方存在相同的方案，那是否有必要再写重复逻辑呢？

答案肯定是没有必要

那我们如何使用已经存在的方案呢？ 

需要借助方法引用来使用已经存在的方法

Printable.java

        package com.itheima_24;

        public interface Printable {
            void printString(String s);
        }

PrintableDemo.java

        package com.itheima_24;
        /*
            需求：
                1. 定义一个接口（Printable）：里面定义一个抽象方法：void printString(String s);
                2. 定义一个测试类（PrintableDemo），在测试类中提供两种方法
                    一个方法是：usePrintalbe(Printable p)
                    一个方法是main方法，在main方法中调用usePrintable方法
         */
        public class PrintableDemo {
            public static void main(String[] args) {
                // Lambda 表达式
                usePrintable(s -> System.out.println(s));

                // 方法引用符 ::
                usePrintable(System.out::println);

                // 可推导就是可省略的
                // 在方法引用中，s参数传递进去了，只不过看不见
            }

            public static void usePrintable(Printable p) {
                p.printString("爱生活");
            }
        }
        
        运行结果：
        爱生活
        爱生活

### 方法引用符
方法引用符
* ：： 该符号为引用运算符，而它所在的表达式被称为方法引用

回顾一下我们在体验方法引用中的代码
* Lambda 表达式：usePrintables(s -> Sysyem.out.println(s));

&emsp;&emsp;分析：拿到参数s之后通过Lambda 表达式，传递给 Sysyem.out.println 方法去处理

* 方法引用：usePrintable(System.out::println);

&emsp;&emsp;分析：直接使用Sysyem.out 中的 println 方法来取代Lambda, 代码更加简洁

推导与省略
* 如果使用 Lambda，那么根据“可推导就是可省略”的原则，无需指定参数类型，也无需指定的重载形式，它们都将自动推导
* 如果使用方法引用，也是同样可以根据上下文进行推导
* 方法引用是Lambda的孪生兄弟
