## 指针
### 指针的定义
    int a = 10;
    int * p = &a;

### 指针的使用（查看指针所指向内存中存放的值）
    //指针变量p前面加 * 的操作叫做 “解引用”
    cout << "*p = " << *p << endl;

### 指针所占内存空间
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

### 空指针
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

### 野指针
野指针：指针指向非法内存空间

    //直接让指针变量p指向内存地址编号为0x1100的空间
    int *p = (int *)0x1100;
    
    //想看看野指针指向地址存放的内容，报错，因为这块空间不是你生成的你没有权限访问它
    cout << *p << endl;

**结论：空指针和野指针都不是我们申请的空间，因此不要访问。**

### 指针、数组、函数三个碰撞在一起的使用
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
### 结构体指针
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

### 结构体数组
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
### 引用的基本使用
1. 作用：给变量起别名。
2. 引用必须初始化。
3. 引用在初始化后，不可以改变。
	
		//此时b就是a的别名，改变b的值就相当于改变了a的值
		int a = 10;
		int &b = a;
	 
### 引用作函数参数
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
		
		
		

### 引用作为函数返回值（感觉不太常用）
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
### 函数默认参数
test(int a, int b=10){}
### 函数占位参数
test(int a, int){}
### 函数重载
作用：函数名称相同，提高复用性（个人感觉你在实际开发中用函数重载会被人打死，慎用！！）
重载满足条件：
* 同一作用域下。
* 函数名称相同。
* 函数参数**类型不同** 或者 **个数不同** 或者 **顺序不同**。

注意：函数的返回值不可以作为函数重载的实现条件

## 类和对象-封装
### 访问权限
* 公共权限 public     类内可以访问 类外可以访问
* 保护权限 protected  类内可以访问 类外不可以访问 儿子可以访问父亲的保护内容
* 私有权限 private    类内可以访问 类外不可以访问 儿子不可以访问父亲的私有内容

### 成员属性私有化
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

### 构造函数析构函数的定义与调用
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

### 初始化列表（感觉不常用）
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

### 类对象作为类成员
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

### 静态成员
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
### 成员变量和成员函数分开存储
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
	
### this指针概念
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

### 空指针访问成员函数
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

### const修饰成员函数
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
### 继承的基本语法
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

### 继承方式
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

### 继承中的对象模型
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
	//cl /d1 reportSingleClassLayout类名 文件名

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

### 同名成员处理
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

### 继承同名静态成员处理方式
问题：继承中同名的静态成员在子类对象上如何进行访问？

静态成员和非静态成员出现同名，处理方式一致
* 访问子类同名成员 直接访问即可
* 访问父类同名成员 需要加作用域

注意：静态成员有两种访问方式 对象访问、类名访问，类名访问中有一个很奇怪的写法，具体解释看代码

	#include<iostream>
	using namespace std;

	// 继承中的同名静态成员处理方式

	class Base {
	public:
		static int m_A;

		static void func()
		{
			cout << "Base - static void func()调用" << endl;
		}
		//父类函数重载
		static void func(int a)
		{
			cout << "Base - static void func(int a)调用" << endl;
		}
	};
	int Base::m_A = 100;

	class Son : public Base {
	public:
		static int m_A;

		static void func()
		{
			cout << "Son - static void func()调用" << endl;
		}
	};
	int Son::m_A = 200;

	//同名静态成员属性
	void test01()
	{
		//1、通过对象访问
		cout << "通过对象访问：" << endl;
		Son s;
		cout << "Son 下 m_A = " << s.m_A << endl;
		cout << "Base 下 m_A = " << s.Base::m_A << endl;

		//2、通过类名访问
		cout << "通过类名访问：" << endl;
		cout << "Son 下 m_A = " << Son::m_A << endl;
		//奇怪的写法  第一个::表示通过类名访问  第二个::表示访问的是父类作用域下的m_A
		cout << "Base 下 m_A = " << Son::Base::m_A << endl; 
	}

	//通过静态成员函数
	void test02()
	{
		//1、通过对象访问
		cout << "通过对象访问" << endl;
		Son s;
		s.func();
		s.Base::func();

		//2、通过类名访问
		cout << "通过类名访问" << endl;
		Son::func();
		Son::Base::func();

		//如果子类中出现和父类同名的静态成员函数，子类的同名静态成员会隐藏掉父类中所有同名静态成员函数
		//如果想访问到父类中被隐藏的同名成员函数，需要加作用域
		Son::Base::func(100);
	}

	int main() {
		test01();
		test02();

		system("pause");
		return 0;
	}
	运行结果：
	通过对象访问：
	Son 下 m_A = 200
	Base 下 m_A = 100
	通过类名访问：
	Son 下 m_A = 200
	Base 下 m_A = 100
	通过对象访问
	Son - static void func()调用
	Base - static void func()调用
	通过类名访问
	Son - static void func()调用
	Base - static void func()调用
	Base - static void func(int a)调用

### 多继承语法
C++允许**一个类继承多个类**

语法：class 子类 : 继承方式 父类1, 继承方式 父类2...

多继承可能会引发父类中同名成员出现，需要加作用域区分
	
**C++实际开发中不建议使用多继承**

## 类和对象-多态
### 多态的基本语法
**多态是C++面向对象三大特性之一**

