---
title: cpp
category: learn
date: 2020-02-08 16:45:08
tags:
toc: true
---

Notes on Cherno C++ Tutorial

TODO: Format this article

<!-- more -->

## Notice

1. If an object have pointer variable inside, write a copy constructor and use it.
2. Reduce memory usage by using 1 byte to store 8 bools.
3. Default float type is `double`, use `float a = 5.5f`.
4. Header or inline?  (the later copys whole function body into the area where the function is called).


## How C++ Works

* A translation unit typically consists of a single `.cpp` file and its associated `.h` files.
* Compile Process Path: Source&rarr;Compile&rarr;Linker&rarr;Executables

1. Pre-process
   Compiler parses all the macros (such as `#include`) in source file.
   - VS2015: `Project Settings`->`Preprocessor`->`Preprocess to a File`
   - GCC: `cpp hello.cpp > hello.i`
2. Compile & Assembly
   Each `.cpp` compile to one `.o` (aka. `.obj`) file.
   - VS2015: Compile Only (`<C-F7>`)
   - GCC: `g++ -S hello.i && as hello.s -o main.o`
3. Linker
   Externally defined functions will be integrated in the link phase. Function declarations that never be called will be optimized away.
   
   The parameter of `ld` are platform specific (mainly depends on GCC version). Enable verbose to get the parameter of the `collect2` (which is an alias of `ld`):
   ```console
   $ g++ -v -o hello hello.cpp
   ```
   Lt may looks messy, but you can probably significantly shorten that link line by removing some arguments. Here's the minimal set I came up after some experimentations:
   ```console
   $ ld \
   -I /lib64/ld-linux-x86-64.so.2 \
   -o hello
   /usr/lib/Scrt1.o \
   /usr/lib/crti.o \
   /usr/lib/gcc/x86_64-pc-linux-gnu/14.1.1/crtbeginS.o \
   -L /usr/lib/gcc/x86_64-pc-linux-gnu/14.1.1 \
   hello.o \
   -l stdc++ \
   -l c \
   /usr/lib/gcc/x86_64-pc-linux-gnu/14.1.1/crtendS.o \
   /usr/lib/crtn.o
   ```

## Variables

> The difference between C++ data types are simply different allocated memory size.
> You can use `sizeof()` to see the data type size.

Different memory allocation size for C++ Data Type:
| | |
|---| --- |
| `char` | 1 byte |
| `short` | 2 bytes |
| `int` | 4 bytes | 
| `long` | 8 bytes (`C++20`), >= 4 bytes |
| `long long` | 8 bytes |
| `float` | 4 bytes |
| `double` | 8 bytes |
| `void*` | 8 bytes (64 bits), 4 bytes (32 bits) |

## Functions

- Write formal parameters with `const` if possible.
- *Function* is a function **not** in a class. whereas *Method* is a function in a class.
- Don't frequently divide your code into functions, calling a function requires creating an entire stack frame. This means we have to push parameters and so on into the stack, and also pull something called the return address from the stack, so that after the function executed the `PC` (Program counter) register could return to the address before the function call.
  Conclusion: Jumping around in memory to execute function instructions comsumes additional time.

## Header Files

Prevent multiple inclusion: You include header file `b.h` in `a.cpp`, but `b.h` includes another header file `c.h`, while you have already include `c.h` before in `a.cpp`)

Two ways:
- `#pragma once`
- 
  ```c++ Log.h
  #ifndef _LOG_H
  #define _LOG_H

  // some sentence...

  #endif
  ```

## How to debug

Visual Studio
- The `watch view` in VS2015 allows you to specify the variables to be monitored,
  In `memory window` you can search by keyword `&a` to show the address of variable `a`
- The default value of uninitialized variables is `0xcccccccc`

## Visual Studio Setup

