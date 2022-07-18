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