多态分为两类
* 静态多态：函数重载 和 运算符重载属于静态多态，复用函数名
* 动态多态：子类和虚函数实现运行时多态

静态多态和动态多态区别：
* 静态多态的函数地址早绑定 - 编译阶段确定函数地址
* 动态多态的函数地址晚绑定 - 运行阶段确定函数地址

demo:

	#include<iostream>
	using namespace std;

	//多态

	//动物类
	class Animal {
	public:
		//虚函数
		virtual void speak()
		{
			cout << "动物在说话" << endl;
		}
	};

	//猫类
	class Cat : public Animal{
	public:
		//重写 函数返回值类型 函数名 参数列表 完全相同
		void speak()
		{
			cout << "猫在说话" << endl;
		}
	};

	//狗类
	class Dog : public Animal {
	public:
		void speak()
		{
			cout << "狗在说话" << endl;
		}
	};

	//执行说话的函数
	//这就是地址早绑定 在编译阶段确定函数地址
	//不管你传什么动物，代码都会走animal里面的sperk()
	//如果想执行让猫说话，那么这个函数地址就不能提前绑定，需要在运行阶段进行绑定，也就是地址晚绑定，父类前加virtual，变成虚函数

	//动态多态满足条件：
	//1、有继承关系
	//2、子类重写父类虚函数  注意：重写和重载不一样

	//动态多态使用
	//父类的指针或者引用 指向子类对象

	void doSpeak(Animal &animal) //Animal &animal = cat 父类的引用可以直接指向子类的对象
	{
		animal.speak();
	}

	void test01()
	{
		Cat cat;
		doSpeak(cat);

		Dog dog;
		doSpeak(dog);
	}

	int main() {
		test01();

		system("pause");
		return 0;
	}
	运行结果：
	猫在说话
	狗在说话
	
总结：

动态多态满足条件：
* 有继承关系
* 子类重写父类虚函数  注意：重写和重载不一样

动态多态使用条件
* 父类指针或引用指向子类对象

重写：函数返回值类型 函数名 参数列表 完全相同成为重写

多态的优点：
* 代码组织结构清晰；
* 可读性强；
* 利于前期和后期的扩展以及维护

举例：计算器类。父类就是计算器类AbstractCalculator()，子类分别为加、减、乘、除，这样后期哪个子类有问题就修改哪个子类，扩展的话增加子类就好，写好的就不用管了。
	
### 纯虚函数和抽象类
在多态中，通常父类中虚函数的实现是毫无意义的，主要都是调用子类重写的内容
	
因此可以将虚函数改为**纯虚函数**

纯虚函数语法：virtual 返回值类型 函数名 （参数列表）= 0;
	
当类中有了纯虚函数，这个类也称为**抽象类**
	
**抽象类特点：**
* 无法实例化对象
* 子类必须重写抽象类中的纯虚函数，否则也属于抽象类

demo:

	#include<iostream>
	using namespace std;

	class Base {
	public:

		//纯虚函数
		//只要有一个纯虚函数，这个类称为抽象类
		//抽象函数特点：
		//1、无法实例化对象
		//2、抽象类的子类 必须要重写父类中的纯虚函数，否则也属于抽象类
		virtual void func() = 0;
	};

	class Son : public Base {
	public:
		void func()
		{
			cout << "func函数调用" << endl;
		}
	};

	void test01()
	{
		//Base b; //抽象类无法实例化对象，在栈区中不行
		//new Base; //抽象类无法实例化对象，在堆区中不行

		Son s; //子类必须重写父类中的纯虚函数，否则无法实例化对象

		//使用父类指针指向子类对象
		//new表示在堆区创建的对象，必须使用指针来接应
		Base* base = new Son;
		base->func();

		//使用父类引用指向子类对象，此时子类对象是在栈区创建的
		Base& b = s;
		b.func();
	}

	int main() {
		test01();

		system("pause");
		return 0;
	}
	
	运行结果：
	func函数调用
	func函数调用

### 多态案例-制作饮品
**案例描述：**
制作饮品的大致流程为：注水-冲泡-倒入杯中-加入辅料
	
利用多态技术实现本案例，提供抽象制作饮品基类，提供子类制作咖啡和茶叶
	
**注意：**delete的操作

	#include<iostream>
	using namespace std;

	//多态案例2 制作饮品
	class AbstractDrinking {
	public:
		//煮水
		virtual void Boil() = 0;
		//冲泡
		virtual void Brew() = 0;
		//倒入杯中
		virtual void PourInCup() = 0;
		//加入辅料
		virtual void PutSomething() = 0;
		//制作饮品
		void makeDrink()
		{
			Boil();
			Brew();
			PourInCup();
			PutSomething();
		}
	};

	//制作咖啡
	class Coffee : public AbstractDrinking {
	public:
		//煮水
		virtual void Boil()
		{
			cout << "煮农夫山泉" << endl;
		}
		//冲泡
		virtual void Brew()
		{
			cout << "冲泡咖啡" << endl;
		}
		//倒入杯中
		virtual void PourInCup()
		{
			cout << "倒入杯中" << endl;
		}
		//加入辅料
		virtual void PutSomething()
		{
			cout << "加入糖和牛奶" << endl;
		}
	};

	//制作茶叶
	class Tea : public AbstractDrinking {
	public:
		//煮水
		virtual void Boil()
		{
			cout << "煮农夫山泉" << endl;
		}
		//冲泡
		virtual void Brew()
		{
			cout << "冲泡茶叶" << endl;
		}
		//倒入杯中
		virtual void PourInCup()
		{
			cout << "倒入杯中" << endl;
		}
		//加入辅料
		virtual void PutSomething()
		{
			cout << "加入菊花和枸杞" << endl;
		}
	};

	//制作函数
	void doWork(AbstractDrinking * abs) //AbstractDringking * abs = new Coffee
	{
		abs->makeDrink();
		delete abs; //防止内存泄漏，记得释放堆区对象
	}

	void test01()
	{
		//制作咖啡
		doWork(new Coffee);

		cout << "----------" << endl;
		//制作茶叶
		doWork(new Tea);
	}

	int main() {
		test01();
	}
	
	运行结果：
	煮农夫山泉
	冲泡咖啡
	倒入杯中
	加入糖和牛奶
	----------
	煮农夫山泉
	冲泡茶叶
	倒入杯中
	加入菊花和枸杞
	
	
