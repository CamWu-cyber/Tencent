## 指针
#### 指针的定义
    int a = 10;
    int * p = &a;

#### 指针的使用（查看指针所指向内存中存放的值）
    //指针变量p前面加 * 的操作叫做 “解引用”
    cout << "*p = " << *p << endl;

#### 指针所占内存空间
32位操作系统中不管什么类型的指针变量，所占内存空间都是4个字节，一般编译器采用的都是32位；
64位操作系统中不管什么类型的指针变量，所占内存空间都是8个字节；
    
    int a = 10;
    int *p = &a;
    cout << sizeof(p) << endl;
    cout << sizeof(char *) << endl;
    cout << sizeof(float *) << endl;
    
运行结果

    4
    4
    4

#### 空指针
空指针：指针变量指向内存中编号为0的空间；

用途：初始化指针变量。一开始不知道指针应该指向哪块内存，就让它指向0空间；

注意：空指针指向的内容是不可以访问的，因为那是系统占用的内存。

    //定义空指针
    int *p = NULL;
    //访问空指针报错
    //内存0~255为系统占用内存，不允许用户访问
    cout << *p << endl;
    //改变空指针的值，报错
    *p = 100;

#### 野指针
野指针：指针指向非法内存空间

    //直接让指针变量p指向内存地址编号为0x1100的空间
    int *p = (int *)0x1100;
    
    //想看看野指针指向地址存放的内容，报错，因为这块空间不是你生成的你没有权限访问它
    cout << *p << endl;

**结论：空指针和野指针都不是我们申请的空间，因此不要访问。**

#### 指针、数组、函数三个碰撞在一起的使用
要求：封装一个函数，利用冒泡排序，实现对整型数组的升序排序

    #include <iostream>
    using namespace std;

    void boubleSort(int * arr, int len){
        for(int i=0;i<len-1;i++){
            for(int j=0;j<len-i-1;j++){
                if(arr[j]>arr[j+1]){
                    int temp = arr[j];
                    arr[j] = arr[j+1];
                    arr[j+1] = temp;
                }
            }
        }
    }

    void printArray(int * arr, int len){
        for(int i=0;i<len;i++){
            cout << arr[i] << endl;
        }
    }

    int main(){
        //1.创建数组
        int arr[10] = {4,3,2,6,9,10,1,7,99,100};

        //2.创建函数，实现冒泡排序
        //数组名就代表数组的首地址！！
        int len = sizeof(arr)/sizeof(arr[0]);
        boubleSort(arr, len);

        //3.打印排序后的数组
        printArray(arr, len);
        return 0;
    }
    
    运行结果：1,2,3,4,6,7,9,10,99,100



## 结构体
#### 结构体指针
1.结构体是自己定义的数据类型，里面可以包含了各种类型的成员；

2.结构体传递到函数中的时候，推荐使用**地址传递**（节省空间，一个指针才4个字节）；

3.**地址传递**的时候形参的值改变了会影响实参的值也跟着变化，如果你不想改变实参的值，就在结构体参数前面加const进行修饰；

    #include<iostream>
    using namespace std;
    #include<string>

    //学生结构体定义
    struct student
    {
        string name;
        int age;
        int score;
    };

    //1.采用地址传递的话，函数里面就要用指针来接受这个地址
    //2.地址传递有一个问题，就是如果函数内部把形参的值不小心修改了，实参的值也会相应地发生改变，所以如果不想改变实参的值，就加上const关键字
    void printStu(const student * stu)
    {
        //stu -> score = 100; //操作失败，因为加了const修饰
        cout << "name:" << stu->name << "  age:" << stu->age << "  score:" << stu->score << endl;
    }

    int main(){
        student stu;
        stu.age = 16;
        stu.name = "zhangsan";
        stu.score = 99;

        //1.函数里面调用结构体有两种方法，一种是值传递printStu(stu),另一种是地址传递printStu(&stu)
        //2.值传递会把整个结构体复制一份，如果数据量较大的时候会占用较多的空间，不推荐使用，一般采用地址传递，因为一个指针只占4个字节
        printStu(&stu);

        return 0;
    }
    
    运行结果：name:zhangsan  age:16  score:99

#### 结构体数组
作用：将自定义的结构体放入数组中方便维护