- Use `Show All Files` view under `Solution Explorer`.
- By default VS2015 put intermediate files in debug directory
- It's recommand to set `Output Directory` into `$(SolutionDir)bin\$(Platform)\$(Configuration)\`
  and set `Intermediate Directory` into `$(SolutionDir)bin\intermediate\$(Platform)\$(Configuration)\`

## Pointers

- The pointer represents a memory address, generally the data type of the pointer is used to represent the type of the data at the target address.
- Pointer to Pointer:
  ```c++
  #include <cstring>

  // Allocate a space with 8 chars
  char* buf = new char[8];
  std::memset(buf, 0, 8); // Fill it with zero,
  // Finally release the memory space.
  delete[] buf;
  
  // pointer to pointer
  char** ptr = &buf;
  ```
  In memory, it may like this:
  ```plaitext
  0x00B6FED4 (&buf):        00 D0 FF F0
  0x00D0FFF0 (new char[8]): 00 00 00 00 00 00 00 00
  0x???????? (&ptr):        00 B6 FE D4
  ```
  > Due to x86's little endian design, what we see from `Memory View` is start from lower bits to higer bits, which is reversed from human's convient that write the most significant digit first. e.g.:
  > ```plaintext
  >                    0x0 0x1 0x2 0x3
  > 0x00B6FED4 (&buf):  F0  FF  D0  00
  > ```

## Reference

- Variables are convert to memory address in assembly by compiler. Reference let compiler just substitute memory address as is (like a macro) using it's copy operator (`=()`), so it is different to `const void*` with creates a memory area to store a memory address (Can be `NULL`).
- Copy operator `=()` copies value of a variable. Send actual parameters to a function implicitly calls copy operator.
- Dereference operator (aka. indirection operator) `*()` get the value from a memory address.
- Address-of operator `&()` obtain the memory address of a variable. Since directly send variable name to `=()` only get its stored values.

## Classes vs Structs, and Enums

They are no different in low level, so a `class` can inherit a `struct` (Not recommand).
`sturct` is more suitable for store multiple variables, it's variables are default have attribute `public`, therefore it's convient to express a data structure. While Classes are more suitable for express objects, which has both member variables and methods.

Enum is a way to define a set of distinct values that have underlying integer types (can use `char` type as well, see below).

```c++
enum Example : unsigned char
{
    A, B, C
};
```

```c++
class Log {
// Access specifiers 'public', 'private' can be placed multiple times
public:
  enum Level {
    // Start from one, default is 0
    LevelError = 1, LevelWarning, LevelInfo
  };
private:
  Level m_log_lv = LevelInfo;
public:
  void set_lv(Level lv) {
    m_log_lv = lv;
  }
  void err(const char* msg) {
    if (m_log_lv >= LevelError) std::cout << "[Error]:" << msg << '\n';
  }
  void warn(const char* msg) {
    if (m_log_lv >= LevelWarning) std::cout << "[WARN]:" << msg<< '\n';
  }
  void info(const char* msg) {
    if (m_log_lv >= LevelInfo)
      std::cout << "[INFO]:" << msg<< '\n';
  }
};
int main() {
  Log log;
  log.set_lv(Log::LevelWarning); // Enum name is optional
  log.err("Hello");
  log.warn("Hello");
  log.info("Hello");
}
```

## Static

- Static functions and variables outside of class limit the scope in its translation unit (like `private` in a class). But define static functions or variables that share a same name in different translation unit will causes duplicate definition error in linking stage.
- Static variable inside a class or struct means that variable is going to share memory with all of the instances of the class, in other words there's only one instance of that static variable. Static methods cannot pass an instance of a class.
- Define `static` variables in header files if possible.
- Local Static
  ```c++
  void func() {
    static int s_i = 0; // 's_' means static
    s_i++;
    std::cout << s_i << '\n';
  }
  int main() { func(); func(); func(); func(); }
  ```

### Classical Singleton

Singleton are claess that allows only one instance.
- One way:
  ```c++
  class Singleton {
  private:
    static Singleton* s_instance;
      Singleton() {}; // Private empty construct function prevents instantiate
  public:
    static Singleton& get() { return *s_instance; };
    void hello() { std::cout << "Hello" << '\n'; };
  };
  Singleton* Singleton::s_instance = nullptr;
  int main() { Singleton::get().hello(); }
  ```
- Another way:
  ```c++
  class Singleton {
  private:
    Singleton() {};
  public:
    static Singleton& get() {
      // Put instace into a local static variable (Pro: less code)
      static Singleton s_instance;
      return s_instance;
    };
    void hello() { std::cout << "Hello" << '\n'; };
  };
  int main() { Singleton::get().hello(); }
  ```

## Constructors & Destructors

Corstructor provides a way to initialize primitive types when creating instance. Otherwise you have to do it manually or they will be keep to whatever is left over in that memory.

To Prevent creating instance, you can set constructor as priavate, or delete it.
```c++
class Log {
Private:
    Log() {} // One way
public:
    Log() = delete; // Another way
    static void write() {}
};
int main() {
    Log::write(); // Only Write() can be invoke
    Log l; // Now you cannot access the constructor
}
```

Destructor provides a way to gently recycle memory when `delete` an object.
It can be called directly: `SomeClass.~SomeClass();`

## Inheritance & Virtual Functions

Size of sub class: (base class) + (defined variables)

Why we need virtual function:
```c++
class Entity {
public:
    std::string get_name() { return "Entity"; }  
};
// public inheritance keep public members in base class public in sub class,
// otherwise they becomes to private
class Player : public Entity {
private:
  std::string m_Name;
public:
  Player(const std::string& name) : m_Name(name) {}
  std::string get_name() { return m_Name; }
};
int main() {
  Entity* entity = new Entity();
  std::cout << entity->get_name() << '\n';

  Player* player = new Player("Cherno");
  Entity* entity2 = player; // Cast player to Entity
  std::cout << entity2->get_name() << '\n';
}
```
Outputs:
```console
Entity
Entity
```
The problem occurs that the second output should be "Cherno"
When the pointer type is the main class `Entity`, the method `get_name()` uses it's main class' version even it's actually an instance of `Player`, this definitely a problem.
If you want to override a method you have to mark the method in the base class as `virtual`. Correct version:
```c++
class Entity {
public:
    // with 'virtual' marked
    virtual std::string get_name() { return "Entity"; }  
};
class Player : public Entity {
private:
    std::string m_Name;
public:
    Player(const std::string& name) : m_Name(name) {}

