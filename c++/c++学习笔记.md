### 指针

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