步骤：1.定义结构体；   2.创建结构体数组，理解成一个数组类型是结构体，数组的元素是一个个结构体对象。

    #include <iostream>
    using namespace std;

    //结构体数组
    //1、定义结构体
    struct Student
    {
        string name;
        int age;
        int score;
    };

    void printArray(Student * stuArray, int len)
    {
        for(int i=0;i<len;i++)
        {
            cout << "name: " << stuArray[i].name
                 << "  age: " << stuArray[i].age
                 << "  score: " << stuArray[i].score << endl;
        }
    }

    int main(){
        //2、创建结构体数组
        struct Student stuArray[3] = 
        {
            {"Alick",8,87},
            {"Bob",9,89},
            {"Cathy",10,90}
        };

        //3、给结构体数组中的元素赋值
        stuArray[2].name = "Mark";
        stuArray[2].age = 13;
        stuArray[2].score = 91;

        //4、遍历结构体数组
        int len = sizeof(stuArray)/sizeof(stuArray[0]);
        printArray(&stuArray[0], len);  //地址传递
    }
    
    运行结果：
    name: Alick  age: 8  score: 87
    name: Bob  age: 9  score: 89
    name: Mark  age: 13  score: 91

## 代码区、全局区、栈区、堆区
1. 这四个区是c++程序运行的整个过程中，内存会被划分成的四个区域。
2. 代码区和全局区是编译时产生的，栈区和堆区是程序运行后才产生的。
3. 代码区存放的是二进制的代码。
4. 全局区存放的是全局变量（不在函数内部的变量）、静态变量（statistic修饰的）、字符串常量（比如：“hello world”）、全局常量（const修饰的全局变量（全局变量见开头））。
5. 栈区存放函数的参数值，局部变量等，由编译器自动释放，所以函数运行完后函数的参数和里面的变量就不在内存中了。
6. 堆区由程序员分配释放（delete），若程序员不释放，程序结束时由操作系统回收。
7. c++中通过new关键字在堆区开辟内存。下面见详细代码。

        #include<iostream>
        using namespace std;

        //1.利用new关键字，在堆区创建数据
        //2.创建好数据后，它是把堆区的地址返回给你，所以需要用指针接收
        int * func()
        {
            int* p = new int(10);
            return p;
        }

        int main() {
            int* p = func();
            cout << *p << endl;

            system("pause");
            return 0;
        }
        
        运行结果：10
        
        //释放堆区数据，用delete
	    delete p;

## 引用
#### 引用的基本使用
1. 作用：给变量起别名。
2. 引用必须初始化。
3. 引用在初始化后，不可以改变。
	
		//此时b就是a的别名，改变b的值就相当于改变了a的值
		int a = 10;
		int &b = a;
	 
#### 引用作函数参数
1. 作用：函数传参时，可以利用引用技术让形参修饰实参，也就是说形参的值发生了改变相当于实参的值也发生了改变。
2. 优点：简化指针修改实参的过程。因为地址传递也能做到形参修改实参的作用，但是指针操作比较麻烦，通过引用来简化这个过程，效果是一样的。
3. 引用作为函数参数的时候，就是形参多了寻址符&，其他的跟正常变量一样！

		#include<iostream>
		using namespace std;

		//1.值传递，无法通过形参修改实参
		void mySwap01(int a, int b) {
			int temp = a;
			a = b;
			b = temp;
		}

		//2.地址传递，可以通过形参修改实参，但是比较麻烦
		void mySwap02(int * a, int * b) {
			int temp = *a;
			*a = *b;
			*b = temp;
		}

		//3.引用传递，可以通过形参修改实参，比地址传递更简洁
		//可以看到，当引用作为函数参数的时候，就是形参多了寻址符&，其他的跟正常变量一样
		void mySwap03(int &a, int &b) {
			int temp = a;
			a = b;
			b = temp;
		}
		
		//3.1有的时候需要实参不被修改，那么在形参的前面加const就行。这个叫**常量引用**
		void mySwap03(const int &val) {
			val = 1000;  // 报错，val不允许修改
		}

		int main() {
			int a = 10;
			int b = 20;

			mySwap01(a, b);
			cout << "a:" << a << "  b:" << b << endl;

			mySwap02(&a, &b);
			cout << "a:" << a << "  b:" << b << endl;

			mySwap03(a, b);
			cout << "a:" << a << "  b:" << b << endl;


			system("pause");
			return 0;
		}
		
		运行结果：a:10  b:20
			  a:20  b:10 //地址传递交换后，引用传递又交换回来了
			  a:10  b:20
		
		
		