    // 'override' is not necessary but it could avoid typo and imporve readability
    std::string get_name() override { return m_Name; }
};
int main() {
    Entity* entity = new Entity();
    std::cout << entity->get_name() << '\n';

    Player* player = new Player("Cherno");
    Entity* entity2 = player;
    std::cout << entity2->get_name() << '\n';
}
```

> `virtual` could reduce "dynamic dispatch" (change object's vtable in runtime).
> `virtual` has its own overhead, it needs extra Vtable space, in order to dispatch the correct method it includes a member pointer in the base class that points to the vtable. And every time we call virtual method, we go through that table to decision which method to map.
> Through the extra overhead it's still recommand to use as much as possible.

### Interface (Pure Virtual Method)

```c++
// Interface class cannot be instantiated directlly
class Printable {
public:
    // interface (pure virtual method)
    virtual std::string get_class_name() = 0;
};
class Player : public Entity, Printable { // a sub class can inherit multiple interface class
public:
    std::string get_class_name() override { return "Player"; }
};
void print(Printable* obj) {// 通过这样的函数, 我们只需要实现这个方法, 该函数就会调用我们的实现
    std::cout << obj->get_class_name() << std::endl;
}
int main() {
    Player* player = new Player();
    print(player);
    print(new Player); // don't do this, it's very easily to cause memory leak
}
```

## Visibility in C++

- The default visibility of a Class would be `private`; if its a `struct` then it would be `public` by default.
- `private` things only visiable in it's own class, nor in sub class, except friend class.
- `protected` things can be seen by sub class.

## Raw Arrays && C++11 Standard Arrays

> Be aware for operations out of index (e.g.`example[-1] = 0`), in C++ you can do this by force the compiler to ignore this check, and it's definitely discouraged.

```c++
// 'arr' is actually a pointer of type int[5], stores the begin address of the array
int arr[5]; // On heap, destory on function end
int* arr = new int[5]; // on stack, would not auto destory
int* ptr = arr;
for (int i = 0; i< 5; i++)
  arr[i] = 2;

arr[2] = 5;
// this is equals to
*(ptr + 2) = 5;
// Bacause 'ptr' has type 'int' (32 bits, 4 bytes),
// so + 2 let it advance two 'int's length (64 bits, 8 bytes)
// *(int*)((char*) ptr + 8) = 5;
// It is pretty wild of this code.

// You cannot dynamically chack a raw array's size.
// Ways like this is unstable
int count = sizeof(arr) / sizeof(int);

// a good ways is to use an constant to remember the array size,
// or using c++11 standard arrays instead.
static const int arr_size = 5;
int arr[arr_size];

// Use square brackets to release an array on stack
delete[] arr;
```

- Standard Arrays
  It's more safe, but has little bit more overhead.
  ```c++
  #include <array>
  
  int main() {
    std::array<int, 5> arr;
    for (int i = 0; i < arr.size(); i++)
      arr[i] = 2;
  }
  ```

## How Strings Work (and How to Use Them)

### C-style Strings (Sring Literals)

C-style strings are store in code segment (virtual address space, which is read-only), this means you can only replace new string to the variable to "change" it.
```c++
const char* name = "Cherno";
"Cherno" + " hello!"; // You cannot do '+()' to raw strings since it's constant
```

> Each strings at end has `\0` (named "null termination character") to prevent out of index at iteration. e.g. `char str[7] = {'C', 'h', 'e', 'r', 'n', 'o', '\0'};`
> Terminal character will actuall break the behavior of string in many cases
> ```c++
> const char name[8] = "Che\0rno";
> std::cout << strlen(name) << std::endl;
> ```

### std::string

It's a char array indeed.

```c++
#include <iostream>

int main() {
  std::string str = "Cherno";
  str += " hello!"; // '+=()' is overloaded in the string class to let you do that.
  std::string str = std::string("Cherno") + " hello!"; // This does more object copy operations
  std::cout << str;
  // Check whether a string has specific word
  bool const contains = name.find("no") != std::string::npos
}
```

```c++
int main() {
    const char* name = u8"Cherno"; // utf-8, which is default
    const char16_t* name2 = u"Cherno"; // utf-16, two bytes per character
    const char32_t* name3 = U"Cherno"; // utf-32, four bytes per character
    const wchar_t* name4 = L"Cherno"; // eitger 2 or 4 bytes, depends on compiler
    // Useful or you want to keep format
    const char* rawstring = R"(Line1
Line2
Line3
Line4)";

    //C++14 Standards
    using namespace std::string_literals;
    std::string name0 = "Cherno"s + " hello";
    std::wstring name1 = L"Cherno"s + L" hello";
    std::u32string name1 = U"Cherno"s + U" hello";

}
```

## const in C++

`const`s does not have any memory address, they are replaced by primitive value at compile time.

### const pointer


```c++
const int MAX_AGE = 90;

