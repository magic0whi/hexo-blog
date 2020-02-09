---
title: cpp
category: learn
date: 2020-02-08 16:45:08
tags:
---

Cherno C++ Tutorial Notes

TODO: 重新格式化这篇博文

<!-- more -->

 > 写在前面: 如果你在看到一半的时候, 若出现**还没有讲到过的操作出现在示范代码中**(比如new), 知道它**基本起什么作用**即可, 在后面的episodes肯定会细讲

else if = else { if(){} }

在Solution Explorer下使用Show All Files视图

默认vs把中间文件放在项目的debug文件夹下, 把最终二进制文件放在Solution文件夹下
推荐设置Output Directory为$(SolutionDir)bin\$(Platform)\$(Configuration)\
Intermediate Directory为$(SolutionDir)bin\intermediate\$(Platform)\$(Configuration)\
bin前不加\因为$(SolutionDir)变量自带backslash

for是自由的不需要循规蹈矩

int i = 0;
bool condition = true;
for ( ; condition; )
{
    Log("Hello World!");
    i++;
    if (!(i<5))
        condition = false;
}

指针: 指针仅仅只是一个数字, 一个内存地址, 指针的数据类型一般用于标志指针对应地址上数据的数据类型.
如果我们不关心数据的数据类型, just using void*

例子, 分配一个8个char的空间的array, memset填充00, 最后delete[]操作删除数组的内存
	char* buffer = new char[8];
	memset(buffer, 0, 8);

	delete[] buffer;

指向指针的指针如 char** ptr = &buffer;
实际运行ptr为0x00B6FED4 (指针buffer的地址), 对应地址的数据为f0 dd d0 00 (指针buffer指向地址), 由于x86是反转的, 实际值为00 d0 dd f0, 即0x00d0ddf0对应地址上的就是buffer实际的数据

Reference
函数使用引用类型参数可以使代码相比指针参数更简洁(访问变量不需要去引用化)

Class vs Struct
Struct和Class无本质区别, Struct变量默认是Public修饰, Class可以继承Struct(编译器会报警)
Struct更适合存储多个对象或基本数据类型

写Class规范
m_LogLevel的"m_"表示变量是类内部(private)变量
public和private可以分离, 如一部分Public修饰变量, 另一部分Public修饰函数:
(这个例子中LogLevel等级可以使用枚举代替)
```C++
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
```

Static on C++
Basically just to cut to the chase (切入正题/长话短说),
static outside of class means that the linkage of that symbol that you declare to be static is going to be internal meaning. It;s only going to be visible to that translation unit that you've defined it in.

Whereas a static variable inside a class or struct means that variable is actually going to share memory with all of the instances of the class meaning that basically across all instances that you create of that class or struct, there's only going to be one instance of that static variable
and a similar thing applies to static methods in a class there is no instance of that class being passed into that method. (Cherno讲的是背后的原理)

第一种情况翻译: 由于translation unit中的(不是class中的)全局变量/函数是全局可见的, 所以加上static让全局变量只在它所在的translation unit可见. 否则在不同的translation unit声明相同符号的变量将在链接阶段报重复定义错误. 类似于translation unit的private修饰

命名规范
s_开头代表变量为静态
static int s_Variable = 5;

尽量在头文件使用static变量, 因为它只是简单将内容复制到cpp文件

局部Static类型变量
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

经典的Singleton类
```C++
class Singleton
{
private:
	static Singleton* s_Instance;
public:
	static Singleton& Get() { return *s_Instance; };
	void Hello() { std::cout << "Hello" << std::endl; };
};

Singleton* Singleton::s_Instance = nullptr;

int main()
{
	Singleton::Get().Hello();

或者放在局部静态变量

class Singleton
{
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

## Enum枚举

管理标识符, 增强代码可读性, 本质是Integer(可指定which types of integer you want to be)
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

# Constructors in C++

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

# Destructors in C++

折构函数可以被这样调用, 但是基本上不会用到, It's weired

someClass.~someClass();

# Inheritance in C++

没啥好说的, 唯一要注意的是继承的类的大小将是: 父类所有的变量总和 + 自己声明变量总和

Use colon to inerit a class

```C++
class Sub_Class : Main_Class
{

}
```

# Virtual Function in C++

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