### 虚析构和纯虚析构
多态使用时，如果子类中有属性开辟到堆区，那么父类指针在释放时无法调用到子类的析构代码，一旦子类对象是在堆区创建的，那么会造成内存溢出。

解决方式：将父类中的析构函数改为**虚析构**或者**纯虚析构**

虚析构和纯虚析构共性：
* 可以解决父类指针释放子类对象
* 都需要具体的函数实现

虚析构和纯虚析构区别：
* 如果是纯虚析构，该类属于抽象类，无法实例化对象
	
虚析构语法：
* virtual ~类名(){}
	
纯虚析构语法：
* virtual ~类名() = 0;
* 类名::~类名(){}

demo:
	
	#include<iostream>
	using namespace std;
	#include<string>

	//虚析构和纯虚析构

	class Animal
	{
	public:
		Animal()
		{
			cout << "Animal构造函数调用" << endl;
		}

		//利用虚析构可以解决 父类指针释放子类指针对象时不干净的问题
		/*virtual ~animal()
		{
			cout << "animal析构虚函数调用" << endl;
		}*/

		//纯虚析构  需要声明也需要实现
		//有了纯虚析构后，这个类也属于抽象类，无法实例化对象
		virtual ~Animal() = 0;

		//纯虚函数
		virtual void speak() = 0;
	};

	Animal::~Animal()
	{
		cout << "Animal 纯 析构虚函数调用" << endl;
	}

	class Cat :public Animal {
	public:

		//构造函数
		//在堆区中创建name属性，然后用m_Name指针维护name属性
		Cat(string name)
		{	
			cout << "Cat构造函数调用" << endl;
			m_Name = new string(name);
		}

		//因为构造函数中在堆区创建了一个对象，所以还需要写一个析构函数来释放掉内存
		~Cat()
		{
			if (m_Name != NULL)
			{
				cout << "Cat析构函数调用" << endl;
				delete m_Name; //释放
				m_Name = NULL; //好习惯，释放掉后再置空
			}
		}

		void speak()
		{
			cout << *m_Name << "小猫在说话" << endl;
		}

		string* m_Name;
	};

	void test01()
	{
		Animal* animal = new Cat("Tom");
		animal->speak();
		//父类指针在析构时候，不会调用子类中析构函数，导致子类如果有堆区属性，会出现内存泄漏
		//解决方式：父类析构函数前 加virtual 即转换成虚析构
		delete animal;
	}

	int main() {
		test01();

		system("pause");
		return 0;
	}
	
	运行结果：
	Animal构造函数调用
	Cat构造函数调用
	Tom小猫在说话
	Cat析构函数调用
	Animal 纯 析构虚函数调用

总结：

1、虚析构或纯虚析构就是用来解决通过父类指针释放子类对象的问题；

2、如果子类对象中没有堆区数据，可以不写虚析构或纯虚析构；

3、拥有纯虚析构函数的类也属于抽象类。

## 文件操作
程序运行时产生的数据都属于临时数据，程序一旦运行结束都会被释放

通过文件可以将数据持久化
	
C++中对文件操作需要包含头文件<fstream>

&nbsp;

文件类型分为两种：

1、**文本文件** - 文件以文本的ASCII码形式存储在计算机中。

2、**二进制文件** - 文件以文本的二进制形式存储在计算机中，用户一般不能直接读懂它们。
	
&nbsp;

操作文件的三大类：

1.ofstream：写操作
	
2.ifstream：读操作
	
3.fstream：读写操作

### 写文件
写文件步骤如下：
	
1、包含头文件
	
   #include<fstream>

2、创建流对象

   ofstream ofs;

3、打开文件

   ofs.open("文件路径",打开方式);
	
4、写数据

   ofs<<"写入数据";
	
5、关闭文件
	
   ofs.close();

&nbsp;
	
文件打开方式：
|  **打开方式**   | **解释**  |
|  ----  | ----  |
| ios::in  | 为读文件而打开文件 |
| ios::out  | 为写文件而打开文件 |
| ios::ate  | 初始位置：文件尾  |
| ios::app  | 追加方式写文件  |
| ios::trunc  | 如果文件存在先删除，再创建  |
| ios::binary  | 二进制方式  |