// 1. Pointer to const value
const int* a = new int;
*a = 2; // I cannot change the contents of the pointer
a = (int*) &MAX_AGE; // But I can change the pointer itself

// 2. Const pointers
int* const b = new int;
*b = 2;
b = (int*) &MAX_AGE; // but I cannot change the pointer's value

// 3. Const pointer to a const value
const int* const c = (int*) &MAX_AGE;
```

> `const int*` equals `int const*`

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

使用构造初始化列表可以避免使用 "=" 从而实列化两次对象

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
};

void PrintEntity(const Entity& entity)
{
    // Printing Statements
}

int main()
{
    // It's weird , you can't do this in other languages(such as C# or Java)
    // This is called implicit conversion
    // It implicit converting "Cherno" into Entity's Constructor Method: Entity(const std::string& name)
    Entity a = std::string("Cherno");
    // 虽然上面是个很好的隐式转换的例子, 但是不建议用这种语法实例化对象


    // You cannot do implicit conversion with explicit method anymore
    // This is not allowed
    Entity b = 22;
    // Correct sentence:
    Entity b = Entity(22);
    

    // This is not allowed
    Entity a = "Cherno";
    // and
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

## OPERATORS and OPERATOR OVERLOADING in C++

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
   1. `std:tuple` (Can return more than two elements)
      ```C++
      #include <tuple>

      std::tuple<std::string, int> CreatePerson()
      {
          return { "Cherno", 24 }; // Implicit conversation
          // Or use "std::make_pair()":
          // return std::make_pair("Cherno", 24);
      }

      int main()
      {
          // 'auto' means automatically check the return type
          auto person = CreatePerson();
          // Get values in std::tuple
          std::string& name = std::get<0>(person);
          int age = std::get<1>(person);
      
          // Or using std::tie()
          std::string name2;
          int age2;
          std::tie(name2, age2) = person;
      }
      ```
   2. `std::pair` (A little bit faster than tuple)
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

Ignore...

## Macros in C++

1. Macros do text replace when preprocessor
   ```C++
   #define WAIT std::cin.get()

   int main()
   {
       WAIT;
   }
   ```
2. Macros function and combine with the environment
   Environment variables can be defined at:
   Open your project settings:
   C/C++ -> Preprocessor -> Preprocessor Definitions
   ```C++
   #ifdef PR_DEBUG
   #define LOG(x) std::cout << x << std::endl
   #elif defined(PR_RELEASE)
   #define LOG(x) // Do nothing
   #endif
   
   int main()
   {
       LOG("Hello");
   }
   ```

## The "auto" keyword in C++

1. be careful with `auto`
```C++
std::string GetName()
{
    return "Cherno";
}

int main()
{
    auto name = GetName();
    // Call a type specific method
    // if the return type of GetName() changed to "char*" , this will be broken
    int a = name.size();
    std::cout << a << std::endl;
}
```
1. `auto`' to reduce type length
   ```C++
   class DeviceManager
   {
   private:
       std::unordered_map<std::string, std::vector<Device*>> m_Devices;
   public:
       const std::unordered_map<std::string, std::vector<Device*>>& GetDevices() const
       {
           return m_Devices;
       }
   }

   int main()
   {
       DeviceManager dm;
       // The type is too mass
       const std::unordered_map<std::string, std::vector<Device*>>& devices = dm.GetDevices();
       // You can use keyword "using" or "typedef"
       using DeviceMap = std::unordered_map<std::string, std::vector<Device*>>;
       const DeviceMap& devices2 = dm.GetDevices();
       // Or auto
       const auto& devices3 = dm.GetDevices();
   }
   ```

## Static Arrays in C++ (std::array)

Ignore...

## Function Pointers in C++

1. 3 ways to definite a Function Pointers
   ```C++
   void HelloWorld(int a)
   {
       std::cout << "Hello World! Value: " << a << std::endl;
   }
   
   int main()
   {
       auto function = HelloWorld();

       // second way:
       // void(*function)(int) = HelloWorld();

       // third way:
       // typedef void(*Balabala)(int);
       // Balabala function = HelloWorld;
   
       function(233);
   }
   ```
2. A simple usage - the ForEach function:
   ```C++
   void PrintValue(int value)
   {
       std::cout << "Value: " << value << std::endl;
   }

   void ForEach(const std::vector<int>& values, void(*func)(int))
   {
       for(int value : values)
           func(value);
   }

   int main()
   {
       std::vector<int> values = { 1, 5, 4, 2, 3 };
       ForEach(values, PrintValue);
       // We can use lambda to simply the function PrintValue() (see more in next episode)
       // ForEach(values, [](int value) { std::cout << "Value: " << value << std::endl; })
   }
   ```

## Lambdas in C++

1. How to put outside variables into lambda function
   [=] : Pass everything in by value, the pass in variables is independent of the outside.
   [&] : Pass everything in by reference.
   [a] : Pass 'a' by value
   [&a] : Pass 'a' by reference.
2. using `mutable` keyword to allow modify outside variables
```C++
int main()
{
    int a = 5;
    auto lambda = [=]() mutable { a = 5; std::cout << "Value: " << a << std::endl; };
}
```
3. We need to use std::function instand of raw function pointer if our callback lambda function have pass in variables.
   ```C++
   #include<functional>

   // Here is the only difference (Use "const &" because its an object)
   // "void(*func)(int)" to "const std::function<void(int)>& func"
   void ForEach(const std::vector<int>* values, const std::function<void(int)>& func)
   {
       // ...
   }

   int main()
   {
       // ...
       int a = 5;
       auto lambda = [=](int value)) {std::cout << "Value: " << a << std::endl;

       ForEach(values, lambda};

   }
   ```
4. Usage of std::find_if (Returns an iterator to the first element for which callback function returns true)
   ```C++
   std::vector<int> values = { 1, 5, 4, 2, 3 };
   auto iterator = std::find_if(values.begin(), values.end(), [](int value) { return value > 3;});
   std::cout << *iterator << std::endl;
   ```

## Why I don't "using namespace std"

Don't absolutely use `using namespace` in header files
But if you must using `using namespace`, please use it in a small scope as possible, but NEVER EVER in a header file.

For example a serious issue of implicit conversion :
```C++
namespace apple
{
    void print(cout std::string& text)
    {
        std::cout << text << std::endl;
    }
}

