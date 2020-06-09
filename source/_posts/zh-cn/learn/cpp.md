---
title: cpp
category: learn
date: 2020-02-08 16:45:08
tags:
toc: true
---

Notes on Cherno C++ Tutorial

TODO: 重新格式化这篇博文

<!-- more -->

> 写在前面: 如果你在看到一半的时候, 若出现**还没有讲到过的操作出现在示范代码中**(比如new), 知道它**基本起什么作用**即可, 在后面的episodes肯定会细讲

> 如果你看到夹杂英文, 那是我直接copy了@Cherno的话并懒得翻译

## Notice

1. Always pass your function parameters with const
2. if object have pointer variable, write copy constructor

## How C++ Works

概念：每个cpp都会编译成一个obj,但一个cpp基本等于一个translation unit，除非这些cpp互相include组成一个大cpp文件

## How the C++ Compiler Works

Compile按钮(ctrl+f7)

## How the C++ Linker Works

工程设置->Preprocessor->Preprocess to a File以生产预处理文件（经编译器解析了宏的源码）
编译阶段只是编译文件，外部definaion函数在link阶段，没有被调用的函数声明会被优化掉
static基本表示该函数只为这个translation unit声明的
宏使用`#if`和`#endif`作为条件判断
`#if 1`为`true`

**头文件函数出现重复定义的问题和`inline`的使用** （inline实际上就是将函数整个代码复制到条用这个函数的函数那里）

## Variables in C++

数据类型大小
| | |
|---| --- |
| char | 1字节 |
| short | 2字节 |
| int | 4字节 | 
| long | **通常**4字节 |
| long long | 8字节 |
| float | 4字节 |
| double | 8字节 |

核心思想：c++所有数据类型仅仅只是一个变量存储空间大小的规定

默认的浮点数是double类型的, 所以别忘了给加上"f"如5.5f如果你只用float的精度


节约内存的小技巧
由于内存只能寻址到byte,而bool类型只有1字节，巧妙的方法是将8个bool存在一个byte里

sizeof(数据类型)查看数据类型大小

## Functions in C++

函数functions与方法methods的区别仅仅在于方法是包含在类中的。当说到函数时就是明确地说某种不属于某个class的东西

划分函数不要太过分，为了让我们调用一个函数，我们需要为这个函数创建一整个stack frame(栈框架)，
也就是说我们得把参数之类的东西push到栈上，还要把一个叫做返回地址的东西放到栈上(以在函数执行完之后返回调用函数的地方)，
所以为了执行函数指令而在内存中跳转来跳转去会消耗额外的时间，

总结，函数的主要目的是防止代码重复
通常将函数拆分为声明和定义：声明写在头文件中，定义写在translation unit中

## C++ Header Files

头文件：任何以#开头的都是预处理语句，

#pragma once可以防止在一个translation unit中多次include一个头文件
(遇到多次include的现象可能是头文件include另一个头文件)

比#pragma once更烂的方法（clion自带）：
Log.h中，有
```C++
#ifndef _LOG_H
#define _LOG_H

......

#endif
```

## How to DEBUG C++ in VISUAL STUDIO

调试：vs中watch界面可以指定监视的变量，memory window可以搜索&a查看变量a的地址
未初始化的变量值默认为0xccccccc

## Visual Studio Setup for C++

在Solution Explorer下使用Show All Files视图

默认vs把中间文件放在项目的debug文件夹下, 把最终二进制文件放在Solution文件夹下
推荐设置Output Directory为$(SolutionDir)bin\$(Platform)\$(Configuration)\
Intermediate Directory为$(SolutionDir)bin\intermediate\$(Platform)\$(Configuration)\
bin前不加\因为$(SolutionDir)变量自带backslash

## CONDITIONS and BRANCHES in C++

基本上0代表false, 任何其他的数代表true

else if = else { if(){} }

## Loops in C++

C++的语法是自由的, 不一定循规蹈率, 比如 for循环 可以写成这样

```C++
int i = 0;
bool condition = true;
for ( ; condition; )
{
    Log("Hello World!");
    i++;
    if (!(i<5))
        condition = false;
}
```

## POINTERS in C++

指针: 指针代表的仅仅只是一个数字, 一个内存地址, 指针的数据类型一般用于标志指针对应地址上数据的数据类型.

如果我们不关心数据的数据类型, just using `void*`

例子, 分配一个8个char的空间的数组, memset填充00, 最后delete[]操作删除数组的内存

```C++
char* buffer = new char[8];
memset(buffer, 0, 8);

delete[] buffer;
```

指向指针的指针

```C++
... // 接上面的那个栗子
char** ptr = &buffer;
```

解说: 若实际运行时`ptr`储存的数据为`0x00B6FED4` (也就是 `buffer` 这个变量的地址), 用VS2019自带的Memory View查看,
**我们看到的是**指针buffer储存的数据为`f0 dd d0 00` (指针`buffer`指向的 `8个char的空间的数组` 的地址),
由于x86的设计我们从Memory View**看到的是反转的数据**, 实际内容应该是`00 d0 dd f0` (即 0x00d0ddf0), 这个地址就是 `8个char的空间的数组` 所在地.

## Reference in C++

函数使用引用类型参数可以使代码**相比指针参数更简洁**(访问变量不使用需要去引用化符号 "*")

## CLASSES vs STRUCTS in C++

Struct和Class**无本质区别**; Struct中的变量默认是Public修饰. 同时, Class可以继承Struct(但编译器会报警)

Struct更适合存储多个对象或基本数据类型

@Cherno 写Class的规范:

1. m_LogLevel的"m_"表示变量是类内部(private)变量
2. public和private可以分离, 如一部分Public修饰变量, 另一部分Public修饰函数:

   ```C++
   class Log {
   public:
       const int LogLevelError = 0;
       const int LogLevelWarning = 1;
       const int LogLevelInfo = 2;
   private:
       int m_LogLevel = LogLevelInfo;
   public:
       void SetLevel(int level)
       {
           m_LogLevel = level;
       }
   
       void Error(const char* message){
           if (m_LogLevel >= LogLevelError)
               std::cout << "[Error]:" << message << std::endl;
       }
   
       void Warn(const char* message){
           if (m_LogLevel >= LogLevelWarning)
           std::cout << "[WARNING]:" << message << std::endl;
       }
       void Info(const char* message){
           if (m_LogLevel >= LogLevelInfo)
           std::cout << "[INFO]:" << message << std::endl;
       }
   }
   ```

   可以看到这个类有两处 `public:`

## Static in C++

Basically just to `cut to the chase` (切入正题/长话短说),
static outside of class means that the linkage of that symbol that you declare to be static is going to be internal meaning. It;s only going to be visible to that translation unit that you've defined it in.

Whereas a static variable inside a class or struct means that variable is actually going to share memory with all of the instances of the class meaning that basically across all instances that you create of that class or struct, there's only going to be one instance of that static variable
and a similar thing applies to static methods in a class there is no instance of that class being passed into that method. (Cherno讲的是背后的原理)

第一种情况翻译: 由于translation unit中的(不是class中的)全局变量/函数是全局可见的, 所以加上static让全局变量只在它所在的translation unit可见. 否则在不同的translation unit声明相同符号的变量将在链接阶段报重复定义错误. 类似于translation unit的private修饰

@Cherno 的命名规范:

s_开头代表变量为静态 `static int s_Variable = 5;`

***

尽量在头文件使用static变量, 因为它只是简单将内容复制到cpp文件

### 局部Static类型变量

使变量在局部可见的同时有static的性质

```c++
void Function()
{
	static int i = 0;
	i++;
	std::cout << i << std::endl;
}

int main()
{
	Function();
	Function();
	Function();
	Function();
}
```

### 经典的Singleton类

Singleton类在整个程序中只有一个实例

```C++
// 第一种实现
class Singleton
{
private:
	static Singleton* s_Instance;
    Singleton() {}; // 这一行作用阻止类被实例化, 后面构造函数会讲
public:
	static Singleton& Get() { return *s_Instance; };
	void Hello() { std::cout << "Hello" << std::endl; };
};

Singleton* Singleton::s_Instance = nullptr;

int main()
{
	Singleton::Get().Hello();
```

```C++
// 或者将对象实例放在局部静态变量中(好处: 相比上一种更少的代码量)
class Singleton
{
private:
    Singleton() {}; // 这一行作用阻止类被实例化, 后面构造函数会讲
public:
	static Singleton& Get() 
	{
		static Singleton instance;
		return instance;
	};
	void Hello() { std::cout << "Hello" << std::endl; };
};

int main()
{
	Singleton::Get().Hello();
}
}
```

## Enum in C++

枚举用来管理标识符, 增强代码可读性, 本质是Integer(可指定which types of integer you want to be)
如
(模式是32位Integer, 这里用unsigned char只有8位可以节约内存)

```C++
enum Example : unsigned char
{
    A, B, C
};
```

使用enum后的Log类

```C++
class Log
{
public:
    enum Level
    { // LevelError = 0代表从零开始递进, 当然默认就是0
        LevelError = 0, LevelWarning, LevelInfo
    };
private:
    Level m_LogLevel = LevelInfo;
public:
    void SetLevel(Level level)
    {
        m_LogLevel = level;
    }

    void Error(const char* message) {
        if (m_LogLevel >= LevelError)
            std::cout << "[Error]:" << message << std::endl;
    }

    void Warn(const char* message) {
        if (m_LogLevel >= LevelWarning)
            std::cout << "[WARNING]:" << message << std::endl;
    }
    void Info(const char* message) {
        if (m_LogLevel >= LevelInfo)
            std::cout << "[INFO]:" << message << std::endl;
    }
};

int main() {
    Log log;
    log.SetLevel(Log::LevelWarning);
    log.Error("Hello");
    log.Warn("Hello");
    log.Info("Hello");
}
```

## Constructors in C++

You have to manually initialize all of your primitive types otherwise they will be set to whatever was left over in that memory.

禁止实例化类, 只需要把构造函数设为Private, 或者删除构造函数
If you did not want people creating instances...

```C++
class Log
{
Private:
    Log() {} // One way
public:
    Log() = delete; // Another way

    static void Write()
    {

    }
};

int main()
{
    // Only Write() can be invoke
    Log::Write();

    // Now you cannot access the constructor
    Log l;
}
```

## Destructors in C++

折构函数可以被这样调用, 但是基本上不会用到, It's weired

someClass.~someClass();

## Inheritance in C++

没啥好说的, 唯一要注意的是继承的类的大小将是: 父类所有的变量总和 + 自己声明变量总和

Use colon to inerit a class

```C++
class Sub_Class : Main_Class
{

}
```

## Virtual Function in C++

为什么要有Virtual函数

```C++
class Entity
{
public:
    std::string GetName() { return "Entity"; }  
};

class Player : public Entity
{
private:
    std::string m_Name;
public:
    Player(const std::string& name)
        : m_Name(name) {}

    std::string GetName() { return m_Name; }
};

int main()
{
    Entity* entity = new Entity();
    std::cout << entity->GetName() << std::endl;

    Player* player = new Player("Cherno");
    Eneity* entity2 = player;
    std::cout << entity2->GetName() << std::endl;
}
```

输出:

```
Entity
Entity
```

发现问题了吗, 第二个输出本应是"Cherno"

实际上当指针类型是父类Entity时, 通过该指针调用子类Player类的重载函数GetName()并没有被调用, 而是父类定义的内容, 这会导致许多问题