**注意：** 文件打开方式可以配合使用，利用 | 操作符

**例如：** 用二进制方式写文件 ios::binary | ios::out

	#include<iostream>
	using namespace std;
	#include<fstream> //头文件包含

	//文本文件 写文件

	void test01()
	{
		//1、包含头文件

		//2、创建流对象
		ofstream ofs;

		//3、指定打开方式
		ofs.open("test.txt", ios::out);

		//4、写内容
		ofs << "姓名：张三" << endl;
		ofs << "性别：男" << endl;
		ofs << "年龄：18" << endl;
		
		//5、关闭文件
		ofs.close();
	}

	int main() {
		test01();

		system("pause");
		return 0;
	}
	
	运行结果：在同一目录下生成了test.txt文件，里面的内容就是ofs写入的内容


### 读文件
读文件与写文件步骤相似，但是读取方式相对于比较多

&nbsp;

读文件步骤如下：

1、包含头文件

#include<fstream>

2、创建流对象

ifstream ifs;
	
3、打开文件并判断文件是否打开成功

ifs.open("文件路径",打开方式);
	
4、读数据

四种方式读取
	
5、关闭文件

ifs.close()
	
	#include<iostream>
	using namespace std;
	#include<fstream> //头文件包含
	#include<string>

	//文本文件 读文件

	void test01()
	{
		//1、包含头文件

		//2、创建流对象
		ifstream ifs;

		//3、打开文件 并且判断是否打开成功
		ifs.open("test.txt", ios::in);

		if (!ifs.is_open())
		{
			cout << "文件打开失败" << endl;
			return;
		}

		//4、读数据

		//第一种 文件写入数组中，输出数组
		/*char buf[1024] = {0};
		while (ifs >> buf)
		{
			cout << buf << endl;
		}*/

		//第二种 按行读取
		/*char buf[1024] = {0};
		while (ifs.getline(buf, sizeof(buf)))
		{
			cout << buf << endl;
		}*/

		//第三种 string读取
		/*string buf;
		while (getline(ifs, buf))
		{
			cout << buf << endl;
		}*/

		//第四种 按字符读取 没有读到文件尾就一直读（太慢，不推荐）
		char c;
		while ((c = ifs.get()) != EOF) // EOF end of file
		{
			cout << c;
		}

		//关闭文件
		ifs.close();
	}

	int main() {
		test01();

		system("pause");
		return 0;
	}
	
	运行结果：
	姓名：张三
	性别：男
	年龄：18

总结：
* 读文件可以利用ifstream，或者fstream类
* 利用is_open函数可以判断文件是否打开成功
* close关闭文件


## 模板
* C++另一种编程思想称为 泛型编程，主要利用的技术就是模板
* C++提供两种模板机制：**函数模板**和**类模板**
	
### 函数模板
函数模板作用：建立一个通用函数，其函数返回值类型和形参类型可以不具体指定，用一个**虚拟的类型**来代表。

&nbsp;

**语法:**
	
	template<typename T>
	函数声明或定义

**解释:**

temple --- 声明创建模板

typename --- 表面其后面的符号是一种数据类型，typename可以用classs代替

T --- 通用的数据类型，名称可以替换，通常为大写字母

	#include<iostream>
	using namespace std;

	//交换两个整型函数
	void swapInt(int &a, int &b) 
	{
		int temp = a;
		a = b;
		b = temp;
	}

	//交换两个浮点型函数
	void swapFloat (float &a, float &b)
	{
		float temp = a;
		a = b;
		b = temp;
	}

	//函数模板
	template<typename T> //声明一个模板，告诉编译器后面代码中紧跟着的T不要报错，T是一个通用数据类型
	void mySwap(T &a, T &b)
	{
		T temp = a;
		a = b;
		b = temp;
	}

	void test01()
	{
		//交换两个整型
		int a = 10;
		int b = 20;
		//swapInt(a, b);
		/*cout << "a = " << a << endl;
		cout << "b = " << b << endl;*/

		//交换两个浮点型
		float c = 1.1;
		float d = 1.2;
		/*swapFloat(c, d);
		cout << "c = " << c << endl;
		cout << "d = " << d << endl;*/

		//利用函数模板交换
		//两种方式使用函数模板

		//1、自动类型推导
		//mySwap(a, b);
		//2、显示指定类型
		mySwap<int>(a, b);
		cout << "a = " << a << endl;
		cout << "b = " << b << endl;
	}

	int main() {
		test01();

		system("pause");
		return 0;
	}
	
        运行结果：
        a = 20
        b = 10

总结：
* 函数模板利用关键字template
* 使用函数模板有两种方式：自动类型推导、显示指定类型
* 模板的目的是为了提高复用性，将类型参数化

### 模板函数注意事项
* 自动类型推导，必须推导出一直的数据类型T，才可以使用
* 模板必须要确定出T的数据类型，才可以使用

### 普通函数与函数模板区别
* 普通函数调用可以发生隐式类型转换
* 函数模板 用自动类型推导，不可以发生隐式类型转换
* 函数模板 用显示指定类型，可以发生隐式类型转换

隐式类型转换：比如，传入的参数是char，函数内部运行的是加法，编译器就隐式地把char变量转成了对应的ascii码

### 普通函数与函数模板的调用规则
调用规则如下：