namespace orange
{
    void print(const char* text)
    {
        std::string temp = text;
        std::reverse(temp.begin(), temp.end());
        std::cout << temp << std::endl;
    }
}

using namespace apple;
using namespace orange;

int main()
{
    print("Hello");
    // Which one will get called?
    // Answer: the orange::print() will be called, because the type of "Hello" is "char*"
}
```

## Namespaces in C++

Use Namespace to
1. avoid naming conflict: `apple::print()`, `orange::print()`
2. avoid C library like naming: `GLFW_initialize` to `GLFW::initialize`

Usage:
1. We can set to use only specific symbol in a namespace
   ```C++
   namespcae apple {
       void print(const char* text)
       {
           //...
       }
       void print_again()
       { 
           ///...
       }
   }
   
   int main()
   {
       using apple::print;
       print("Hello");
       
       //We still need "apple::" to call print_again()
       apple::print_again();
   }
   ```
2. Nested Namespaces can be shorten using Alias
   ```C++
   namespace apple { // Or apple::functions (c++17)
       namespace functions {
           void print(const char* test)
           {
               // ...
           }
       }
   }

   int main()
   {
       namespace a = apple::functions;

       a::print("Hello");
   } 
   ```

## Threads in C++

If we want to do something else when we called functions that will block the current thread, we can use threads.

Here is an example:
We created a thread that will do loop on outputting "Working..",
and simultaneously the main() function is waiting for user input.
```C++
#include <iostream>
#include <thread>

static bool is_Finished = false;

void DoWork()
{
    using namespace std::literals::chrono_literals; // Or use "sleep_for(std::chrono::seconds(1))" below

    std::cout << "Started thread id=" << std::this_thread::get_id() << std::endl;

    while (!is_Finished)
    {
        std::cout << "Working...\n";

        std::this_thread::sleep_for(1s);
    }
}

int main()
{
    // As soon as we create instance, it's going to immediately kick off that thread
    std::thread worker(DoWork);

    std::cin.get();
    is_Finished = true;

    // Call main thread to wait this thread (block main thread)
    worker.join();
    std::cout << "Finished." << std::endl;
}
```

## Timing in C++

1. Make a Timer for statistical time-consuming
```C++
struct Timer
{
    std::chrono::time_point<std::chrono::steady_clock> start, end;
    std::chrono::duration<float> duration;

    Timer()
    {
        start = std::chrono::high_resolution_clock::now();
    }

    ~Timer()
    {
        end = std::chrono::high_resolution_clock::now();
        duration = end - start;

        float ms = duration.count() * 1000.0f;
        std::<< "Timer took " << ms << "ms " << std::endl;
    }
}

void Function()
{
    // The timer will be auto delete when run out of the scope
    Timer timer;

    for (int i = 0; i < 100; i++)
        std::cout << "Hello\n"; // std::endl is quiet slow
} 

