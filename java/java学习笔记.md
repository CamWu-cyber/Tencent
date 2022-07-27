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

# 修饰符
## 包
### 包的概述和使用
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
