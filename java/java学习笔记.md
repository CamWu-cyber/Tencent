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