int main()
{
    Function();
}
```

## Multidimensional Arrays in C++

```C++
int main()
{
    // Allocate a 2D array to store twenty integer pointers
    int** a2d = new int*[20];

    // We can set each of these pointers to point to an array
    for (int i = 0; i < 20; i++)
        a2d[i] = new int[50];
    
    // 3D array, you can imagine a cube with size of 20x30x40
    int*** a3d = new int**[20];
    for (int i = 0 i < 20; i++)
    {
        a3d[i] = new int*[30];
        for (int j = 0; j < 20; j++)
        {
            a3d[i][j] = new int[40]:
        }
            
    }

    a3d[0][0][0] = 666;

    // When you want to delete this array, you have to go through inner array and delete all of those arrays from inside to out
    for(int i = 0; i < 20; i++)
    {
        for (int j = 0; j < 30; j++)
        {
            delete[] a3d[i][j];
        }
        delete[] a3d[i];
    }
    delete[] a3d;
}
```

The most issue is that the Multidimensional Arrays will results memory fragmentation.
When iterating the array we have to jump to another location to read or write that data,
and that's results probably a cache miss which means that we're wasting time fetching data from our actual RAM.

One of the most important things you can do is just store them in a single dimensional array:

```C++
int main()
{
    int* array =new int[5 * 5];
    for (int x = 0; x < 5; y++)
    {
        for (int y = 0; y < 5; x++)
        {
            array[x * 5 + y] = 2;
        }
    }
}
```

## Sorting in C++

```C++
#include <algorithm>

int main()
{
    std::vector<int> values = { 3, 5, 1, 4, 2 };
    std::sort(values.begin(), values.end());

    // Output the results
    for (int value : values)
        std::cout << value << std::endl;

    // We can define how the sorting rule the sort() dose
    // Such like sorting by big to small
    std::sort(values.begin(), values.end(), [](int a, int b)
    {
        return a > b;
    });

    // Output the results
    for (int value : values)
        std::cout << value << std::endl;

    // Or force '1' to the last of array list
    std::sort(values.begin(), values.end(), [](int a, int b)
    {
        // Return true equals do exchange the position of a, b
        if(a == 1)
            return false;
        if(b == 1)
            return true;

        return b < a;
    });
}
```

## Type Punning in C++


You can Treat an Entity struct as an int array:

```C++
struct Entity
{
    int x, y;
}

int main()
{
    Entity e = { 5, 8 };
    
    int* position = (int*)&e;
    std::cout << position[0] << ", " << position[1] << std::endl;

    // More crazy usage
    int y = *(int*)((char*)&e + 4);
    std::cout << y << std::endl;
}
```

## Unions in C++

Defined member in one Union means same memory location

We have Vector2 and Vector2, but there is only one function `PrintVector2()` can output Vector2,
what can we do?

```C++
struct Vector2
{
    float x, y;
};

void PrintVector2(const Vector2& vector)
{
    std::cout << vector.x << ", " << vector.y << std::endl;
}

struct Vector4
{
    union
    {
        // The benefit of anonymous struct is converting all variables in struct into a single member which is what the Union expects
        struct
        {
            float x, y, z, w;
        };
        struct
        {
            // 'zy' will be the same memory as 'x, y', and 'zw' will be the same memory as 'z, w' 
            Vector2 xy, zw;
        };
    };
};

int main()
{
    Vector4 vector = { 1.0f, 2.0f, 3.0f, 4.0f };
    PrintVector2(vector.xy);
    PrintVector2(vector.zw);
    std::cout << "------------------" << std::endl;
    vector.z = 500.0f;
    PrintVector2(vector.xy);
    PrintVector2(vector.zw);
}
```

Type Punning can do as the same result, but using Union makes it more concise.

## Virtual Destructors in C++

Virtual Destructors is really important if you are writing a father class,
otherwise no one's going to be able to safely delete the extend class
(Because without `virtual` mark you are just adding a new Destructor instead of overload it)

```C++
class Base
{
public:
    Base() { std::cout << "Base Constructor\n"; }
    virtual ~Base() { std::cout << "Base Destructor\n"; }
};

class Derived : public Base
{
public:
    Derived() { std::cout << "Derived Constructor\n"; }
    ~Derived() { std::cout << "Derived Destructor\n"; }
};

int main()
{
    // Polymorphic kind of type
    // without `virturl`, the "~Derived()" will not be called
    Base* poly = new Derived();
    delete poly;
}
```

## Casting in C++

C++ cast not do anything that C-style casts cannot do
those casts do make you code more solid and looks better.

1. Static cast (Will do compile time checking)
2. Interpret cast (for Type Punning)
3. Dynamic cast (Will return `NULL` if casting is failed)
4. Const cast (TODO: Supplement this content)

```C++
class Base
{
public:
    Base() { }
    virtual ~Base() { }
};

class Derived : public Base
{
    // ...
};