1.如果函数模板和普通函数都可以实现，优先调用普通函数

2.可以通过空模板参数列表来强制调用函数模板
	
3.函数模板也可以发生重载

4.如果函数模板可以产生更好的匹配，优先调用函数模板

### 模板的局限性
模板并不是万能的，有些自定义的数据类型就不能直接使用模板，需要用具体化的方式做特殊处理
	
	#include<iostream>
	using namespace std;
	#include<string>

	//模板的局限性

	class Person
	{
	public:
		Person(string name, int age)
		{
			this->m_Name = name;
			this->m_Age = age;
		}
		string m_Name;
		int m_Age;
	};

	//对比两个数据是否相等函数模板
	template<class T>
	bool myCompare(T &a, T &b)
	{
		if (a == b)
		{
			return true;
		}
		else {
			return false;
		}
	}

	//利用具体化Person的版本实现代码，具体化优先调用
	template<> bool myCompare(Person& p1, Person& p2)
	{
		if (p1.m_Name == p2.m_Name && p1.m_Age == p2.m_Age) {
			return true;
		}
		else {
			return false;
		}
	}

	void test01()
	{
		int a = 10;
		int b = 20;
		bool ret = myCompare(a, b);
		if (ret) {
			cout << "a == b" << endl;
		}
		else {
			cout << "a!=b" << endl;
		}
	}

	void test02()
	{
		Person p1("Tom", 10);
		Person p2("Tom", 10);
		bool ret = myCompare(p1, p2);
		if (ret) {
			cout << "p1 == p2" << endl;
		}
		else {
			cout << "p1!=p2" << endl;
		}
	}

	int main() {
		//test01();
		test02();

		system("pause");
		return 0;
	}

        运行结果：
        p1 == p2

总结：
* 利用具体化的模板，可以解决自定义类型的通用化
* 学习模板并不是为了写模板，而是在STL能够运用系统提供的模板
	
### 类模板
类模板作用：
* 建立一个通用类，类中的成员数据类型可以不具体制定，用一个**虚拟的类型**来代表。

**语法：**
	
	template<typename T>
	类

**解释:**

temple --- 声明创建模板

typename --- 表面其后面的符号是一种数据类型，typename可以用classs代替

T --- 通用的数据类型，名称可以替换，通常为大写字母

**示例**
	
	#include<iostream>
	using namespace std;
	#include<string>

	//类模板
	template<class NameType, class AgeType>
	class Person
	{
	public:
		Person(NameType name, AgeType age)
		{
			this->m_Name = name;
			this->m_Age = age;
		}

		void showPerson()
		{
			cout << "name:" << this->m_Name << "age: " << this->m_Age << endl;
		}

		NameType m_Name;
		AgeType m_Age;
	};

	void test01()
	{
		Person<string, int> p1("孙悟空", 999);
		p1.showPerson();
	}

	int main()
	{
		test01();

		system("pause");
		return 0;
	}
	
        运行结果：
        name:孙悟空age: 999
	
总结：类模板和函数模板语法相似，在声明模板template后面加类，此类称为类模板

### 类模板与函数模板区别
* 类模板没有自动类型推导的使用方式
* 类模板在模板参数列表中可以有默认参数
	
### 类模板中成员函数创建时机
类模板中成员函数和普通类中成员函数创建时机是有区别的：
* 普通类中的成员函数一开始就会创建并编译
* 类模板中的成员函数在调用时才创建，一开始不会创建所以编译都会通过，调用后编译器才会去判断是否调用正确

### 类模板对象做函数参数
类模板实例化出的对象，向函数传参的方式，一共有三种：
* 指定传入类型 --- 直接显示对象的数据类型           **最常用**
* 参数模板化   --- 将对象中的参数变为模板进行传递
* 整个类模板化 --- 将这个对象类型 模板化进行传递

**示例：**

	#include<iostream>
	using namespace std;
	#include<string>

	//类模板对象做函数参数

	template<class T1, class T2>
	class Person
	{
	public:
		Person(T1 name, T2 age)
		{
			this->m_Name = name;
			this->m_Age = age;
		}

		void showPerson()
		{
			cout << "姓名：" << this->m_Name << "年龄：" << this->m_Age << endl;
		}

		T1 m_Name;
		T2 m_Age;
	};

	//1.指定传入类型
	void printPerson1(Person<string, int>&p)
	{
		p.showPerson();
	}

	void test01()
	{
		Person<string, int>p("孙悟空", 999);
		printPerson1(p);
	}

	//2.参数模板化
	template<class T1, class T2>
	void printPerson2(Person<T1, T2>&p)
	{
		p.showPerson();
		cout << "T1的类型为：" << typeid(T1).name() << endl;   //显示模板类型的名称，注意：string类型的名字很长
		cout << "T2的类型为：" << typeid(T2).name() << endl;
	}

	void test02()
	{
		Person<string, int>p("猪八戒", 999);
		printPerson2(p);
	}


	//3.整个类模板化
	template<class T>
	void printPerson3(T &p)
	{
		p.showPerson();
		cout << "T的数据类型为：" << typeid(T).name() << endl;
	}

	void test03()
	{
		Person<string, int>p("唐僧", 999);
		printPerson3(p);
	}


	int main() {
		test01();
		test02();
		test03();

		system("pause");
		return 0;
	}
	运行结果：
	姓名：孙悟空年龄：999
	姓名：猪八戒年龄：999
	T1的类型为：class std::basic_string<char,struct std::char_traits<char>,class std::allocator<char> >
	T2的类型为：int
	姓名：唐僧年龄：999
	T的数据类型为：class Person<class std::basic_string<char,struct std::char_traits<char>,class std::allocator<char> >,int>
	
