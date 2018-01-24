### 关于g++和gcc

`gcc main.cpp -lstdc++` #gcc编译cpp不会自动调用链接的c++库, 需要手动链接

`g++ main.cpp` #g++编译cpp会自动调用链接的c++库

### gcc/g++编译的四个过程
- 1. 预处理，生成.i文件
- 2. 将预处理后的文件转为汇编文件，生成.s文件
- 3. 汇编转为机器码，生成.o文件
- 4. 连接机器码，生成可执行程序

### 具体过程
（1）、预处理过程

`g++ -E main.cpp > main.i` 

> 这一步的功能，包括宏的替换，注释的消除，找到相关的库文件，将#include文件的全部内容插入。<>内的文件为系统库，""内的为用户库。

（2）、将预处理后文件转为汇编文件，生成.s文件

`g++ -S main.cpp`

> 生成main.s文件，.s文件表示汇编文件。

（3）、汇编转机器码，生成.o文件

`g++ -c main.cpp`

> .o文件是gcc生成的目标文件，也就是二进制机器码。

（4）、连接目标代码，生成可执行文件

`g++ main.o -o main`

> 如果单独执行g++ main.o，则默认生成a.out，也就是main和a.out仅仅是名字不同而已。

#### 当然也可以用快捷的方式来编译
`g++ main.cpp -o main`

`./main`

### gcc/g++动态库与静态库
##### 静态库
是指编译链接时，把库文件的代码全部加入到可执行文件中。生成的文件比较大，但运行时不需要库文件了。后缀名一般为.a。
##### 动态库
在编译链接时没有把库文件的代码加入到可执行文件中，而是在程序执行时由运行时链接文件加载库，这样可节省系统开销。后缀名一般为.so。gcc/g++在编译时默认使用动态库。无论是静态库还是动态库，都是由.o文件创建的。

##### 构建动态链接库[来自cnblog](http://www.cnblogs.com/zjiaxing/p/5557629.html)
`g++ dynamic_a.cpp dynamic_b.cpp dynamic_c.cpp -fPIC -shared -o libdynamic.so` #将三个文件编译成动态链接库libdynamic.so

`g++ main.cpp -L. -ldynamic -o main` #将main.cpp和libdynamic.so打包成一个可执行文件main

`ldd main`  #若在mac下则用otool -L main，查看动态依赖

##### 编译静态库
`g++ -c dynamic_a.cpp dynamic_b.cpp dynamic_c.cpp` #批量编译三个文件

`ar cr libstatic.a dynamic_a.o dynamic_b.o dynamic_c.o` #将目标文件归档，生成静态库文件libstatic.a

`nm libstatic.a` #查看libstatic.a

`g++ main.cpp -lstatic -L. -static -o main` #生成可执行文件main


### C++指针[资料](http://www.cnblogs.com/ggjucheng/archive/2011/12/13/2286391.html)

> 指针是一种特殊的变量，它里面存储的数值被解释为内存里的一个地址。指针的类型，指针指向的类型，指针本身占据的内存区，指针的指（指向的内存区）。

```
//指针实例
int *ptr;
char *ptr;
int **ptr;
int (*ptr)[3];
int *(*ptr)[4];

//指针的类型
从语法角度，只要把指针声明语句中指针名去掉，剩下的就是指针的类型。
int *ptr; //指针的类型是 int *
char *ptr; //指针的类型是 char *
int **ptr; //指针的类型是 int **
int (*ptr)[3]; //指针的类型是 int (*)[3]
int *(*ptr)[4]; //指针的类型是 int *(*)[4]

//指针指向的类型
从语法角度，只要把指针声明语句中的指针名和名字左边的指针声明符*去掉，剩下的就是指针指向的类型。
int *ptr; //指针指向的类型是 int
char *ptr; //指针指向的类型是 char
int **ptr; //指针指向的类型是 int *
int (*ptr)[3]; //指针指向的类型是 int ()[3]
int *(*ptr)[4]; //指针指向的类型是 int *()[4]



//指针是一个存储计算机内存地址的变量。
int *ptr; //声明一个int类型指针ptr
int val =1; //声明一个int变量val，赋值1
ptr = &val;  //为指针ptr分配一个int值的引用，也就是说指针ptr指向变量val的内存地址
int deref = *ptr;  //对指针进行取值

//void指针、NULL指针和未初始化指针
int *uninit;  //int指针未初始化
int *nullptr = NULL;  //初始化为NULL
void *vptr;  //void指针未初始化
int val = 1;
int *iptr;
int *castptr;
iptr = &val;
vptr = iptr;  //void类型可以存储任意类型的指针或引用
printf("iptr=%p, vptr=%p\n", iptr, vptr);   //打印两个内存地址
castptr = (int *)vptr; //显示转换void指针为int指针
printf("*castptr=%p\n", *castptr);  //指针取值打印
printf("uninit=%p, nullptr=%p\n", uninit, nullptr);  //打印未初始化指针和NULL指针
printf("*nullptr=%d\n", nullptr); //NULL指针被初始化为0


//指针和数组
数组变量是常量，指向数组的第一个元素的内存地址。
数组变量赋值给指针时，指针指向数组第一个元素的内存地址。
int myarray[4] = {1,3,5,7};
int *ptr = myarray;
printf("*ptr=%d\n", *ptr); //结果为1

//指针与结构体
指向结构体的指针存储了结构体第一个元素的内存地址。结构体的指针必须声明和结构体类型一致，或void类型。
struct person {
int age;
char *name;
};
struct person first;
struct person *ptr;
first.age = 21;
char *fullname = "full name";
first.name = fullname;
ptr = &first;
printf("age=%d, name=%s\n", first.age, ptr->name); //也可以(*ptr).name
```