int main()
{
    // Static cast
    double value = 5.5;
    double s = static_cast<int>(value);

    Base* base = new Base();
    Derived* ac = dynamic_cast<Derived*>(base);
    if (!ac) { std::cout << "Converting failed\n"; }
}
```

## Conditional and Action Breakpoints in C++

Condition Breakpoints: If I only want the breakpoint to trigger under a certain condition
Action Breakpoints: Generally print something to the console when a breakpoint is hit
They can prevent recompile and save time

For details. please [watch the video](https://www.youtube.com/watch?v=9ncNA6Co2Nk&list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb&index=70)

## Safety in modern C++ and how to teach it

You should 100% use smart pointers if you are doing serious work

## Precompiled Headers in C++

Look to 7:07

## Dynamic Casting in C++

If we force type casting a Enemy class to Player and access data(funcions, variables) that is unique to player, the program will probablly crash.

Dynamic Casting is actually does some validation for us to ensure that cast is valid
```C++
class Entity
{
};

class Player : public Entity
{
};

class Enemy : public Entity
{
};

int main()
{
    Entity* actuallyPlayer = new Player();
    Entity* actuallyEnemy = new Enemy();

    // How does it know that actuallyPlayer is actually a Player and not an Enemy?
    // The way it does that is it stores runtime type information(RTTI)
    // This does add an overhead but it lets you do things like dynamic casting
    // Be aware that RTTI can be enabled or disabled
    Player* p0 = dynamic_cast<Player*>(actuallyPlayer);
    
    // Will return null
    Player* p1 = dynamic_cast<Player*>(actuallyEnemy);

    // dynamic_cast<xxx*>(xxx); equal to xxx instanceof xxx
}
```

## BENCHMARKING in C++ (how to measure performance)

Always make sure that you profile is actually meaningful in a releases because you're not gonna be shipping code in debug anyway

```C++
class Timer
{
private:
    std::chrono::time_point<std::chrono::steady_clock> m_StartTime, m_EndTime;
public:
    Timer()
    {
        m_StartTime = std::chrono::high_resolution_clock::now();
    }

    ~Timer()
    {
        m_EndTime = std::chrono::high_resolution_clock::now();
        auto start = std::chrono::time_point_cast<std::chrono::microseconds>(m_StartTime).time_since_epoch().count();
        auto end = std::chrono::time_point_cast<std::chrono::microseconds>(m_EndTime).time_since_epoch().count();

        auto duration = end - start;

        double ms = duration * 0.001;
        std::cout << duration << "us (" << ms << "ms)\n" << std::endl;
    }
};

int main()
{
    struct Vector2
    {
        float x, y;
    };

    std::cout << "Make Shared\n";
    {
        std::array<std::shared_ptr<Vector2>, 1000> sharedPtrs;
        Timer timer;
        for (int i = 0; i < sharedPtrs.size(); i++)
            sharedPtrs[i] = std::make_shared<Vector2>();
    }

    std::cout << "New Shared\n";
    {
        std::array<std::shared_ptr<Vector2>, 1000> sharedPtrs;
        Timer timer;
        for (int i = 0; i < sharedPtrs.size(); i++)
            sharedPtrs[i] = std::shared_ptr<Vector2>(new Vector2());
    }

    std::cout << "Make Unique\n";
    {
        std::array<std::shared_ptr<Vector2>, 1000> sharedPtrs;
        Timer timer;
        for (int i = 0; i < sharedPtrs.size(); i++)
            sharedPtrs[i] = std::make_shared<Vector2>();
    }
}
```

## STRUCTURED BINDINGS in C++

Only in C++17 and newer
a better way compare to How to Deal with Multiple Return Values in C++

```C++
std::tuple<std::string, int> CreatePerson()
{
    return { "Cherno", 24 };
}

int main()
{
    // Structured binding
    auto[name, age] = CreatePerson();
    std::cout << name;
}
```

## How to Deal with OPTIONAL Data in C++

## Multiple TYPES of Data in a SINGLE VARIABLE in C++?

## How to store ANY data in C++

## How to make C++ run FASTER (with std::async)

```C++
#include <future>

std::vector<std::std::future<void>> m_Futures // void is the return type of the function LoadMesh()

static std::mutex s_MeshesMutex;

// Here we use pointer parameter because reference parameter doesn't work(Cherno not entirely sure why)
static void LoadMesh(const std::vector<Ref<Mesh>>* meshes, std::string filepath)
{
    auto mesh = Mesh::Load(filepath);

    // We need to lock this meshes vector while it been modify
    // lock the push_back() function
    std::lock_guard<std::mutex> lock(s_MeshesMutex);
    meshes->push_back(mesh);
}

