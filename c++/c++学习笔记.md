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

	
## 对象的初始化和清理-构造函数和析构函数

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
	
	#include<iostream>
	using namespace std;

	class Person {
	public:
		// 第一种定义：无参（默认）构造函数
		Person() {
			cout << "无参构造函数" << endl;
		}

		// 第二种定义：有参构造函数
		Person(int age, int height) {
			cout << "有惨构造函数" << endl;
			m_age = age;
			m_height = new int(height); // 在堆区开辟空间
		}

		// 本来是可以拷贝函数的不写的，但是为了解决浅拷贝带来的问题，自己实现拷贝构造函数
		// 第三种定义：拷贝构造函数
		// 所谓拷贝构造函数就是把原本定义好的构造函数对象拷贝给一个新的对象，所以参数用const+引用传递
		Person(const Person& p) {
			cout << "拷贝构造函数" << endl;
			// 如果不利用深拷贝在堆区创建新内存，会导致浅拷贝带来的重复释放同一堆区问题
			m_age = p.m_age;
			//m_height = p.m_height; // 编译器默认实现的就是这行代码，但是会报错，引起释放同一堆区的问题，所以下一行我们自己写一个深拷贝的操作
			m_height = new int(*p.m_height);  // 拷贝的时候申请另外一块空间进行拷贝操作
		}

		// 析构函数
		~Person() {
			cout << "析构函数！" << endl;
			// 将堆区开辟的数据释放掉
			if (m_height != NULL)
			{
				delete m_height;
				m_height = NULL; // 防止野指针出现，做一个置空的操作
			}
		}

	public:
		int m_age;
		int* m_height; // 身高是在堆区创建的，所以需要用指针来接受它
	};

	void test01() {
		Person p;
		Person p1(18, 160);
		cout << "p1的年龄为：" << p1.m_age << " 身高为：" << *p1.m_height << endl;
		Person p2(p1);
		cout << "p2的年龄为：" << p2.m_age << " 身高为：" << *p2.m_height << endl;
	}

	int main() {
		test01();
		system("pause");
		return 0;
	}
	运行结果：
	无参构造函数
	有参构造函数
	p1的年龄为：18 身高为：160
	拷贝构造函数
	p2的年龄为：18 身高为：160
	析构函数！
	析构函数！
	析构函数！

#### 初始化列表（感觉不常用）
作用：c++提供了初始化列表语法，用来初始化属性。
语法：构造函数():属性1(值1)，属性2（值2），属性3（值3）...{}
		
	#include<iostream>
	using namespace std;

	class Person {
	public:
		////传统初始化方式，在构造函数中初始化
		//Person(int a, int b, int c)
		//{
			//m_A = a;
			//m_B = b;
			//m_C = c;
		//}

		// 方式二：通过初始化列表方式初始化属性
		Person(int a, int b, int c) :m_A(a), m_B(b), m_C(c)
		{

		}

		int m_A;
		int m_B;
		int m_C;
	};

	void test01() {
		//Person p(10, 20, 30);
		Person p(30, 20, 10);
		cout << "m_A = " << p.m_A << endl;
		cout << "m_B = " << p.m_B << endl;
		cout << "m_C = " << p.m_C << endl;
	}

	int main() {
		test01();
		system("pause");
		return 0;
	}

	运行结果：
	m_A = 30
	m_B = 20
	m_C = 10

#### 类对象作为类成员
c++类中的成员可以是另一个类的对象，我们称该成员为*对象成员*
例如：
	class A ()
	class B 
	{
		A a;
	}