### 类模板与继承
当类模板碰到继承时，需要注意以下几点：
* 当子类继承的父类是一个类模板时，子类在声明的时候，要指定出父类中T的类型
* 如果不指定，编译器无法给子类分配内存
* 如果想要灵活指定出父类中的T的类型，子类也需变为类模板
	
## STL初识
### STL的诞生
* 长久以来，软件界一直希望建立一种可重复利用的东西
* C++的**面向对象**和**泛型编程**思想，目的就是**复用性的提升**
* 大多情况下，数据结构和算法都未能有一套标准，导致被迫从事大量重复工作
* 为了建立数据结构和算法的一套标准，诞生了STL
	
### STL基本概念
* STL(Standard Template Library, 标准模板库)
* STL从广义上分为：**容器（container）算法（algorithm）迭代器（iterator）**
* **容器**和**算法**之间通过**迭代器**进行无缝连接
* STL几乎所有的代码都采用了模板类或者模板函数

### STL六大组件
STL大体分为六大组件，分别是：容器、算法、迭代器、仿函数、适配器（配接器）、空间配置器

1.容器：各种数据结构，如vector、list、deque、set、map等，用来存放数据

2.算法：各种常用算法，如sort、find、copy、for_each等

3.迭代器：扮演了容器与算法之间的胶合剂

4.仿函数：行为类似函数，可作为算法的某种策略

5.适配器：一种用来修饰容器或者仿函数或迭代器接口东西

6.空间配置器：负责空间的配置与管理。

### STL中容器、算法、迭代器
**容器：**  置物之所也

STL容器就是将运用最广泛的一些数据结构实现出来

常用的数据结构：数组、链表、树、栈、队列、集合、映射表等

这些容器分为**序列式容器**和**关联式容器**两种：

* 序列式容器：怎么放的就是怎样的顺序
* 关联式容器：二叉树结构，跟放入的顺序无关，容器内部会对元素进行排序
	
**算法：** 问题之解法也

算法分为：质变算法和非质变算法。
	
* 质变算法：是指运算过程中会更改区间内的元素内容。例如拷贝、替换、删除等等

* 非质变算法：是指运算过程中不会更改区间内的元素内容，例如查找、计数、遍历、寻找极值等等
	
**迭代器：**   容器和算法之间粘合剂
提供一种方法，使之能够依序访问某个容器所含的各个元素，而又无需暴露该容器的内部表示方式。
	
每个容器都有自己专属的迭代器
	
迭代器使用非常类似于指针，初学阶段我们可以先理解迭代器为指针
	
常用的容器中迭代器种类为双向迭代器，和 随机访问迭代器
	

## 容器算法迭代器初识
### vector存放内置数据类型
容器：vector

算法：for_each
	
迭代器：vector<int>::iterator
	
**示例：**
	
	#include<iostream>
	using namespace std;
	#include<vector>
	#include<algorithm>

	//vector容器存放内置数据类型

	void myPrint(int val)
	{
		cout << val << endl;
	}

	void test01()
	{	
		//创建了一个vector容器，数组
		vector<int> v;

		//向容器中插入数据
		v.push_back(10);
		v.push_back(20);
		v.push_back(30);
		v.push_back(40);

		//通过迭代器访问容器中的数据
		//vector<int>::iterator itBegin = v.begin(); //起始迭代器，指向容器中第一个元素
		//vector<int>::iterator itEnd = v.end(); //结束迭代器，指向容器中最后一个元素的下一个位置

		//第一种遍历方式
		//while (itBegin != itEnd)
		//{
		//	cout << *itBegin << endl;
		//	itBegin++;
		//}

		//第二种遍历方式
		for (vector<int>::iterator it = v.begin(); it != v.end(); it++)
		{
			cout << *it << endl;
		}

		//第三种遍历方式  利用STL提供遍历算法
		//for_each(v.begin(), v.end(), myPrint);
	}

	int main() {
		test01();

		system("pause");
		return 0;
	}
	
	运行结果：
	10
	20
	30
	40
	
### vector存放自定义的数据类型
	
