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