B类中有对象a作为成员，a为*对象成员*
那么当创建B对象时，A与B的构造和析构的顺序谁先谁后？
结论：当其他类对象作为本类成员，构造时候先构造类对象，再构造自身，析构的顺序与构造相反
	
	#include<iostream>
	using namespace std;
	#include<string>

	//类对象作为类成员时如何定义和调用
	//结论：当其他类对象作为本类成员，构造时候先构造类对象，再构造自身，析构的顺序与构造相反

	//手机类
	class Phone {
	public:
		// 在构造函数中对手机名称作赋值操作
		Phone(string PName)
		{
			m_PName = PName;
			cout << "手机类构造函数" << endl;
		}

		~Phone()
		{
			cout << "手机类析构函数" << endl;
		}

		//手机品牌名
		string m_PName;
	};

	//人类
	class Person {
	public:
		// 采用初始化列表的方法给成员属性赋值
		// Phone m_Phone = pName 隐式转化法
		Person(string name, string pName) : m_Name(name), m_Phone(pName)
		{
			cout << "人类构造函数" << endl;
		}

		~Person()
		{
			cout << "人类析构函数" << endl;
		}

		//姓名
		string m_Name;
		//手机
		Phone m_Phone;
	};

	void test01() 
	{
		Person p("张三","iPhoneX");
		cout << p.m_Name << "拿着：" << p.m_Phone.m_PName << endl;
	}

	int main() {
		test01();
		system("pause");
		return 0;
	}

	运行结果：手机类构造函数
		人类构造函数
		张三拿着：iPhoneX
		人类析构函数
		手机类析构函数

#### 静态成员
静态成员就是在成员变量和成员函数前加上关键字static，成为静态成员
静态成员分为：
	
* 静态成员变量
	- 所有对象共享同一份数据
	- 在编译阶段分配内存
	- 类内声明，类外初始化

* 静态成员函数
	- 所有对象共享同一个函数
	- 静态成员函数只能访问静态成员变量

demo:
	
	#include<iostream>
	using namespace std;

	class Person {
	public:

		//静态成员函数
		static void func()
		{
			m_A = 100;//静态成员函数可以访问 静态成员变量
			//m_B = 200;//静态成员函数 不可以访问 非静态成员变量，无法区分到底是哪个对象的m_B属性
			cout << "static void func调用" << endl;
		}

		static int m_A; //静态成员变量，类内声明，类外初始化
		int m_B; //非静态成员变量

		//静态成员函数也是有访问权限的
	private:
		static void func2()
		{
			cout << "static void func2调用" << endl;
		}
	};

	int Person::m_A = 0;

	//有两种访问方式
	void test01()
	{
		//1、通过对象访问静态成员函数
		Person p;
		p.func();

		//2、通过类名访问静态成员函数，因为大家都共享同一个，所以可以直接用类名访问
		Person::func();

		//Person::func2();类外访问不到私有静态成员函数
	}

	int main() {
		test01();

		system("pause");
		return 0;
	}

        运行结果：static void func调用
		 static void func调用

## C++对象模型和this指针
#### 成员变量和成员函数分开存储
在c++中，类内成员变量和成员函数分开存储。

只有非静态成员变量才属于类的对象上，静态成员变量、非静态成员函数、静态成员函数都不属于类的对象，只此一份，大家共享。
	
	#include<iostream>
	using namespace std;

	//成员变量 和 成员函数 分开存储的

	class Person {
		int m_A; //非静态成员变量 属于类的对象上 p对象大小为4

		static int m_B; //静态成员变量 不属于类的对象上  p对象大小还是为4

		void func() {} //非静态成员函数 不属于类的对象上 p对象大小还是为4

		static void func2() {} //静态成员函数 不属于类的对象上 p对象大小还是为4
	};
	int Person::m_B = 0;

	void test01()
	{
		Person p;
		//空对象占用内存空间为：1
		// C++编译器会给每个空对象也分配一个字节空间，是为了区分空对象占内存的位置，因为如果有多个空对象的时候，你不能让两个空对象占同一块内存吧
		//每个空对象也应该有一个独一无二的内存地址
		cout << "size of p = " << sizeof(p) << endl;
	}

	void test02()
	{
		Person p;
		cout << "size of p = " << sizeof(p) << endl; //不是空对象的时候，非静态成员变量属于类的对象上，int类型占4个字节
	}

	int main() {
		test01();
		test02();
		system("pause");
		return 0;
	}

	运行结果：size of p = 4
		 size of p = 4
	