C++11引入了Override关键字, 它不是必须的, 但能够避免发生拼写错误并能增强代码的可读性

正确示范:

```C++
class Entity
{
public:
    // 加上了virtual
    virtual std::string GetName() { return "Entity"; }  
};

class Player : public Entity
{
private:
    std::string m_Name;
public:
    Player(const std::string& name)
        : m_Name(name) {}

    // 加上了override
    std::string GetName() override { return m_Name; }
};

int main()
{
    Entity* entity = new Entity();
    std::cout << entity->GetName() << std::endl;

    Player* player = new Player("Cherno");
    Eneity* entity2 = player;
    std::cout << entity2->GetName() << std::endl;
}
```

总结:

If you want to override a function you have to mark the base function in the base class as virtual.

不重要的补充: 

1. 使用virtual可以减少dynamic dispatch这种操作的使用 (运行时修改对象的vtable).

2. 很不幸virtual它不属于免费的午餐, 首先使用它会需要额外的Vtable空间以让我们能够dispatch to the correct function that includes a member pointer in the actual base class that points to the Vtable. 第二, 我们每次调用virtual函数我们都要通过那个table来决定which function to actually map to.

    但是基本上区别并不明显, 所以能用就用. 除非你的程序要跑在一个性能真的非常非常差的嵌入式设备上.


## Interface (Pure virtual method)

在Printable中创建一个GetClassName的赋值为0的函数, 它就是一个接口

这个类也不能被直接实例化, 需要实现此方法的子类才能工作

```C++
class Printable{
public:
    virtual std::string GetClassName() = 0;
}
```

简单的示例

```C++

class Player : public Entity, Printable // 子类可以继承多个接口类
{
public:
    std::string GetClassName() override { return "Player"; }
}

void Print(Printable* obj) // 通过这样的函数, 我们只需要实现这个方法, 该函数就会调用我们的实现
{
    std::cout << obj->GetClassName() << std::endl;
}

void main()
{
    Player player = new Player();
    Print(player);

    Print(new Player); // 不要这么用, 会导致内存泄漏
}

```

## Visibility in C++

The default visibility of a Class would actually be private; if its a Struct then it would be public by default.

private标记的内容只有本类能访问, 子类也不行, 但是朋友类(friend class)可以, 会在以后视频中讲到.

protected标记是在private基础上, 子类可以访问

## Arrays in C++

### Raw Arrays

举个例子`int example[5]`

数组名称 example 是一个指针, 指向 example[0] 的所在地址

请注意, 不能使用超出数组下标的操作(如`example[-1] = 0`), 那意味着访问不在当前数组许可范围内的内存; 在debug模式下它会报错, 但在release下它可能会造成不可预料的后果

还可以用指针的方式访问数组

```C++
int example[5];
int* ptr = example;
for (int i = 0; i< 5; i++)
    example[i] = 2;

example[2] = 5;
// equals to *(ptr + 2) = 5;
// 因为这个指针是int类型, 所以每次+1都向后偏移4字节的内存地址(对指针进行加减操作是不同于普通的);
// 所以也可以写成:
// *(int*)((char*)ptr + 8) = 6;
// 即先以1 byte的char*进行指针加法操作, 然后再转回int*
// It is pretty wild line of code.
```

更多的, 两种不同的创建数组的方式

```C++
int example[5];
int* another = new int[5];
// 前者是创建在stack上的并且会在函数执行完后被摧毁
// 后者是创建在heap上的
// 建议在类中使用前者, 在函数中使用后者

// We need to delete using the square brackets
delete[] another;
```

**在C++中你无法动态地检查一个普通数组的大小**

```C++
int* another = new int[5];

int count = sizeof(another) / sizeof(int);
// 使用这样的方式是不靠谱的
// 由于another是个指针, 所以最终 count 的结果是1, 这显然不对
```

一个比较好的办法是管理一些记录数组大小的常量

```C++
static const int exampleSize = 5;
int example[size];
```

### C++11 Standard Arrays

好处是能很方便地检查一个数组的大小

```C++
#include <array>

int main(){
    std::array<int, 5> another;

    for (int i = 0; i < another.size(); i++)
        eanother[i] = 2;
}
```

两者区别是Standard Arrays 相比 Raw Array s有更多性能的开销(但是值得), 且更安全

@TheCherno 更喜欢用Raw Arrays, Because he like to live dangerously :)


## How Strings Work in C++ (and how to use them)

### C风格字符串

```C++
const char* name = "Cherno";
```

字符串分配的内存是fixed allocated block, 这表示想要扩充只能通过分配全新的字符串(新的地址)并删除旧的字符串来实现
同时由于这是个char指针所以这个字符串不能用delete(后面会细讲new和delete, rule of thumb is just if you don't use 'new' keyword dont't use the 'delete' keyword)

#### 细节的东西

`null termination character`: 每个字符串末尾都会有一个null结束符0(int类型, 等价于'\0'这个char类型)
比如 `char name[7] = {'C', 'h', 'e', 'r', 'n', 'o', '\0'};`
如果没有结束符, 那么当使用cout输出字符串时, 它会一直输出接下来的内存数据直到读到'\0'. (表现为输出"Cherno"然后跟着一串乱码)

**注意**: Double quote "" by default it becomes a char pointer; 翻译: "Cherno" 默认表示为类型为 char* 的数据.
为什么是 const char* ? 因为字符串本质是char数组, 而数组变量本质上储存的也就是这个数组的起始地址, 所以用 const char* 没毛病, cout对const char*有特殊对待不不断地读下去直到碰到前文所说的`null termination character`

同时, 从Memory View看到的十六进制 `cc` 大概率表示we are right outside of the bounds of our allocation.

### C++ 标准字符串

**本质上是char array**

