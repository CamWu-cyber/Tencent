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