#### this指针概念
通过上一节我们知道在c++成员变量和成员函数是分开存储的
	
每一个非静态成员函数只会诞生一份函数实例，也就是说多个同类型的对象会共用一块代码
	
那么问题是：这一块代码是如何区分哪个对象调用自己的呢？

&nbsp;

c++通过提供特殊的对象指针，this指针，解决上述问题。**this指针指向被调用的成员函数所属的对象，即哪个对象调用成员函数，this就指向哪个对象。**

&nbsp;
	
this指针是隐含在每一个非静态成员函数内的一种指针

this指针不需要定义，直接使用即可
	

this指针的用途：
* 当形参和成员变量同名时，可用this指针来区分
* 在类的非静态成员函数中返回对象本身，可使用return *this（因为this本来指向的就是对象，解引用后返回的就是对象本身）

![this](https://github.com/CamWu-cyber/Tencent/blob/main/c%2B%2B/images/this.png)

	#include<iostream>
	using namespace std;

	class Person {
	public:
		//命名冲突，此时编译器认为三个age是同一个
		//解决方案有两个
		//1、一般成员变量的命名前面都会加m_，表示member，改成int m_Age
		//正确的命名方式可以少许多麻烦
		//2、改写成this->age = age;
		Person(int age)
		{	
			//this指针指向 p1
			this->age = age;
		}

		//传入p的引用
		//返回p2的引用
		Person& PersonAddAge(Person& p)
		{
			this->age += p.age;

			//this指向p2的指针，而*this指向的就是p2这个对象本体
			return *this;
		}

		int age;
	};

	//1、解决名称冲突
	void test01()
	{
		Person p1(18);
		cout << "p1的年龄为： " << p1.age << endl;
	}

	//2、返回对象本身用*this
	void test02()
	{
		Person p1(10);

		Person p2(10);
		p2.PersonAddAge(p1).PersonAddAge(p1).PersonAddAge(p1); //链式编程思想
		cout << "p2的年龄为： " << p2.age << endl;
	}

	int main() {
		test01();
		test02();

		system("pause");
		return 0;
	}

	运行结果：p1的年龄为： 18
		 p2的年龄为： 40

#### 空指针访问成员函数
C++中空指针也可以调用成员函数，但是也要注意有没有用调用成员变量，因为调用成员变量的时候会用到this指针，而此时this指针为空，是无法调用成员变量的，会报错。

&nbsp;

如果用到this指针，需要加以判断保证代码的健壮性

&nbsp;

	#include<iostream>
	using namespace std;

	class Person {
	public:
		void showClassName()
		{
			cout << "this is Person class" << endl;
		}

		void showPersonAge()
		{	
			//在成员变量里面都默认加了this指针的
			//报错的原因是因为传入的指针是NULL

			//为了让代码不崩，需要对this指针进行判断
			if (this == NULL)
			{
				return;
			}

			cout << "age = " << m_Age << endl;
		}

		int m_Age;
	};

	void test01()
	{	
		//用空指针调用两个成员函数
		//调用第一个函数的时候没问题，第二个函数的时候就报错，区别在于第二个函数里面用到了成员变量m_Age
		Person* p = NULL;
		p->showClassName();
		p->showPersonAge();
	}

	int main() {
		test01();
		system("pause");
		return 0;
	}

#### const修饰成员函数
**常函数：**
* 成员函数后加const后我们称这个函数为**常函数**
* 常函数内不可以修改成员属性
* 成员属性声明时加关键字mutable后，在常函数中依然可以修改

**常对象：**
* 声明对象前加const称该对象为常对象
* 常对象的成员属性也不可以修改，除非属性前加了mutable
* 常对象只能调用常函数

demo:
	
	#include<iostream>
	using namespace std;

	//常函数
	class Person {
	public:
		//this指针的本质 是指针常量(形如Person* const this) 指针的指向对象是不可以修改的(比如令this=NULL就会报错),但是指针指向的值是可以修改的
		//为了让this指针指向的值也不被修改，需要加const关键字修饰(形如const Person* const this)
		//这个操作经过官方的设定就变为：在成员函数后面加const，修饰的是this指针，让指针指向的值也不可以修改
		void showPerson() const
		{
			/*this->m_A = 100;
			this = NULL;*/ //this指针不可以修改指针的指向对象的
		}

		void func() {}

		int m_A = 100;
		mutable int m_B; //特殊变量，即使在常函数中，也可以修改这个值，加关键字mutable
	};

	void test01()
	{
		Person p;
		p.showPerson();
	}

	// 常对象

	void test02()
	{
		const Person p; //在对象前加const，变为常对象
		//p.m_A = 100;
		p.m_B = 100; //m_B是特殊值，在常对象下也可以修改

		//常对象只能调用常函数
		p.showPerson();
		//p.func(); //常对象不可以调用普通成员函数，因为普通成员函数可以修改属性，这与const的定义是相悖的
	}

## 友元
在程序中，有些私有（private）属性也想让类外特殊的一些函数或者类进行访问，就需要用到友元的技术；

友元的目的就是让一个函数或者类 访问另外一个类中的私有成员；
	
友元的关键字为 friend
	
具体等用到的时候再来学吧。。。

## 运算符重载
对已有的运算符重新进行定义，赋予其另一种功能，以适应不同的数据类型

运算符重载的关键词为 operator
	
具体等用到的时候再来学吧。。。

## 类和对象-继承（父类-子类）
#### 继承的基本语法
继承好处：减少重复代码

语法：class 子类 : 继承方式 父类

子类 也称为 派生类

父类 也称为 基类

	#include<iostream>
	using namespace std;

	//公共界面
	class BasePage {
	public:
		void header()
		{
			cout << "首页、公开课、登录、注册...（公共头部）" << endl;
		}
		void footer()
		{
			cout << "帮助中心、交流合作、站内地图...（公共底部）" << endl;
		}
	};

	//Java页面
	class Java : public BasePage {
	public:
		void content()
		{
			cout << "Java学科视频" << endl;
		}
	};
	//Python页面
	class Python : public BasePage {
	public:
		void content()
		{
			cout << "Python学科视频" << endl;
		}
	};
	//C++页面
	class CPP : public BasePage {
	public:
		void content()
		{
			cout << "C++学科视频" << endl;
		}
	};

	void test01()
	{
		cout << "Java下载视频界面如下：" << endl;
		Java ja;
		ja.header();
		ja.content();
		ja.footer();

		cout << "----------------------" << endl;
		cout << "Python下载视频界面如下：" << endl;
		Python py;
		py.header();
		py.content();
		py.footer();

		cout << "----------------------" << endl;
		cout << "C++下载视频界面如下：" << endl;
		CPP cpp;
		cpp.header();
		cpp.content();
		cpp.footer();
	}

	int main() {
		test01();
		system("pause");
		return 0;
	}

        运行结果：
	Java下载视频界面如下：
	首页、公开课、登录、注册...（公共头部）
	Java学科视频
	帮助中心、交流合作、站内地图...（公共底部）
	----------------------
	Python下载视频界面如下：
	首页、公开课、登录、注册...（公共头部）
	Python学科视频
	帮助中心、交流合作、站内地图...（公共底部）
	----------------------
	C++下载视频界面如下：
	首页、公开课、登录、注册...（公共头部）
	C++学科视频
	帮助中心、交流合作、站内地图...（公共底部）

#### 继承方式
继承的语法： class 子类 : 继承方式 父类
**继承方式一共有三种：**
* 公共继承
* 保护继承
* 私有继承
	
demo:
	#include<iostream>
	using namespace std;

	//继承方式

	//公共继承
	class Base1
	{
	public:
		int m_A;
	protected:
		int m_B;
	private:
		int m_C;
	};

	class Son1 : public Base1
	{
	public:
		void func()
		{
			m_A = 10;//父类中公共权限成员 到子类中依然是公共权限
			m_B = 10;//父类中保护权限成员 到子类中依然是保护权限
			//m_C = 10;//父类中私有权限成员 子类访问不到
		}
	};

	void test01()
	{
		Son1 s1;
		s1.m_A = 100;
		s1.m_B = 100;//到Son1中 m_B是保护权限 类外访问不到
	}

	//保护继承
	class Son2 : protected Base1 {
	public:
		void func()
		{
			m_A = 100;//父类中公共权限成员 到子类变为保护权限
			m_B = 100;//父类中保护权限成员 到子类中依然是保护权限
			//m_C = 100;//父类中私有权限成员 子类访问不到
		}
	};

	void test02()
	{
		Son2 s2;
		//s2.m_A = 1000;//在Son2中 m_A变为保护权限，因此类外访问不到
		//s2.m_B = 1000;//在Son2中 m_B为保护权限，因此类外访问不到
	}

	//私有继承
	class Son3 : private Base1 {
	public:
		void func()
		{
			m_A = 100;//父类中公共权限成员 到子类变为私有权限
			m_B = 100;//父类中保护权限成员 到子类变为私有权限
			//m_C = 100;//父类中私有权限成员 子类访问不到
		}
	};

	void test03()
	{
		Son3 s3;
		//s3.m_A = 1000;//在Son3中 m_A变为私有权限，因此类外访问不到
		//s3.m_B = 1000;//在Son3中 m_B变为私有权限，因此类外访问不到
	}

#### 继承中的对象模型
父类中私有成员也被子类继承下去了，只是由编译器给隐藏后访问不到

	#include<iostream>
	using namespace std;

	class Base {
	public:
		int m_A;
	protected:
		int m_B;
	private:
		int m_C;
	};

	class Son : public Base {
	public:
		int m_D;
	};

	//利用开发人员命令提示工具查看对象模型
	//跳转盘符 F:
	//跳转文件路径 cd 具体路径下
	//查看命令
	//cl /dl reportSingleClassLayout类名 文件名

	void test01()
	{
		// 16
		// 父类中所有非静态成员属性都会被子类继承下去
		// 父类中私有成员属性 是被编译器给隐藏了，因此是访问不到，但是确实被继承下去了
		cout << "size of Son = " << sizeof(Son) << endl;
	}

	int main(){
		test01();
		system("pause");
		return 0;
	}

        运行结果：
	size of Son = 16

#### 同名成员处理
问题：当子类与父类出现同名的成员，如何通过子类对象，访问到子类或父类中同名的数据呢？
* 访问子类同名成员 直接访问即可
* 访问父类同名成员 需要加作用域

demo:
	
	#include<iostream>
	using namespace std;

	class Base {
	public:
		Base()
		{
			m_A = 100;
		}
		void func()
		{
			cout << "Base 下 func()调用" << endl;
		}
		//父类中函数重载了，如何访问函数呢？
		void func(int a)
		{
			cout << "Base 下 func(int a)调用: "<< a << endl;
		}
		int m_A;
	};

	class Son : public Base {
	public:
		Son()
		{
			m_A = 200;
		}
		void func()
		{
			cout << "Son 下 func()调用" << endl;
		}
		int m_A;
	};

	void test01()
	{
		Son s;
		cout << "Son 下 m_A = " << s.m_A << endl;
		//如果通过子类对象 访问到父类中同名成员，需要加作用域
		cout << "Base 下 m_A = " << s.Base::m_A << endl;
	}

	void test02()
	{
		Son s;
		s.func(); //直接调用 调用的是子类中的同名函数

		//如何调用到父类中同名成员函数？
		s.Base::func();

		//如果子类中出现和父类同名的成员函数，子类的同名成员会隐藏掉父类中所有同名成员函数
		//如果想访问到父类中被隐藏的同名成员函数，需要加作用域
		s.Base::func(100);
	}

	int main()
	{
		test01();
		test02();
		system("pause");
		return 0;
	}
	
	运行结果：
	Son 下 m_A = 200
	Base 下 m_A = 100
	Son 下 func()调用
	Base 下 func()调用
	Base 下 func(int a)调用: 100