```C++
#include <iostream>
#include <string> // cout没有输出string类型的overload, 如果不输出可以不用include

int main()
{
    

    std::string name = "Cherno" + " hello!"; //这种方式是不对的, 因为"Cherno"是一个 const char* 类型
    
    std::string name = "Cherno";
    name += " hello!"; // 这种是对的, a nice easy way, because you are adding it to a string, and "+=" is overloaded in the string class to be able to let you do that.
    // 或者这样(虽然会有更多的对象拷贝操作, 大多数情况下性能影响不大)
    std::string name = std::string("Cherno") + " hello!";

    std::cout << name << std::endl;
}
```

简单的使用:

```C++
// 判断字符串是否包含指定文字
bool const contains = name.find("no") != std::string::npos // std::string::npos 代表没有找到返回的值

// 因为字符串操作在c++中很普遍, 而每次都进行不必要的对象复制无疑很低效, 使用const xxx&将对象直接以只读形式传入函数
void PrintString(const std::string& string)
{
    string += "h";
    std::cout << string << std::endl;
}
```
## String literals


Terminal character will actuall break the behavior of this string in many cases
```C++
#include <stdlib.h>

int main()
{
    const char name[8] = "Che\0rno";
    std::cout << strlen(name) << std::endl;
}
```

String literals are stored in read-only section of memory
This code might not vaild for all CPP compilers.
And edit this string is actually didn't work.
```C++
int main()
{
    char* name = "Cherno";
    // 实际上这种定义是错误的, 字符串类型指针应该永远是const char*
    // 要想运行时修改字符串, 正确操作应是定义一个字符串数组而不是一个指针
    // char name[] = "Cherno";
    name[2] = 'a';
    std::cout << name << std::endl;
}

```

### 其他类型的字符串
```C++
int main()
{
    const char* name = u8"Cherno"; // utf-8, u8前缀非必要
    const char16_t* name2 = u"Cherno"; // two bytes per character (utf-16)
    const char32_t* name3 = U"Cherno"; // four bytes per character (utf-32)
    const wchar_t* name4 = L"Cherno"; // 由编译器决定, eitger 2 ir 4 bytes, its 2 bytes on Windows and 4 on Linux and I acpect Mac as well
}
```

C++14标准
```C++
int main()
{
    using namespace std::string_literals;
    std::string name0 = "Cherno"s + " hello";
    std::wstring name1 = L"Cherno"s + L" hello";
    std::u32string name1 = U"Cherno"s + U" hello";

// raw形式赋值, 对拷贝大篇文章时保留文章格式有用
    const char* rawstring = R"(Line1
Line2
Line3
Line4)";
}
```

## const in C++

### const pointer

在前, you cannot change the data at that memory address
在后, you cannot reassign the actual pointer itself to point something else

```C++
const inst MAX_AGE = 90;

// 1. Pointer to const value
const int* a = new int;
*a = 2; // I cannot change the contents of the pointer
a = (int*) &MAX_AGE; // But I can change the pointer itself

// 2. Const pointers
int* const b = new int;
*b = 2;
b = (int*) &MAX_AGE; // but you cannot change the pointer whose value

// 3. Const pointer to a const value
const int* const c = (int*) &MAX_AGE;
```

注意: `const int*` 和 `int const*` 是相等的, 你需要将`const` 放在 `*` 号后面

<span id="const_method"></span>
### const method

const 修饰的方法无法改变类中的变量
Only avaliable in class method

涉及内容:
1. 当const方法返回指针类型时怎么做: 方法数据类型应为 "const int* const"
2. 以 `const Object&` 实例化的对象只能访问const方法
3. `mutable` 变量

```C++
class Entity
{
private:
    int m_X, *m_Y, *m_Z; // 定义多个变量时指针类型要为每个单独加 '*' 号
    mutable int var;
public:
    int GetX() const
    {
        // m_X = 2; 你不能修改变量的值
        var = 233; // 但我们可以在const方法中修改mutable修饰的变量
        return m_X;
    }

    const int* const GetY() const // 若const方法要返回指针类型变量, 方法数据类型应为 "const int* const"
    {
        return m_Y;
    }

    void SetX()
    {
        m_X = 2;
    }
}

void PrintEntity (const Eneity& e)
{
    // std::cout << e.SetX() << endl; 常量型对象无法访问无const修饰的方法
    std::cout << e.GetX() << endl;
}

int main()
{
    Entity e = new Entity();
    PrintEneity(e);
}
```

## Mutable Keyword in C++