void EditorLayer::LoadMeshes()
{
    // Get file context use c++ file stream
    std::ifstream stream("src/Models.txt");
    std::string line;
    std::vector<std::string> meshFilepaths;
    while (std::getLine(stream, line))
        meshFilepaths.push_back(line);

    // m_Meshes is a vector in this class
#define ASYNC 1
#if ASYNC
    // Asynchronous
    for (const auto& file : meshFilepaths)
    {
        // std::async() will return std::future and its really important to handle that std::future or its will be destoryed and peform the destructor to make sure the async is actually finished
        // that basically means it won't be parallel at all 
        m_Futures.push_back(std::async(std::launch::async, LoadMesh, &m_Meshes, file));
    }
#else
    // Sequence
    for (const auto& file : meshFilepaths)
    {
        m_Meshes.push_back(Mesh::Load(file));
    }
#endif
}
```

In VS, you can use DEBUG->Windows->Parrllel Stacks (Ctrl+Shift+D)
to do window parallel debugs

## How to make your STRINGS FASTER in C++!

```C++
```

## VISUAL BENCHMARKING in C++ (how to measure performance visually)

## SINGLETONS in C++

```C++
```

## Small String Optimization in C++

## Track MEMORY ALLOCATIONS the Easy Way in C++

```C++
```

## lvalues and rvalues in C++

In my opinion, any data that is not being attached by a symbol(not being explicitly assigned an address) is an R-value, otherwise it's an L-value.

1. L-value reference and R-value reference
   ```C++
   int i = 10;
   // L-value reference can accept L-value and R-value
   int& = i;
   // R-value reference can only accept R-value
   int&& = 10;
   ```
2. `const type&` is a special rule, realistically what happens is the compiler will probably create like a temporary variable.
   there are not to kind of avoid creating an L-value but rather to just kind of support both support both L-value and R-values
   ```C++
   const int& a = 10;
   // int temp = 10;
   // const int& a = temp;
   ```

## Continuous Integration in C++

## Static Analysis in C++

Static Analysis is a very important thing that even for an experienced programmer there is still going to be stuff what you miss.
It can find logic errors in your code and gives you some tips to fix it

`clang-tidy` is a free tool to do static analysis

## Argument Evaluation Order in C++

## Move Semantics in C++

```C++
class String
{
public:
	String() = default; // equals with String(){}

	String(const char* string)
	{
		printf("Created!\n");
		m_Size = strlen(string);
		m_Data = new char[m_Size];
		memcpy(m_Data, string, m_Size);
	}

	String(const String& other)
	{
		printf("Copied!\n");
		m_Size = other.m_Size;
		m_Data = new char[m_Size];
		memcpy(m_Data, other.m_Data, m_Size);
	}

	String(String&& older) noexcept // use noexcept to get rid of compiler warning
	{
		printf("Moved!\n");
		m_Size = older.m_Size;
		// This immediately presents a problem
		// because when the old one gets deleted
		// it's going to delete the m_Data with us
		m_Data = older.m_Data;
		// so this is the major thing that we need to do
		older.m_Size = 0;
		older.m_Data = nullptr;
	}

	~String()
	{
		printf("Destroyed!\n");
		delete m_Data;
	}

	void Print()
	{
		for (uint32_t i = 0; i < m_Size; i++)
			printf("%c", m_Data[i]);

		printf("\n");
	}
private:
	char* m_Data;
	uint32_t m_Size;
};

class Entity
{
public:
	Entity(const String& name)
		: m_Name(name)
	{
	}

	Entity(String&& name)
		: m_Name(std::move(name)) // equals to m_Name((String&&) name)
	{
	}

	void PrintName()
	{
		m_Name.Print();
	}
private:
	String m_Name;
};

int main()
{
	Entity entity("Cherno");
	entity.PrintName();
}
```

## std::move and the Move Assignment Operator in C++

`std::move` actually do force casting but can make your program more search friendly

Move Assignment will allow us do move operation on existing objects

```C++
class String
{
public:
	String() = default;

	String(const char* string)
	{
		printf("Created!\n");
		m_Size = strlen(string);
		m_Data = new char[m_Size];
		memcpy(m_Data, string, m_Size);
	}

	String(String&& older) noexcept
	{
		printf("Moved!\n");
		m_Size = older.m_Size;
		m_Data = older.m_Data;

		older.m_Size = 0;
		older.m_Data = nullptr;
	}

    // define move assignment
    String& operator=(String&& older) noexcept
    {
        if (this != &older)
        {
            // Because use move assignment meaning assure there already existing data in currently class
            // So clean the current class
            delete[] m_Data;
        
		    printf("Moved!\n");
		    m_Size = older.m_Size;
		    m_Data = older.m_Data;

		    older.m_Size = 0;
		    older.m_Data = nullptr;
        }

        return *this;
    }

	~String()
	{
		printf("Destroyed!\n");
		delete m_Data;
	}

	void Print()
	{
		for (uint32_t i = 0; i < m_Size; i++)
			printf("%c", m_Data[i]);

		printf("\n");
	}
private:
	char* m_Data;
	uint32_t m_Size;
};

int main()
{
    String apple = "Apple";
    String dest;

    std::cout<< "Apple: ";
    apple.Print();
    std::cout<< "Dest: ";
    dest.Print();

    dest = std::move(apple);

    std::cout<< "Apple: ";
    apple.Print();
    std::cout<< "Dest: ";
    dest.Print();
}
```

## ARRAY - Making DATA STRUCTURES in C++