**示例：**
	
	#include <iostream>
	using namespace std;
	#include <vector>
	#include<string>

	//vector容器存放自定义数据
	class Person
	{
	public:
		Person(string name, int age)
		{
			this->m_Name = name;
			this->m_Age = age;
		}
		string m_Name;
		int m_Age;
	};

	void test01()
	{
		vector<Person>v;

		Person p1("aaa", 10);
		Person p2("bbb", 20);
		Person p3("ccc", 30);
		Person p4("ddd", 40);

		//向容器中添加数据
		v.push_back(p1);
		v.push_back(p2);
		v.push_back(p3);
		v.push_back(p4);

		//遍历容器中数据
		for (vector<Person>::iterator it = v.begin(); it != v.end(); it++)
		{	
			//两种写法均可
			//cout << "姓名：" << (*it).m_Name << "年龄：" << (*it).m_Age << endl;
			cout << "姓名：" << it->m_Name << "年龄：" << it->m_Age << endl;
		}
	}

	//存放自定义数据类型的指针
	void test02()
	{
		vector<Person*>v;

		Person p1("aaa", 10);
		Person p2("bbb", 20);
		Person p3("ccc", 30);
		Person p4("ddd", 40);

		//向容器中添加p1-p4的地址
		v.push_back(&p1);
		v.push_back(&p2);
		v.push_back(&p3);
		v.push_back(&p4);

		//遍历容器
		for (vector<Person*>::iterator it = v.begin(); it != v.end(); it++)
		{
			// 解引用的结果就是尖括号里的数据类型，此处是一个Person类的指针，那么想通过指针拿到属性，得在后面加上->
			cout << "姓名：" << (*it)->m_Name << "年龄：" << (*it)->m_Age << endl;
		}
	}

	int main() {
		//test01();
		test02();

		system("pause");
		return 0;
	}
	
	运行结果：
	姓名：aaa年龄：10
	姓名：bbb年龄：20
	姓名：ccc年龄：30
	姓名：ddd年龄：40

### vector容器嵌套容器
**示例：**
	
	#include<iostream>
	using namespace std;
	#include<string>
	#include<vector>

	//容器嵌套容器
	void test01() {
		vector<vector<int>>v;

		//创建小容器
		vector<int>v1;
		vector<int>v2;
		vector<int>v3;
		vector<int>v4;

		//向小容器中添加数据
		for (int i = 0; i < 4; i++) {
			v1.push_back(i + 1);
			v2.push_back(i + 2);
			v3.push_back(i + 3);
			v4.push_back(i + 4);
		}

		//小容器插入大容器
		v.push_back(v1);
		v.push_back(v2);
		v.push_back(v3);
		v.push_back(v4);

		//通过大容器，遍历所有数据
		for (vector<vector<int>>::iterator it = v.begin(); it != v.end(); it++)
		{
			// (*it) --- 容器 vector<int>
			for (vector<int>::iterator vit = (*it).begin(); vit != (*it).end(); vit++)
			{
				cout << *vit << " ";
			}
			cout << endl; //一行输出完了，空一行

		}
	}


	int main() {
		test01();

		system("pause");
		return 0;
	}
	运行结果：
	1 2 3 4
	2 3 4 5
	3 4 5 6
	4 5 6 7

## STL常用容器
### string容器
### string基本概念
**本质：**
	
* string是C++风格的字符串，而string本质上是一个类
* char*是C语言的字符串

**特点：**
	
string类内部封装了很多成员方法
	
例如：查找find，拷贝copy, 删除delete，替换replace，插入insert
	
string管理char*所分配的内存，不用担心复制越界和取值越界等，由类内部进行负责
	
### string构造函数
构造函数原型：
	
* string();                 //无参构造。创建一个空的字符串 例如：string str;
* string(const char* s);     //相当于出入C语言的字符串来构造C++的字符串。使用字符串s初始化
* string(const string& str);  //拷贝构造。用一个已知的string对象来构造一个新的string对象
* string(int n, char c);      //使用n个字符c初始化
	
**实例：**
	
	#include<iostream>
	using namespace std;
	#include<string>

	//string的构造函数

	//string();                 //无参构造。创建一个空的字符串 例如：string str;
	//string(const char* s);     //相当于出入C语言的字符串来构造C++的字符串。使用字符串s初始化
	//string(const string& str);  //拷贝构造。用一个已知的string对象来构造一个新的string对象
	//string(int n, char c);      //使用n个字符c初始化

	void test01() 
	{
		string s1;   //无参

		const char* str = "hello world"; 
		string s2(str);
		cout << "s2 = " << s2 << endl;

		string s3(s2);    //拷贝构造
		cout << "s3 = " << s3 << endl;

		string s4(10, 'a');
		cout << "s4 = " << s4 << endl;
	}

	int main() {
		test01();

		system("pause");
		return 0;
	}
	运行结果：
	s2 = hello world
	s3 = hello world
	s4 = aaaaaaaaaa

### string赋值操作
功能描述：
* 给string字符串进行赋值
	
赋值的函数原型：
* string& operator=(const char* s);                 //char*类型字符串 赋值给当前字符串
* string& operator=(const string &s);               //把字符串s赋给当前的字符串
* string& operator=(char c);                        //字符赋值给当前的字符串
* string& assign(const char *s);                     //把字符串s赋给当前的字符串
* string& assign(const char *s, int n);              //把字符串s的前n个字符赋给当前的字符串
* string& assign(const string &s);                   //把字符串s赋给当前字符串
* string& assign(int n, char c);                     //用n个字符c赋给当前字符串

**示例：**
	
	#include<iostream>
	using namespace std;
	#include<string>

	void test01() 
	{
		string str1;
		str1 = "hello world";
		cout << "str1 = " << str1 << endl;

		string str2;
		str2 = str1;
		cout << "str2 = " << str2 << endl;

		string str3;
		str3 = 'a';
		cout << "str3 = " << str3 << endl;

		string str4;
		str4.assign("hello C++");
		cout << "str4 = " << str4 << endl;

		string str5;
		str5.assign("hello C++", 5);
		cout << "str5 = " << str5 << endl;

		string str6;
		str6.assign(str5);
		cout << "str6 = " << str6 << endl;

		string str7;
		str7.assign(10, 'w');
		cout << "str7 = " << str7 << endl;
	}

	int main() {
		test01();

		system("pause");
		return 0;
	}
	运行结果：
	str1 = hello world
	str2 = hello world
	str3 = a
	str4 = hello C++
	str5 = hello
	str6 = hello
	str7 = wwwwwwwwww
	