`mutalbe`有两种用法, \
第一种用于常量方法中, 见[#const method](#const_method),
第二种用于lambda, 用的不多

```C++
int x = 8;
auto f = [=]() mutable
{
    x++;
    std::cout << x << std::endl;
}

f();
// x is still = 8, 因为[=]模式 is just copy this value into this lambda, mutable代表它能更改传递进来的值并且不让编译器报错, 但实际上8本身是个常数所以没有效果.
```

**什么是lambda**: a lambda is basically like a little throwaway function that you can write and assign to a variable quickly.


## Memory Initializer Lists in C++ (Constructor Initializer List)

使用构造初始化列表可以避免实列化两次对象

```C++
class Example
{
public:
    Example()
    {
        std::cout << "Created Entity!" << std::endl;
    }

    Example(int x)
    {
        std::cout << "Create Entity with " << x << "!" << std::endl;
    }
}

class Entity
{
private:
    Example m_example;
    int x, y, z;
public:
    Entity()
        : m_example(Example(8)), x(0), y(0), z(0) // m_example(Example(8)) 或者m_example(8)都行
        {
            // m_Example = Example(8); 如果在构造函数中实列化内部变量, 该对象会实例化两次
        }
}

int main()
{
    Entity e0;
}
```

## Ternary Operators in C++ (Conditional Assignment)

使用三目运算符可以简化if_else语句
```C++
#include <string>

static int s_Level = 1;
static int s_Speed = 2;

int main()
{
    s_Speed = s_Level > 5 && s_Level < 100 ? s_Level > 10 ? 15 : 10 : 5;

    std::cout << s_Speed << std::endl;

    // 以上式子等价于
    // if (s_Level > 5 && s_Level < 100)
    // {
    //     if(s_Level > 10)
    //     {
    //         s_Speed = 15;
    //     }
    //     else
    //     {
    //         s_Speed = 10;
    //     }
    // } 
    // else 
    // {
    //     s_Speed = 5;
    // }
}
```

## How to CREATE/INSTANTIATE OBJECTS in C++

There are two main section of memory: the stack and heap

Stack objects have an automatic lifespan, there lifetime is actually controlled by the scope their declared

Heap: once you allocated an object in that heap, it's gonna sit there unill you explicit delete it

栈空间一般只有1M到几M(取决于平台), 如果你需要创建一个巨大的对象, 你将不得不将他创建在堆内存上


不使用 `new` 关键字, 对象创建在栈空间上
```C++
Entity entity("Cherno");
// 或者
Entity entity = Entity("Cherno");
```

使用 `new` 关键字, 对象创建在堆空间上
```C++
// new 返回的是创建对象的内存地址, 所以用 Entity*
Entity* entity = new Entity("Cherno");
// 你需要手动删除 new 创建的对象来释放内存
delete entity;
```

所以用 `new` 创建对象很容易导致内存泄漏, 之后会讲的 **智能指针** 可用很好地解决这个问题

## The NEW Keyword in C++

更多new的细节

How the `new` keyword find free space on memory? There is something called *free list* which actually maintain the addresses that have bytes free. It's obvously written in intelligent but its stll quite slow.

几点事实:
1. `new` is just an operator, means that you can overload `new` and change its bahavious
2. Usually calling `new` will call the underlying C Function `malloc()`
3. **delete** also calls the destructor

三种 `new` 的不同使用方法(normal new, array new, placement new)
```C++
int main()
{
    // If we wanted an array of entries (An array which is stored 50 Objects of Entity)
    Entity* entity = new Entity();

    delete entity; // Remember delete object

    // If using new with square bracket "[" and "]"
    // The new operator is actually a slightly differ function than normal
    Entity* entity2 = new Entity()[50];
    // Also we need calling delete with square bracket
    delete[] eneity2;

    // Placement New is where you actually get to decide kind of where the memory comes from.
    // You don't really allocating memory wieh new,
    // you're just calling the constructor and initializing you Object in a specific memory address
    int* buffer = new int[50];
    Entity* entity3 = new(buffer) Entity();
    delete entity3;
    delete[] buffer;
    // in C there are some kinds of equivalent:
    // Entity* entity = (Entity*) malloc(sizeof(Entity));
    // malloc() will not call the constructor so you need to call it in manual
    // So it's better don't use this in C++
}
```

## Implicit Conversion and the Explicit Keyword in C++

```C++
class Entity
{
private:
    std::string m_Name;
    int m_Age;
public:
    Entity(const std::string& name)
        : m_Name(name), m_Age(-1) P{}
    // Use explicit keyword to disable implicit conversion
    explicit Entity(int age)
        : m_Name("Unknown"), m_Age(age) {}
}

void PrintEntity(const Entity& entity)
{
    // Printing Statements
}

int main()
{
    // It's weird , you can't do this in other languages(such as C# or Java)
    // This is called implicit conversion
    // It implicit converting "Cherno" into Entity's Constructor Method: Entity(const std::string& name)
    Entity a = "Cherno";
    // 虽然上面是个很好的隐式转换的例子, 但是不建议用这种语法实例化对象


    // You cannot do explicit conversion with explicit method anymore
    // This is not allowed
    Entity b = 22;
    // Correct sentence:
    Entity b = Entity(22);
    

    // This is not allowed
    PrintEntity("Cherno"); 
    // "Cherno" is a const char array
    // C++ need to do two conversions, one from const char* to std::string, and then call into Entity(const std::string& name)
    // It's only allowed to do one implicit conversion at same time
    // Correct sentence:
    PrintEntity(std::string("Cherno");
    // or as normal:
    PrintEntity(Entity("Cherno"));
}
```

## OOERATORS and OPERATOR OVERLOADING in C++

In the case of operator overloading you're allowed to define or change the behavior of operator

**Operators are just functions**

Here goes some examples:
```C++
struct Vector2
{
    float x, y;

    Vector2(float x, float y)
        : x(x), y(y) {}

    // overload the function "operator+()" equals redefine the behavior of operator plus in this Object
    Vector2 operator+(const Vector2& other) const
    {
        return Vector2(x + other.x, y + other.y);
    }

    // As same with above
    Vector2 operator*(const Vector2& other) const
    {
        return Vector2(x * other.x, y * other.y);
    }

    // As same with above
    bool operator==(const Vector2& other) const
    {
        return x == other.x && y == other.y;
    }
    
    bool operator!=(const Vector2& other) const
    {
        // We have a simple way
        return !operator==(other);
        // Or
        // return !(*this == other);
    }
};

// See the use case in main()
std::ostream& operator<<(std::string& stream, const Vector2& other)
{
    stream << other.x << ", " << other.y;
    return stream;
}

int main()
{
    Vector2 position(4.0f, 4.0f);
    Vector2 speed(0.5f, 1.5f);
    Vector2 powerup(1.1f, 1.1f);


    Vector2 result1 = position + speed * powerup;

    // We cannot output the variables in vector directly
    // We need overload the function "operator<<"
    std::cout << result1 << std::endl;


    // In programs such like Java we have to use equals() to compare objects
    // but in C++ we can simply overload the "operator=="
    if(retult1 == position) std::cout<< "foo" << std::endl;

}
```

## Ths "this" keyword in C++

this是一个指向当前方法所在对象的指针
避免形参名字和对象成员变量的名字一样所造成的歧义

```C++
class Entity
{
public:
    int x, y;
    Entity(int x, int y)
    {
        // 这里只是示范, 实际有更方便的构造参数列表来赋值
        this->x = x; // or (*this).x = x;
    }
}
```

## Object Lifetime in C++ (Stack/Scope Lifetimes)

下面的例子讲了三件事:
1. 编写函数时不要返回在stack内存空间创建的对象
2. 使用 `{}` 创建局部scope以使stack上的对象更早地被自动清理
3. Unique Pointer的基本原理展示

```C++
int* CreateArray()
{
    // Don't write code like this
    // The array gets cleared as soon as we go out of scope
    int array[50];
    return array;
}

class ScopedPtr
{
private:
    Entity* m_Ptr;
public:
    ScopedPtr(Entity* ptr) : m_Ptr(ptr)
    {}

    ~ScopedPtr()
    {
        delete m_Ptr; // delete Entity when ScopedPtr get deleted
    }
}

int main()
{
    // scope with brace
    {
        Entity e; // the object created on stack will gets free when out of the scope

        // We could use something in the standard library called unique pointer which is a scoped pointer
        // but here we write our own to explain how it works
        ScopedPtr e = new Entity(); // It's implicit conversion
        // because the scoped pointer object gets allocated on the stack which means it will gets deleted when out of the scope and call ~ScopedPtr()
        // then corresponding the Entity will gets deleted
    }
}
```

## SMART POINTERS in C++ (std::unique_ptr, std::shared_ptr, std::weak_ptr)

Smart pointers mean that when you call `new` , you don't have to call `delete`
In face in many cases with smart pointers we don't even have to call `new`

This episode introduce:
1. unique pointer
2. shared pointer & weak pointer

```C++
#include <iostream>
#include <string>
#include <memory>

class Entity()
{
public:
    Entity()
    {
        std::cout << "Created Entity!" << std::endl;
    }

    ~Entity()
    {
        std::cout << "Destoryed Entity~" << std::endl;
    }

    void Print() {}
}

int main()
{
    {
        // All smart pointer are marked as explicit
        std::unique_ptr<Entity> uniqueEntity(new Entity());
        // The preferred way through to construct this would be to assign it to std::make_unique<Entity>
        // The primary that is important for unique pointers is due to exception safety
        std::unique_ptr<Entity> uniqueEntity = std::make_unique<Entity>();

        // You can access it like you would normally
        uniqueEntity->Print();

        // Tou cannot copy unique pointer, you will get a compile error
        std::unique_ptr<Entity> e0 = entity;

        
        // If you go to definition of unique pointer you'll see that the copy constructor
        // and copy assignment operator are deleted;

         // shared pointer is use something called reference counting
         // You create one shread pointer and you define another shared pointer and copy the
         // previous one, the reference count is now 2,
         // when the first one dies(out of scope), the reference count goes down 1
         // and when the last one dies. the reference count goes back to zero,
         // and free the memory.
         // BTW, for shared pointer using make_shared is very recommand because its more efficient
         
         // There is something else that you can use with shared pointer is called weak pointer
         // weak pointer will not increase the reference count
         std::weak_ptr<Entity> weakEntity;
         {

             std::shared_ptr<Entity> sharedEntity = std::make_shared<Entity>();
             weakEntity = sharedEntity2;
         }
         // The sharedEntity will still be free immediately here.

    }
    
}

```

## Copying and Copy Constructors in C++

1. Ues "="(called Shallow Copy) to copy 
   an object created on heap(with `new`) which is without a Copy Constuctors
   or an object created on stack but with private pointer variables(objects created on heap)
   will lead to unexpected results.
   It's essentially just copy the address data in pointer variable
2. So you need a Copy Constructor which delimit the behavior of the copy operation.

```C++
class String
{
private:
    char* m_Buffer;
    unsigned int m_Size;
public:
    String(const char* string)
    {
        m_Size = strlen(string);
        m_Buffer = new char[m_Size + 1]; // +1 for last null termination char
        // You can also use strcpy()
        memcpy(m_Buffer, string, m_Size + 1);
    }

    // Copy Consturcor
    String(const String& other)
        : m_Size(other.m_Size)
    {
        m_Buffer = new char[m_Size + 1];
        // Or if you want to be more exciting, you can use this instead
        // memcpy(this, &other, sizeof(String));
    }

    // Or you can just prevent this object to do copy operation
    //String(const String& other) = delete;

    ~String()
    {
        delete[] m_Buffer;
    }

    char& operator[](unsigned int index)
    {
        return m_Buffer[index];
    }

    // make <<() to be a fried so it can access private variables in this object
    friend std::ostream& operator<<(std::ostream& stream, const String& string);
};

std::ostream& operator<<(std::ostream& stream, const String& string)
{
    stream << string.m_Buffer;
    return stream;
}

int main()
{
    String first_String = "Cherno";
    String second_string = first_string;
    
    second_string[2] = 'a';

    std::cout << first_String << endl;
    std::cout << first_String << endl;
}
``` 

## The Arrow Operator in C++

0. Normal usage
   ```C++
   int main()
   {
       Entity* entity = new Entity();
       entity->x = 2; // = (*entity).x = 2;
   }
   ```
1. It is actually possible to overload the Arror Operator and use it in specific class such as ScopedPtr (For more see previous chapter: SMART POINTERS in C++).
   ```C++
   class Entity
   {
   public:
       void Print() const { std::cout << "Hello!" << std::endl; }
   };

   class ScopedPtr
   {
   private:
       Entity* m_Obj;
   public:
       ScopedPtr(Entity* entity)
           : m_Obj(entity)
       {}

       ~ScopedPtr()
       {
           delete m_Obj;
       }

       Entity* operator->()
       {
           return m_Obj;
       }
   };

   int main()
   {
       ScopedPtr entity = new Entity(); // Do you still remember the Implicit Conversion?
       entity->Print();
   }
   ```
2. It can also be used to get the variable's memory offset in an Object (In some memory hack :)
   ```C++
   struct Vector3
   {
       // I deliberately desrupt the naming order to make it in a different memory layout.
       float z, y, x;
   };
   
   int main()
   {
       // Get the offset of that 'x'
       int offset = (int) &((Vector3*)0)->x; // Or &((Vector3*)nullptr)->x;
       std::cout << offset << std::endl;
   }
   ```

## Dynamic Arrays in C++ (std::vector)

1. Vector in C++ is not mathematical vector, it's kind of like Dynamic Arrays

```C++
struct Vertex
{
    float x, y, z;
};

std::ostream& operator<<(std::ostream& stream, const Vertex& vertex)
{
    stream << vertex.x << ", " << vertex.y << ", " << vertex.z;
    return stream;
}

int main()
{
    // the '<Object>' is called template, will show in later chapter
    // For now, we only need to know this definite the type which the Dynamic Arrarys stores
    std::vector<Vertex> vertices;
    // It is hard to decide whether you should be using like vertex pointers
    // or just vertex object in this case
    // Which is object stored in line or fragmented in memory

    // Add object to the end of Dynamic Array
    vertices.push_back({ 1, 2, 3 }); // Note: Implicit conversion
    vertices.push_back({ 4, 5, 6 });

    // Using range based 'for loop' to iterate the object in Dynamic Array
    for (const Vertex& v : vertices)
        std::cout << v << sed::endl;

    // We can remove object in dynamic array individually
    // This remove the second object in dynamic array
    vertices.erase(vertices.begin() + 1);

    // Or we can clean the whole dynamic array
    vertices.clear();
}
```

## Optimizing the usage of std::vector in C++

Two ways to reduce memory copy

```C++
struct Vertex
{
    float x, y, z;

    Vertex(float x, float y, float z)
        : x(x), y(y), z(z)
    {
    }

    // Copy Constructor, used to capture copied times
    Vertex(const Vertex& v)
        : x(v.x), y(v.y), z(v.z)
    {
        // Output to console to see how many times copied
        std::cout << "Copied!" << std::endl;
    }
};

int main()
{
    std::vector<Vertex> vertices_bad;
    vertices_bad.push_back(Vertex(1, 2, 3)); // Make it easier to read than previous chapter
    vertices_bad.push_back(Vertex(4, 5, 6));
    vertices_bad.push_back(Vertex(7, 8, 9));
    vertices_bad.push_back(Vertex(10, 11, 12));
    // Each pass parameter operation in push_back() will cause 1 copy operation
    // And **each push_back() called will cause memory rearrange**,
    // which are copy previous objects in dynamic array into new memory area.
    // So total copied times: 1 + (1 + 1) + (1 + 2) + (1 + 3) = 10

    std::cout << "vertices good" << std::endl;
    std::vector<Vertex> vertices_good;

    // Below is the optimized implementation
    // 1. Use reserver() to prevent memory rearrange.
    vertices_good.reserve(4);
    // 2. Replace push_back() with emplace_back() to prevent parameter copy
    // it acts as a proxy to process you provided parameter into Constructor.
    vertices_good.emplace_back(1, 2, 3);
    vertices_good.emplace_back(4, 5, 6);
    vertices_good.emplace_back(7, 8, 9);
    vertices_good.emplace_back(10, 11, 12);
}
```

## Using Libraries in C++

Preposition work:
 Visual Studio Setup
   Using GLFW as example:
   1. Create a folder called "Dependencies" under your project directory
      and then put your libraries into it.
      ```txt C:\Users\USERNAME\source\repos\Your_Project_Directory\
      Dependencies\
      -> GLFW\
         -> include\
            -> GLFW\
               glfw3.h
               ...
         -> lib-vc2015\
            glfw3.dll
            glfw3.lib
            glfw3.dll.lib
      ```
   2. Open your project settings:
      -> Configuration: All Configuration
      -> C/C++ -> Additional Include Directories: `$(SolutionDir)\Dependencies\GLFW\include`
      -> Linker -> General -> Additional Library Directories: `$(SolutionDir)\Dependencies\GLFW\lib-vc2015`

### Static linking

Static linking happens at compile time, the lib intergrate into executable or a dynamic library

1. Open your project setting
   -> Linker -> Input -> Additional Dependencies: `glfw3.lib;xxxxxx;balabala;...`
2. Static Link
   ```C++ Main.cpp
   // quote for header in this project, regular bracket for external library
   #include <GLFW/glfw3.h>
   \\ Or `extern "C" int glfwInit();`
   \\ Because GLFW is actually a C library so we need `extern "C"`
   int main()
   {
       int result = glfwInit();
       std::cout << result << std::endl;
   }
   ```

### Dynamic linking

Dynamic linking happens at runtime

* Some librarys like GLFW  supports both static and dynamic linking in a single header file
* `glfw3.dll.lib` is basically a series of pointers into `glwfw3.dll`

0. Code is basically as same as the Static linking above
1. Open your project settings:
   -> Linker -> Input -> Additional Dependencies: `glfw3.dll.lib;xxxxxx;balabala;...`
2. put your dll file(`glfw3.dll`) to the same folder as your executable file(i.e: `$(SolutionDir)\Debug`)
3. In fact, to call a function in dynamic library, it needs a prefix called `__declspec(dllimport)`
   If you explore `glfw3.h` you will see there is a `GLFWAPI` prefix in every function definition,
   ```C++
   #elif defined(_WIN32) && defined(GLFW_DLL)
   /* We are calling GLFW as a Win32 DLL */
    #define GLFWAPI __declspec(dllimport)
   ```
   So **you need to define a Macro** in VS:
   Open your project setting:
   -> C/C++ -> Preprocessor -> Preprocessor Definitions: `GLFW_DLL;xxxxx;bababa...`
   
   But why it seems stll work properly without the `dllimport` prefix?
   In modern windows, `dllimport` is not needed for functions, but `dllimport` is still needed for C++ classes and global variables.

## Making and Working with Libraries in C++ (Multiple Projects in Visual Studio)

1. Visual Studio Setup:
   1. Create one solution with 2 projects "Game" and "Engine",
   2. Project "Game":
   -> Ceneral->Project Defaults->Configuration Type: Application (.exe)
   -> C/C++ -> General -> Additional include Directories: `$(SolutionDir)\Engine\src;`

   3. Project "Engine":
   Ceneral->Project Defaults->Configuration Type: Static library (.lib)

   4. Right click on projects "Game" -> Add -> Reference -> Select project "Enginx"
2. Code for project "Engine":
   ```C++ Your_Project_Directory\src\Engine.h
   #pragma once

   namespace engine {
       void PrintMessage();
   }
   ```
   ```C++ Your_Project_Directory\src\Engine.cpp
   #include "Engine.h"

   #include <iostream>

   namespace engine {
       void PrintMessage()
       {
           std::cout << "Hello World!" << std::endl;
       }
   }
   ```
3. Code for project "Game":
   ```C++ Your_Project_Directory\src\Application.cpp
   #include "Engine.h"

   int main()
   {
       engine::PrintMessage();
   }
   ```

## How to Deal with Multiple Return Values in C++

Example scenario: we have a function called `ParseShader()` , it needs to return two strings

1. Return a struct cotains two strings (Cherno's choose):
   ```C++
   struct ShaderProgramSource
   {
       std::string VertexSource;
       std::string FragmentSource;
   }

   ShaderProgramSource ParseShader()
   {
       // Some statements that process result 'vs' and 'fs'
       // ...
       return { vs, fs };
   }
   ```
2. Using reference paremeter (Probably one of the most optimal way)
   ```C++
   void ParseShader(std::string& outVertexSource, std::string& outFragmentSource)
   {
       // Some statements that process result 'vs' and 'fs'
       // ...
       outVertexSource = vs;
       outFragmentSource = fs;
   }

   main()
   {
       std::string vertexSource, fragmentSource;
       ParseShader(vertexSource, fragmentSource)
   }

   // Or using Pointer parameter if you want to pass nullptr(ignore the output):
   // void ParseShader(std::string* outVertexSource, std::string* outFragmentSource)
   // {
   //     if (outVertexSource)
   //         *outVertexSource = vs;
   //     if (outFragmentSource)
   //         *outFragmentSource = fs;
   // }
   // main()
   // {
   //     ParseShader(nullptr, &fragmentSource)
   // }
   ```
3. Return a `std::array` or `std::vector`(ignored, too simple)
   The different is primarly the arrays can be create on the stack where as
   vectors gonna store its underlying storage on the heap.
   So technically returning a standard array would be faster.
   ```C++
   #include <array>

   std::array<std::string, 2> ParseShader()
   {
       // Some statements that process result 'vs' and 'fs'
       // ...
       std::array<std::string, 2> result;
       result[0] = vs;
       result[1] = fs;
       return result;
   }
   ```
4. Using `std::tuple` and `std::pair`
   1. `std:tuple`
      ```C++
      #include <tuple>

      std::tuple<std::string, std::string> ParseShader()
      {
          // Some statements that process result 'vs' and 'fs'
          // ...
          return std::make_pair(vs.fs);
          // template type is don't need, equal with:
          // return std::make_pair<std::string, std::string>(vs.fs);
      }

      int main()
      {
          // 'auto' means automatically check the return type
          auto sources = ParseShader();
          // Get values in std::tuple
          vertexSource = std::get<0>(sources);
          fragmentSource = std::get<1>(sources);
      }
      ```
   2. `std::pair` (A little bit better than tuple)
      ```C++
      // Show the only difference with 'std::tuple'
      std::pair<std::string, std::string> ParseShader()
      {
          // ...
      }

      int main()
      {
          // ...
          vertexSource = sources.first;
          vertexSource = sources.second;
      }
      ```

## Templates in C++

Template can improve code reuse rate and reduce duplicate code (for example function overload)
The essence of template is similar to macros

This chapter includes:
1. template type
2. template argument
```C++
// In this case, template specifying how to create methods based on your usage of them.(Automatically generate corresponding overloaded function)
// So if nobody call this function, it's code will not exist in compiled file,
// even if there is a grammatical error in this function code, it will still compile successfully.

template<typename T>// exactly same with: "template<class T>"
void Print(T value)
{
    std::cout << value << std::endl;
}

int main()
{
    Print(5);
    // I actually specifying the type explicitly
    // The complete code should be:
    // Print<int>(5);
    Print("Hello");
    Print(5.5f);
}
```

```C++
// Yes, multiple template targets can be in one template definition
template<typename T, int N>
class Array
{
private:
    T m_Arry[N];
public:
    int GetSize() const { return N; }
};

int main()
{
    Array<int, 5> array;
    // Which means its will generate the following code:
    // class Array
    // {
    // private:
    //     int m_Arry[5];
    // public:
    //     int GetSize() const { return 5; }
    // };
    std::cout << array.GetSize() << std::endl;
}
```

## Stack vs Heap Memory in C++

## Macros in C++

## The "auto" keyword in C++

## Static Arrays in C++ (std::array)

## Function Pointers in C++

## Lambdas in C++

## Why I don't "using namespace std"

## Namespaces in C++

## Threads in C++