#### 引用作为函数返回值（感觉不太常用）
1. 注意：引用作为函数返回值的时候不可以返回局部变量的引用，只能返回静态局部变量的引用。
2. 把引用作为返回值的函数，也能当做左值使用。

		#include<iostream>
		using namespace std;

		// 引用作为函数返回值的时候，只要在函数定义的时候加&即可，return对应的变量即可。

		//函数返回局部变量的引用
		int& test01() {
			int a = 10; //局部变量
			return a;
		}

		//函数返回静态变量的引用
		int& test02() {
			static int a = 20; //
			return a;
		}

		int main() {
			//不能返回局部变量的引用
			int& ref = test01();
			cout << "ref=  " << ref << endl; // 第一次结果正确，因为编译器做了保留
			cout << "ref=  " << ref << endl; // 第二次结果错误，因为a的内存已经释放

			//返回静态局部变量是可以的
			int& ref2 = test02();
			cout << "ref2=  " << ref2 << endl;
			cout << "ref2=  " << ref2 << endl;

			//如果函数作左值，那么必须返回引用
			test02() = 1000;  // 相当于a=1000，因为test02()返回的是a的引用
			int& ref3 = test02();
			cout << "ref3=  " << ref3 << endl;

			system("pause");
			return 0;
		}

		运行结果：
		ref=  10
		ref=  2080151944
		ref2=  20
		ref2=  20
		ref3=  1000

## 函数高级用法
#### 函数默认参数
test(int a, int b=10){}
#### 函数占位参数
test(int a, int){}
#### 函数重载
作用：函数名称相同，提高复用性（个人感觉你在实际开发中用函数重载会被人打死，慎用！！）
重载满足条件：
* 同一作用域下。
* 函数名称相同。
* 函数参数**类型不同** 或者 **个数不同** 或者 **顺序不同**。

注意：函数的返回值不可以作为函数重载的实现条件

## 类和对象-封装
#### 访问权限
* 公共权限 public     类内可以访问 类外可以访问
* 保护权限 protected  类内可以访问 类外不可以访问 儿子可以访问父亲的保护内容
* 私有权限 private    类内可以访问 类外不可以访问 儿子不可以访问父亲的私有内容

#### 成员属性私有化
**优点1：** 将所有成员属性设置为private，再在public里面写读写的函数，这样就能自己控制读写的权限，避免了直接访问属性。
**优点2：** 对于读写函数，里面可以设置条件来检测数据的有效性。
		#include<iostream>
		using namespace std;

		class Person {
		private:
			string m_Name;

		public:
			// 读函数
			string getName() {
				return m_Name;
			}
			// 写函数
			void setName(string name) {
				m_Name = name;
			}
		};

		int main() {
			Person p;
			p.m_Name // 报错，无法访问私有属性
			
			// 使用读写函数访问私有属性
			string name = "李四";
			p.setName(name);
			cout << p.getName() << endl;
		}

		运行结果：李四

	
## 类和对象-对象特性-构造函数和析构函数

* 构造函数：主要作用在于创建对象时为对象成员属性赋值，构造函数由编译器自动调用，无需手动调用。
* 析构函数：主要作用在于对象**销毁前**系统自动调用，执行一些清理工作。

**构造函数语法：** 类名(){}
1. 构造函数，没有返回值也不写void。
2. 函数名称与类名相同。
3. 构造函数可以有参数，因此可以发生重载。
4. 程序在调用对象时候会自动调用构造，无需手动调用，而且只会调用一次。
	
**析构函数语法：** ~类名(){}
1. 析构函数，没有返回值也不写void。
2. 函数名称与类名相同，在名称前加上符合~。
3. 析构函数不可以有参数，因此不可以发生重载。
4. 程序在对象销毁前会自动调用析构函数，无须手动调用，而且只会调用一次。

#### 构造函数析构函数的定义与调用
深拷贝是面试经典问题，也是常见的一个坑。
	
浅拷贝：简单的赋值拷贝操作。
	
深拷贝：在堆区重新申请空间，进行拷贝操作。