### 字符串的查找和替换
* find查找是从左往右，rfind从右往左
* find找到字符串后返回查找的第一个字符位置，找不到返回-1
* replace在替换时，要指定从哪个位置起，多少个字符，替换成什么样的字符串
	
**示例：**
	
	#include<iostream>
	using namespace std;
	#include<string>

	//1.查找 find元素第一次出现的位置
	void test01()
	{
		string str1 = "abcdefgde";
		int pos = str1.find("de");
		if (pos == -1)
		{
			cout << "未找到字符串" << endl;
		}
		else {
			cout << "pos = " << pos << endl;
		}

		//rfind 和 find 区别
		//rfind从右往左查找   find从左往右查
		pos = str1.rfind("de");
		cout << "pos = " << pos << endl;
	}

	//2.替换
	void test02()
	{
		string str1 = "abcdefg";
		//从1号位置起，3个字符 替换为“1111”
		str1.replace(1, 3, "1111");
		cout << "str1 = " << str1 << endl;
	}

	int main()
	{
		test01();
		test02();
		system("pause");
		return 0;
	}
	运行结果：
	pos = 3
	pos = 7
	str1 = a1111efg

### string字符串比较
字符串之间的比较
*  = 返回 0
*  > 返回 1
*  < 返回 -1

**函数原型：**
* int compare(const string &s) const;  //与字符串s比较
* int compare(const char *s) const;    //与字符串s比较
      
**示例：**
      
      #include<iostream>
	using namespace std;

	//字符串比较

	void test01()
	{
		string str1 = "xello";
		string str2 = "hello";

		if (str1.compare(str2) == 0)
		{
			cout << "str1 等于 str2" << endl;
		}
		else if (str1.compare(str2) > 0)
		{
			cout << "str1 大于 str2" << endl;
		}
		else
		{
			cout << "str1 小于 str2" << endl;
		}
	}

	int main() {
		test01();

		system("pause");
		return 0;
	}
	运行结果：
	str1 大于 str2

### string字符存取

string中单个字符存取有两种方式
* char& operator[](int n);   //通过[]方式取字符
* char& at(int n);           //通过at方法获取字符
	
**示例：**
	
	#include<iostream>
	using namespace std;
	#include<string>

	//string字符存取

	void test01()
	{
		string str = "hello";

		//1.通过 []访问单个字符
		for (int i = 0; i < str.size(); i++)
		{
			cout << str[i] << " ";
		}
		cout << endl;

		//2.通过 at 访问单个字符
		for (int i = 0; i < str.size(); i++)
		{
			cout << str.at(i) << " ";
		}
		cout << endl;

		//修改单个字符
		str[0] = 'x';
		str.at(1) = 'x';
		cout << "str = " << str << endl;
	}

	int main()
	{
		test01();

		system("pause");
		return 0;
	}
	运行结果：
	h e l l o
	h e l l o
	str = xxllo
	
总结：string字符串中单个字符存取有两种方式，利用[] 或 at

### string插入和删除
对string字符串进行插入和删除字符操作
**函数原型**
* string& insert(int pos, const char *s);    //插入字符串
* string& insert(int pos, const string &str);    //插入字符串
* string& insert(int pos, int n, char c);    //在指定位置插入n个字符c
* string& erase(int pos, int n = npos);      //删除从pos开始的n个字符
	
**示例：**
	
	#include<iostream>
	using namespace std;
	#include<string>

	//字符串 插入和删除
	void test01()
	{
		string str = "hello";

		//插入
		str.insert(1, "111");
		//hlllello
		cout << "str = " << str << endl;

		//删除
		str.erase(1, 3);    //从 1 号位置开始删除 3 个字符
		cout << "str = " << str << endl;
	}

	int main()
	{
		test01();

		system("pause");
		return 0;
	}
	运行结果：
	str = h111ello
	str = hello

总结：插入和删除的起始下标都是从0开始

### string字串
从字符串中获取想要的子串
	
**函数原型：**
* string substr(int pos = 0, int n = npos) const;     //返回由pos开始的n个字符串组成的字符串

**示例：**
	
	#include<iostream>
	using namespace std;
	#include<string>

	//字符串 插入和删除
	void test01()
	{
		string str = "abcdef";

		string subStr = str.substr(1, 3);

		cout << "subStr = " << subStr << endl;
	}

	//实用操作
	void test02()
	{
		string email = "zhangsan@sina.com";

		//从邮件地址中获取 用户名 信息
		int pos = email.find("@");   // 8
		cout << pos << endl;

		string usrName = email.substr(0, pos);
		cout << usrName << endl;
	}

	int main()
	{
		test01();
		test02();

		system("pause");
		return 0;
	}
	运行结果：
	subStr = bcd
	8
	zhangsan
	
总结：灵活运用求子串功能，可以在实际开发中获取有效信息。

###
