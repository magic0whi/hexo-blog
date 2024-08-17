---
title: cpp
category: learn
date: 2020-02-08 16:45:08
tags:
toc: true
variant: cyberpunk
article:
  highlight:
    theme: github-dark
---

Notes on Cherno C++ Tutorial

<!-- more -->
<style>
.card:not(#back-to-top) .card-content::before {
  background-color: #1b1b1b;
}
article.article .content {
  font-family: KaTeX_Main, FZYaSongS-R-GB, serif;
  font-size: 18px;
  word-spacing: 2px;
}
article.article .content code, article.article .content pre {
  font-family: Iosevka Term Extended, monospace;
  font-size: 14px;
}
</style>

## Notice

1. If an object have pointer variable inside, write a copy constructor and use it.
2. Default float type is `double`, use `float a = 5.5f`.
3. Header or inline?  (the later copys whole function body into the area where the function is called).

## Assembly

```c++
int main(int const argc, char const* argv[]) {
  if (argc > 5) return 29;
  else return 42;
}
```

```asm
cmp edi, 5      // Compare argc to 5
mov edx, 42     // Loads 42 to edx
mov eax, 29     // Loads 29 to eax
cmovle eax, edx // Condition move, if the above cmp get false, move edx to eax
ret
```

## How C++ Works

* A translation unit consists of a single `.cpp` file and its associated `.h` files.
* Compile Process Path: Source&rarr;Compile&rarr;Linker&rarr;Executables

1. Pre-process
   Compiler parses all the macros (such as `#include`) in source file.
   - Clang: `clang -x c++ -E hello.cpp -o hello.i`
   - GCC: `cpp hello.cpp > hello.i`
   - VS2015: `Project Settings`->`Preprocessor`->`Preprocess to a File`
2. Compile & Assembly
   - Clang: `clang++ -x c++-cpp-output -S hello.i -o hello.o`
   - GCC: `g++ -S hello.i && as hello.s -o main.o`
   - VS2015: Compile Only (`<C-F7>`)
3. Linker
   Externally defined functions will be integrated in the link phase. Function declarations that never be called will be optimized away.
   
   The parameter of `ld` are platform specific (mainly depends on Clang/GCC version). Enable verbose to get the parameter of the `collect2` (which is an alias of `ld`):
   For Clang:
   ```console
   # # Only run preprocess, compile, and assemble steps
   $ clang++ -v -c hello.cpp -o hello.o
   # # For GCC:
   $ g++ -v -c hello.cpp -o hello.o
   ```
   Ld may looks messy, but you can significantly shorten that link line by removing some arguments. Here's the minimal set I came up after some experimentations:
   ```console
   $ ld -I/lib64/ld-linux-x86-64.so.2 \
   -o hello \
   /usr/lib64/Scrt1.o \
   -L/usr/lib64/gcc/x86_64-pc-linux-gnu/14.2.1 \
   hello.o \
   -l stdc++ -l m -l gcc_s -l gcc -l c \
   /sbin/../lib64/gcc/x86_64-pc-linux-gnu/14.2.1/crtendS.o \
   /usr/lib64/crtn.o
   ```

## Primitive Types (Fundamental Type)

- The difference between C++ data types are simply different allocated memory size.
- You can use `sizeof()` to see the data type size.
- Trivial type: either a primitive type or a type composed of only trivial types.
  trivial type can be copied and moved with `memcpy`, `memmove` and constructed destructed without doing anything. Can be checked using `std::is_trivial<Type>()`.

Different memory allocation size for C++ Data Type:
| | |
|---| --- |
| `char` | 1 byte |
| `short` | 2 bytes |
| `int` | 4 bytes |
| `long` | 8 bytes (`c++20`), >= 4 bytes |
| `long long` | 8 bytes |
| `float` | 4 bytes |
| `double` | 8 bytes |
| `void*` | 8 bytes (64 bits), 4 bytes (32 bits) |

## Functions

- Write formal parameters with `const` if possible.
- *Function* is a function **not** in a class. whereas *method* is a function in a class.
- Don't frequently divide your code into functions, calling a function requires creating an entire stack frame. This means we have to push parameters and so on into the stack, and also pull something called the return address from the stack, so that after the function executed the `PC` (Program counter) register could return to the address before the function call.
  Conclusion: Jumping around in memory to execute function instructions comsumes additional time.

## Header Files

Duplicate inclusion: You include header file `b.h` in `a.cpp`, but `b.h` includes another header file `c.h`, while you have already include `c.h` before in `a.cpp`)

Two ways:
- `#pragma once`
- 
  ```c++ Log.h
  #ifndef _LOG_H
  #define _LOG_H

  // some sentence...

  #endif
  ```

## Visual Studio Setup & Debug

- Use `Show All Files` view under `Solution Explorer`.
- By default VS2015 put intermediate files in debug directory
- It's recommand to set `Output Directory` into `$(SolutionDir)bin\$(Platform)\$(Configuration)\`
  and set `Intermediate Directory` into `$(SolutionDir)bin\intermediate\$(Platform)\$(Configuration)\`

- The `watch view` in VS2015 allows you to specify the variables to be monitored,
  In `memory window` you can search by keyword `&a` to show the address of variable `a`
- The default value of uninitialized variables is `0xcccccccc`

## Pointers

- The pointer represents a memory address, generally the data type of the pointer is used to represent the type of the data at the target address.
- Pointer to Pointer:
  ```c++
  #include <cstring>
  int main() {
    char* buf{new char[8]}; // Allocate a space with 8 chars
    std::memset(buf, 0, 8); // Fill it with zero,
    char** ptr_to_ptr{&buf}; // pointer to pointer
    delete[] buf; // Finally release the memory space.
  }
  ```
  In memory, it may like this:
  ```plaitext
  0x00B6FED4 &buf:           00 D0 FF F0
  0x00D0FFF0 *(new char[8]): 00 00 00 00 00 00 00 00
  0x???????? &ptr_to_ptr:    00 B6 FE D4
  ```
  > Due to x86's little endian design, what we see from `Memory View` is start from lower bits to higer bits, which is reversed from human's convient that write the most significant digit first. e.g.:
  > ```plaintext
  >                    0x0 0x1 0x2 0x3
  > 0x00B6FED4 (&buf):  F0  FF  D0  00
  > ```

## Reference

- Variables are convert to memory address in assembly by compiler. Reference let compiler just substitute memory address as is (like a macro) using it's copy operator (`=()`), so it is different to `const void*` with creates a memory area to store a memory address (Can be `NULL`).
- Copy operator `=()` copies value of a variable. Send the actual parameters to a function implicitly calls copy operator.
- Dereference operator (aka. indirection operator) `*()` get the value from a memory address.
- Address-of operator `&()` obtain the memory address of a variable. Since directly send variable name to `=()` only get its stored values.

## Classes vs Structs, and Enums

They are no different in low level, so a `class` can inherit a `struct` (Not recommand).

`sturct` is more suitable for store multiple variables, its variables are default have attribute `public`, therefore it's convient to express a data structure. While `class` is more suitable for express object, which has both member variables and methods.


Struct Bit-fields
```c++
struct S {
  unsigned int a : 1 {0}; // Default value are allowed in C++20
  unsigned int b : 8 = 'a';
  int c : 23;
  long e : 32;
};
int main() {
  std::println("Size of Sttucture with Bit Fields: {}", sizeof(S)); // Bit-field size: 1 + 7 + 24 + 32 = 64 (12 bytes), depends on the largest primitive type in structure
}
```

Enum is a way to define a set of distinct values that have underlying integer types (, see below).

```c++
#include <iostream>
#include <string_view>
class Log {
// Access specifiers 'public', 'private' can be placed multiple times
public:
  // Can use `char` type as well
  // enum Level : unsigned char
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
  void err(const std::string_view msg) const {
    if (m_log_lv >= LevelError) std::cout << "[Error]:" << msg << '\n';
  }
  void warn(const std::string_view msg) const {
    if (m_log_lv >= LevelWarning) std::cout << "[WARN]:" << msg<< '\n';
  }
  void info(const std::string_view msg) const {
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

> `using enum` to create alias for `enum`

## Static

- Static functions and variables outside of class limit the scope in its translation unit (like `private` in a class). But define static functions or variables that share a same name in different translation unit will cause duplicate definition error in linking stage.
- Static variable inside a class or struct means that variable is going to share memory with all of the instances of the class, in other words there's only one instance of that static variable. Static methods cannot access non-static members its class, hence it don't have instance of the class.
- Define `static` variables in header files if possible.
- Local Static
  ```c++
  #include <iostream>
  void func() {
    static int s_i{}; // 's_' means static
    s_i++;
    std::cout << s_i << '\n';
  }
  int main() { func(); func(); func(); func(); }
  ```

### Classical Singleton

Singleton are classes that allow only one instance.
- One way:
  ```c++
  #include <iostream>
  class Singleton {
  private:
    static Singleton* s_instance;
    Singleton() {}; // Private empty construct function prevents instantiate
  public:
    static Singleton& get() { return *s_instance; };
    void hello() { std::cout << "Hello" << '\n'; };
  };
  // Static members in class shoud defined out-of-line, but here I given a nullptr to pass static analyze
  Singleton* Singleton::s_instance{nullptr};
  // Though no memory spaces created for the class, we can still access methods
  int main() { Singleton::get().hello(); }
  ```
- Another way:
  ```c++
  #include <iostream>
  class Singleton {
  private:
    Singleton() {};
  public:
    static Singleton& get() {
      static Singleton s_instance; // Put instance into a local static variable (Pro: less code)
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
private:
  Log() {} // One way
public:
  // Log() = delete; // Another way
  static void write() {}
};
int main() {
  Log::write(); // Only Write() can be invoke
  // Log l; // Now you cannot access the constructor
}
```

Destructor provides a way to gently recycle memory when `delete` an object.
It can be called directly: `SomeClass.~SomeClass();`

## Inheritance & Virtual Functions

- Size of derived class (aka. sub class): (base class) + (defined variables)
- Derived class implicitly call base class's constructor

Why we need virtual function:
```c++
#include <iostream>
#include <string>
class Entity {
public:
  std::string get_name() { return "Entity"; }  
};
// public inheritance keep public members in base class public in derived class,
// otherwise they becomes to private
class Player : public Entity {
private:
  std::string m_name;
public:
  Player(const std::string& name) : m_name{name} {}
  std::string get_name() { return m_name; }
};
int main() {
  Entity* entity{new Entity};
  std::cout << entity->get_name() << '\n';

  Player* player{new Player{"Cherno"}};
  Entity* entity2{player}; // Cast player to Entity
  std::cout << entity2->get_name() << '\n';
}
```
Outputs:
```console
Entity
Entity
```
The problem occurs that the second output should be "Cherno". When the pointer type is the main class `Entity`, the method `get_name()` uses it's main class' version even it's actually an instance of `Player`, this definitely a problem.

If you want to override a method you have to mark the method in the base class as `virtual`. Correct version:
```c++
#include <iostream>
class Entity {
public:
  virtual std::string get_name() { return "Entity"; } // with 'virtual' marked
};
class Player : public Entity {
private:
  std::string m_name;
public:
  Player(const std::string& name) : m_name{name} {}
  // 'override' is not necessary but it could avoid typo and imporve readability
  std::string get_name() override { return m_name; }
};
int main() {
  Entity* entity{new Entity};
  std::cout << entity->get_name() << '\n';

  Player* player{new Player{"Cherno"}};
  Entity* entity2{player};
  std::cout << entity2->get_name() << '\n';
}
```

> `virtual` could reduce "dynamic dispatch" (change object's vtable in runtime).
> `virtual` has its own overhead, it needs extra Vtable space, in order to dispatch the correct method it includes a member pointer in the base class that points to the vtable. And every time we call virtual method, we go through that table to decision which method to map.
> Through the extra overhead it's still recommand to use as much as possible.

### Interface (Pure Virtual Method)

```c++
#include <iostream>
#include <string>
class Entity {};
class Printable { // Interface class cannot be instantiated directlly
public:
  virtual std::string get_class_name() = 0; // interface (pure virtual method)
};
class Player : public Entity, public Printable { // a sub class can inherit multiple interface class
public:
  std::string get_class_name() override { return "Player"; }
};
// Now even the Player instance is casted into its base class, it will still use sub class's function implement
void print(Printable* obj) { std::cout << obj->get_class_name() << '\n'; }
int main() {
  Player* player{new Player};
  print(player);
  print(new Player); // don't do this, it's very easily to cause memory leak
}
```

## Visibility in C++

- The default visibility of a Class would be `private`. If it's a `struct` then it would be `public` by default.
- `private` things only visiable in it's own class, nor in sub class, except friend class.
- `protected` things can be seen by sub class.

## Literal Arrays & C++11 Standard Arrays

> Be aware for operations out of index (e.g. `example[-1] = 0`), in C++ you can do this by force the compiler to ignore this check, and it's definitely discouraged.

- Literal Array
  ```c++
  #include <iostream>
  int main() {
    int arr[5]; // On stack, destory on function end
  
    int* arr2{new int[5]}; // on heap, would not auto destory
    delete[] arr2; // Use square brackets to release an array on heap
  
    int* ptr{arr}; // 'arr' is actually a pointer which stores the begin address of the array
    for (int i = 0; i< 5; i++) arr[i] = i;
    
    arr[2] = 5; 
    *(ptr + 2) = 5; // this is equal but in a pretty wild way
    // Bacause 'ptr' has type 'int' (32 bits, 4 bytes),
    // so + 2 let it advance two 'int's length (64 bits, 8 bytes)
    // We can also do the same operation in this form (`char` is 8 bits, 1 bytes)
    *(int*) ((char*) ptr + 8) = 5;
  
    for (int i = 0; i< 5; i++) std::cout << arr[i] << '\n';
    
    // You cannot dynamically chack a raw array's size.
    int count{sizeof(arr) / sizeof(int)}; // Ways like this is unstable
    
    // A good ways is to use an constant to remember the array size,
    // or using c++11 standard arrays instead.
    static const int&& arr_size{5};
    int arr3[arr_size];
  }
  ```
- Standard Arrays
  It's more safe, but has little bit more overhead.
  ```c++
  #include <array>
  int main() {
    std::array<int, 5> arr;
    for (int i = 0; i < arr.size(); i++) arr[i] = 2;
  }
  ```

## How Strings Work (and How to Use Them)

### C-style Strings (String Literals) and std::string_view

C-style strings are stored in code segment (virtual address space, which is read-only), this means you can only replace new string to the variable to "change" it.
```c++
#include <cstring>
#include <string_view>
int main() {
  const char* name{"Cherno"};
  // "Cherno" + " hello!"; // You cannot do '+()' to literal strings since it's constant
  std::string_view name2{name}; // c++17, it's equivalent to 'const char*'
}
```

> Each strings at end has `\0` (named "null termination character") to prevent out of index at iteration. e.g. `char str[7] = {'C', 'h', 'e', 'r', 'n', 'o', '\0'};`
> Terminal character will actuall break the behavior of string in many cases, use `std::string_view` can prevent this problem.
> ```c++
> #include <cstring>
> #include <iostream>
> int main() {
>   char name[8]{"Che\0rno"};
>   std::cout << strlen(name) << std::endl;
> }
> ```

A sample implementation of `std::string_view`:
```c++
// An implementation of std::string_view
class StaticString {
  using const_iterator = char const*;
private:
  char const* const m_str;
  std::size_t const m_size;
public:
  template <std::size_t N>
  StaticString(char const (&str)[N]) noexcept : m_str{str}, m_size{N - 1} {}
  StaticString(char const* str, std::size_t N) noexcept : m_str{str}, m_size{N} {}
  const char* data() const noexcept { return m_str; }
  std::size_t size() const noexcept { return m_size; }
  const_iterator begin() const noexcept { return m_str; }
  const_iterator end() const noexcept { return m_str + m_size; }
  operator char const* const() const { return m_str; }
  char operator[](std::size_t n) const {
    // clang-format off
    return n == std::numeric_limits<size_t>::max()
      ? m_str[m_size - 1]
      : n < m_size ? m_str[n] : throw std::out_of_range(std::format("static_string: out of range, index at {}", n));
    // clang-format on
  }
};
inline std::ostream& operator<<(std::ostream& os, StaticString const& str) { return os.write(str.data(), str.size()); }
int main() {
  StaticString str{"homo114514"};
  try {
    str[-1];
    str[-2];
  } catch (std::out_of_range e) { std::cout << e.what() << '\n'; }
}
```

### std::string

It's a char array indeed.

```c++
#include <iostream>
int main() {
  std::string str{"Cherno"};
  str += " hello!"; // '+=()' is overloaded in the string class to let you do that.
  std::string str2{std::string{"Cherno"} + " hello!"}; // This does more object copy operations
  std::cout << str << '\n';
  const bool contains = str.find("no") != std::string::npos; // Check whether a string has specific word
  std::cout << contains << '\n';
}
```

```c++
#include <string>
int main() {
  const char8_t* name1{u8"Cherno"}; // utf-8, which is default
  const char16_t* name2{u"Cherno"}; // utf-16, two bytes per character
  const char32_t* name3{U"Cherno"}; // utf-32, four bytes per character
  const wchar_t* name4{L"Cherno"}; // either 2 or 4 bytes, depends on compiler
  // Useful or you want to keep format
  const char* raw_string{R"(Line1
Line2
Line3
Line4)"};
  // c++14 Standards
  using namespace std::string_literals;
  std::string name5{"Cherno"s + " hello"};
  std::wstring name6{L"Cherno"s + L" hello"};
  std::u32string name7{U"Cherno"s + U" hello"};
}
```

## Constants in C++

The mutability of const depends on how it stores:
- String Literal stored in the read-only section of the memory. So modifying will cause segmentation fault.
- Constant variables may convert to a literal and place the primitive value in assemble in compile stage, while if the code attempt to take the address of constant variable the compiler will let it place in memory.

### Constant Pointer

```c++
#include <iostream>
int main() {
  const int MAX_AGE{90};
  // 1. Pointer to const value
  const int* a{new int};
  std::cout << a << '\n';
  // *a = 2; // I can't change the contents of the pointer
  a = (int*) &MAX_AGE; // But I can change the pointer itself
  std::cout << a << '\n';
  // 2. Const pointers
  int* const b{new int};
  *b = 2; // I can change the contents of the pointer
  // b = (int*) &MAX_AGE; // But I can't change the pointer's value
  // 3. Const pointer to a const value
  const int* const c{&MAX_AGE};
}
```

> `const int*` equals `int const*`. So `const int*& ptr{&a}` is illegal since `a`'s address is a rvalue, `const int* const& ptr{&a}` or `const int*&& ptr{&a}` should work.

### Const Method

Const methods cannot change member variables in the class, except for `mutable` and `static` variables.

```c++
#include <iostream>
class Entity {
private:
  int m_x, * m_y;
  mutable int m_z;
public:
  // When a const method return pointer types, it should has 'const' on both side
  const int* const get_y() const { return m_y; }
  int get_z() const {
    // m_x = 2; // I can't change member variable
    m_z = 2; // But I can change mutable variable
    return m_z;
  }
};
int main() {
  const Entity& e{Entity{}};
  // e.set_x(); // A const object can only call its const methods.
  std::cout << e.get_z() << '\n';
}
```

## Member Initializer Lists in C++ (Constructor Initializer List)

Use member initializer lists can prevent the use of `=` which may initialize object twice.

```c++
#include <iostream>
class Example {
public:
  Example() { std::cout << "Created Entity!" << '\n'; }
  Example(int x) { std::cout << "Created Entity with " << x << "!" << '\n'; }
};
class Entity {
private:
  Example m_example;
  int x, y, z;
public:
  Entity() : m_example{8}, x{}, y{}, z{} { // m_example{8} equals m_example = Example{8}
  // m_example = Example{8}; // This will initialize twice
  }
  Entity(int x, int y) { this->x = x, this->y = y; } // `this` is a pointer point to current object,
  // to avoid ambiguity in member variable and the method's args
};
int main() { Entity e; }
```

## Ternary Operators in C++ (Conditional Assignment)

Ternaay can simplify `if else`
```c++
#include <iostream>
static int s_level{1};
static int s_speed{2};
int main() {
  s_speed = s_level > 5 && s_level < 100 ? s_level > 10 ? 15 : 10 : 5;
  // Above sentence equals to:
  // if (s_level > 5 && s_level < 100)
  //   if(s_level > 10) s_speed = 15;
  //   else s_speed = 10;
  // else s_speed = 5;
  std::cout << s_speed;
}
```

## Create/Instantiate Objects

There are two main section of memory: stack and heap.
- Stack objects have an automatic lifespan, their lifetime is actually controlled by the their scope.
  the stack size is small (usually 1~10M), if you have a big object, you have to store it in the heap.
- Heap: once you allocated an object in the heap, it's gonna sit there unill you explicit delete it.

```c++
#include <iostream>
#include <string_view>
class Entity {
public:
  Entity(const std::string_view str) { std::cout<< str << '\n'; }
};
int main() {
  // Without `new`, object created on the stack
  Entity entity{"Cherno"}; // equals Entity entity = Entity{"Cherno"};
  // With `new`, object created on the heap, new returns the memory address
  Entity* entity2{new Entity{"Cherno"}};
  // We need manually release the object on the heap
  delete entity2;
}
```

> Manual resource release is memory leak prone, use smart pointer is a better idea.

### The New/Delete Keyword

How the `new` keyword find free space on memory? There is something called *free list* which maintain the addresses that have bytes free. It's obvously written in intelligent but it's stll quite slow.
- `new` is just an operator, it uses the underlying C function `malloc()`, means that you can overload `new` and change its bahavious. `new` also calls the constructor.
- `delete` also calls the destructor.

Three uses of `new` (normal new, array new, placement new)
```c++
#include <cstdlib>
#include <new>
class Entity {};
int main() {
  Entity* entity{new Entity}; // Normal `new`
  delete entity; // Remember delete object

  // If we want an array of entries (An array which is stored 50 Objects of Entity)
  Entity* entity2{new Entity[50]}; // Array `new`
  delete[]entity2; // Also we need calling delete with square bracket

  // Placement `new` is where you actually get to decide where the memory comes from.
  // You don't really allocating memory with `new`,
  // but just calling the constructor and initializing object in a specific memory address
  int* buffer{new int[50]};
  Entity* entity3{new(buffer) Entity};
  delete entity3;
  // delete[] buffer; // This will cause double free
  // In C there are some kinds of equivalent:
  Entity* entity4{(Entity*) malloc(sizeof(Entity))};
  new(entity4) Entity; // malloc() will not call the constructor so I need to call it in manual
  entity4->~Entity(); // free() also not call the destructor automatically
  free(entity);
}
```

## Implicit Conversion and the Explicit Keyword in C++

```c++
#include <iostream>
#include <string_view>
class Entity {
private:
  std::string_view m_name;
  int m_age;
public:
  Entity(const std::string_view name) : m_name{name}, m_age{-1} {}
  explicit Entity(int age) : m_name("Unknown"), m_age(age) {} // Use explicit keyword to disable implicit conversion
  std::string_view get_name() const { return m_name; }
};
void print_entity(const Entity& entity) { std::cout << entity.get_name() << '\n'; }
int main() {
  // This is a implicit conversion
  Entity a = std::string_view{"Cherno"};
  // It implicit converting std::string_view{"Cherno"} into
  // Entity's constructor method Entity(const std::string_view& name)
  // It's weird , you can't do this in other languages (such as C# or Java)

  // Entity b = 22; // I can't do implicit conversion with explicit method anymore
  Entity b{22}; // equals Entity b = Entity{22};
  // {} is uniform initialization, it is narrowing conversion (high to low precision) prevention
  
  // C++ allows only one implicit conversion at same time
  // "Cherno" is a const char array, C++ needs to do two conversions,
  // one from const char* to std::string_view, and then call into Entity(const std::string_view& name)
  // Entity c = "Cherno"; // Fail
  // print_entity("Cherno"); // Fail
  print_entity(std::string_view{"Cherno"});
  print_entity(Entity{"Cherno"});
  print_entity({"Cherno"}); // Same as above
}
```

## Operators and Operator Overloading in C++

In the case of operator overloading you're allowed to define or change the behavior of operator

- **Operators are just functions**

Here goes some examples:
```c++
#include <iostream>
#include <ostream>
struct Vector2 {
  float x, y;
  Vector2(float const x, float const y) : x{x}, y{y} {}
  // overload the function "operator+()" equals redefine the behavior of '+' in this Object
  Vector2 operator+(Vector2 const& other) const { return Vector2(x + other.x, y + other.y); }
  Vector2 operator*(Vector2 const& other) const { return Vector2(x * other.x, y * other.y); }
  bool operator==(Vector2 const& other) const { return x == other.x && y == other.y; }
  bool operator!=(Vector2 const& other) const { return !operator==(other); /*Or return !(*this == other);*/ }
};
std::ostream& operator<<(std::ostream& stream, Vector2 const& other) {
  return stream << other.x << ", " << other.y;
}
int main() {
  Vector2 pos{4.0f, 4.0f};
  Vector2 spd{0.5f, 1.5f};
  Vector2 time{1.1f, 1.1f};
  Vector2 res{pos + spd * time};
  // We cannot output the variables in vector directly,
  // We need overload the function `operator<<`
  std::cout << res << '\n';
  // In programs such as Java we have to use equals() to compare objects,
  // but in C++ we can simply overload the "operator=="
  if (res == pos) std::cout << "foo" << '\n';
  else std::cout << "bar" << '\n';
}
```

## Object Lifetime (Stack/Scope Lifetimes)

- Don't return object stored in stack
- Use `{}` to create a local scope so that stacked things will be released earlier.
- Underlying of Unique Pointer
  ```c++
  #include <iostream>
  int* CreateArray() {
    int array[50]; // Don't write code like this
    return array; // The array gets cleared as soon as we go out of scope
  }
  class Entity {
  public:
    void print() { std::cout<< "Print from Entity!" << '\n'; }
    ~Entity() { std::cout << "Entity released~" << '\n'; }
  };
  // We can use `std::unique_pointer` which is a scoped pointer,
  // but here we write our own to explain how it works
  template<class T>
  class ScopedPtr {
  private:
    T* m_ptr;
  public:
    ScopedPtr(T* ptr) : m_ptr{ptr} {}
    ~ScopedPtr() { delete m_ptr; }
    // `->` is special, it's invoked in a loop to call another `->` if
    // the return value is another object (not a pointer), and will finally
    // dereference the founded pointer.
    T* operator->() { return m_ptr; }
    T operator*() { return *m_ptr; }
    ScopedPtr(ScopedPtr<T>&) = delete; // Inhibit copy constructor
    ScopedPtr<T> operator=(ScopedPtr<T>) = delete; // Inhibit copy assignment
  };
  int main() {
    { // scope with brace
      Entity e; // the object created on stack will gets free when out of the scope
  
      ScopedPtr<Entity> ptr = {new Entity};
      // ScopedPtr<Entity> ptr2{ptr};
      ptr->print();
      (*ptr).print();
    }
    // since the scoped pointer object gets allocated on the stack which means it
    // will gets deleted when out of the scope and call ~ScopedPtr()
  }
  ```

## Smart Pointers (std::unique_ptr, std::shared_ptr, std::weak_ptr)

Smart pointers is that when you call `new` , you don't have to call `delete`. Actually in many cases with smart pointers we don't even have to call `new`.

- **Unique pointer**
- **Shared pointer** & **Weak pointer**
  Shared pointer use something called reference counting. If create one shread pointer and define another shared pointer and copy the previous one, the reference count is now 2; when the first one dies (out of scope), the reference count goes down 1; when the last one dies. the reference count goes back to zero and free the memory.
  Weak pointer will not increase the reference count.

```c++
#include <iostream>
#include <memory>
class Entity {
public:
  Entity() { std::cout << "Created Entity!" << std::endl; }
  ~Entity() { std::cout << "Destoryed Entity~" << std::endl; }
  void print() {}
};
int main() {
  // All smart pointer are marked as explicit due to exception safety
  std::unique_ptr<Entity> unique_entity{new Entity};
  // The preferred way to construct this would be use std::make_unique<T>()
  auto unique_entity2{std::make_unique<Entity>()};

  // I can access it like I would normally
  unique_entity2->print();

  // In the definition of unique pointer, the copy constructor
  // and copy assignment operator are deleted
  // std::unique_ptr<Entity> e0{unique_entity2}; // I cannot copy unique pointer
         
  // Weak pointer won't increase the reference count
  std::weak_ptr<Entity> weak_entity;
  { // For shared pointer use make_shared is very recommand because its more efficient
    auto shared_entity{std::make_shared<Entity>()};
    std::cout << "Current count: " << shared_entity.use_count() << '\n';
    auto shared_entity2{shared_entity};
    std::cout << "Current count: " << shared_entity.use_count() << '\n';
    weak_entity = shared_entity;
    std::cout << "Current count: " << shared_entity.use_count() << '\n';
  } // The shared_entity will be free immediately here.
}
```

## Copying and Copy Constructors

Ues `=` (shallow copy) to copy. An object created on heap without a copy constuctors or an object created on stack but with pointer variables that point objects on heap will lead to unexpected results since shallow copy don't copy them fully but essentially just copy the address in pointer variable. So a copy constructor is required to delimit the behavior of the copy operation.

```c++
#include <algorithm>
#include <cstring>
#include <iostream>
class String {
private:
  char* m_buffer;
  size_t m_size;
public:
  String(const char* str) : m_size{strlen(str) + 1} {
    m_buffer = new char[m_size + 1]; // +1 for last null termination char
    std::copy_n(str, m_size, m_buffer);
    // memcpy(m_buffer, str, m_size + 1); // C-style way, you can also use strcpy()
  }
  String(const String& other) : m_size(other.m_size) { // Copy Consturcor
    m_buffer = new char[m_size + 1];
    std::copy_n(other.m_buffer, m_size, m_buffer);
    // The shallow copy is like 'memcpy(this, &other, sizeof(String));'
  }
  // String(const String& other) = delete; // Or you can just prevent this object to do copy operation
  ~String() { delete[] m_buffer; }
  char& operator[](unsigned int const idx) const { return m_buffer[idx]; }
  // make <<() to be a fried so it can access private variables in this object
  friend std::ostream& operator<<(std::ostream& stream, const String& str);
};
std::ostream& operator<<(std::ostream& stream, const String& str) { return stream << str.m_buffer; }
int main() {
  String first_str{"Cherno"};
  String second_str{first_str};
  second_str[2] = 'a';
  std::cout << first_str << '\n';
  std::cout << second_str << '\n';
}
```

## The Arrow Operator

- It's possible to overload the Arror Operator and use it in specific class such as ScopedPtr:
- It can also be used to get the variable's memory offset in an object (in some memory hack):
```c++
#include <iostream>
struct Vector3 {
  float z, y, x; // I deliberately desrupt the naming order to make it in a different memory layout.
};
int main() {
  // float has 4 bytes, 32 bits, long in c++20 has 8 bytes, 64 bits
  long offset_x{(long) &((Vector3*) 0)->x}; // Or &((Vector3*) nullptr)->x;
  std::cout << offset_x << std::endl;
  long offset_y{(long) &((Vector3*) 0)->y}; // Or &((Vector3*) nullptr)->x;
  std::cout << offset_y << std::endl;
  long offset_z{(long) &((Vector3*) 0)->z}; // Or &((Vector3*) nullptr)->x;
  std::cout << offset_z << std::endl;
}
```

## Dynamic Arrays (std::vector)

Vector in C++ is not mathematical vector, it's kind of dynamic arrays like.

```c++
#include <iostream>
#include <ostream>
#include <vector>
struct Vertex {
  float x, y, z;
};
std::ostream& operator<<(std::ostream& stream, Vertex const& vertex) {
  stream << vertex.x << ", " << vertex.y << ", " << vertex.z;
  return stream;
}
int main() {
  std::vector<Vertex> vertices;
  vertices.push_back({1, 2, 3});
  vertices.push_back({4, 5, 6});
  // Using range based 'for loop' to iterate the object in dynamic array
  for (const Vertex& v : vertices) std::cout << v << '\n';
  vertices.erase(vertices.begin() + 1); // Remove the second object by using an iterator
  vertices.clear();                     // Or we can clean the whole dynamic array
}
```

### Optimizing the usage of std::vector

Two ways to reduce memory copy

```c++
#include <iostream>
#include <vector>
struct Vertex {
  float x, y, z;
  static int copy_count;
  Vertex(float x, float y, float z) : x(x), y(y), z(z) {}
  // Copy Constructor, used to capture copied times
  Vertex(Vertex const& v) : x(v.x), y(v.y), z(v.z) { std::cout << "Copied " << ++copy_count << " times" << '\n'; }
};
int Vertex::copy_count{};
int main() {
  std::vector<Vertex> vertices_bad;
  vertices_bad.push_back({1, 2, 3}); // 1 copy to store
  // Trigger rearrange, new size is current_elements x 2, which is 2 x Vertex
  vertices_bad.push_back({4, 5, 6}); // 1 copies to rearrange + 1 copy to store
  // Trigger rearrange, now reserved 4 x Vertex
  vertices_bad.push_back({7, 8, 9});    // 2 copies to rearrange + 1 copy to store
  vertices_bad.push_back({10, 11, 12}); // 1 copy to store
  // Trigger rearrange, new reserved 8 x Vertex
  vertices_bad.push_back({13, 14, 15}); // 4 copies to rearrange,  1 copy to store
  vertices_bad.push_back({16, 17, 18}); // 1 copy to store
  vertices_bad.push_back({19, 20, 21}); // 1 copy to store
  vertices_bad.push_back({22, 23, 24}); // 1 copy to store
  // Trigger rearrange, new reserved 16 x Vertex
  vertices_bad.push_back({25, 26, 27}); // 8 copies to store, 1 copies to store, total 24
  // Each time push_back() will do 1 copy operation to store the Vertex to vector,
  // And each push back may let vector do memory rearrange if reserved memory is full, which copies
  // previous objects in dynamic array into new memory area.

  std::cout << "vertices good" << '\n';
  std::vector<Vertex> vertices_good;
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

Using GLFW as example.
- Visual Studio
  1. Create a folder called "Dependencies" under your project directory and then put the library into it.
     ```plaintext
     C:\Users\USERNAME\source\repos\Your_Project_Directory\Dependencies\
     -> GLFW\
       -> include\GLFW\
         glfw3.h
         ...
       -> lib-vc2015\
         glfw3.dll
         glfw3.lib
         glfw3.dll.lib
      ```
   2. Open project settings:
      -> Configuration: All Configuration
      -> C/C++ -> Additional Include Directories: `$(SolutionDir)\Dependencies\GLFW\include`
      -> Linker -> General -> Additional Library Directories: `$(SolutionDir)\Dependencies\GLFW\lib-vc2015`

### Static linking

Static linking happens at compile time, the lib intergrate into executable or a dynamic library

Visual Studio
1. Open project setting
   -> Linker -> Input -> Additional Dependencies: `glfw3.lib;xxxxxx;balabala;...`
2. Static Link
   ```c++ Main.cpp
   // quote for header in this project, regular bracket for external library
   #include <GLFW/glfw3.h>
   // Or `extern "C" int glfwInit();`
   // Since GLFW is actually a C library so we need `extern "C"`
   int main() {
     int result = glfwInit();
     std::cout << result << '\n';
   }
   ```

### Dynamic linking

Dynamic linking happens at runtime

- Some librarys like GLFW supports both static and dynamic linking in a single header file.
- `glfw3.dll.lib` is basically a series of pointers into `glwfw3.dll`
- Code is basically as same as static linking.

Visual Studio
1. Open project settings:
   -> Linker -> Input -> Additional Dependencies: `glfw3.dll.lib;xxxxxx;balabala;...`
2. Put `glfw3.dll` to the same folder as your executable file (i.e: `$(SolutionDir)\Debug`)
3. In fact, to call a function in dynamic library, it needs a prefix called `__declspec(dllimport)`
   If you explore `glfw3.h` you will see there is a prefix `GLFWAPI` in every function's definition:
   ```c++
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
   1. Create one solution with 2 projects: "Game" and "Engine",
   2. Project "Game":
      Ceneral->Project Defaults->Configuration Type: Application (.exe)
      -> C/C++ -> General -> Additional include Directories: `$(SolutionDir)\Engine\src;`
   3. Project "Engine":
      Ceneral->Project Defaults->Configuration Type: Static library (.lib)
   4. Right click on projects "Game" -> Add -> Reference -> Select project "Engine"
2. Code for project "Engine":
   ```c++ Your_Project_Directory\src\Engine.h
   #pragma once
   namespace engine {
     void print_message();
   }
   ```
   ```c++ Your_Project_Directory\src\Engine.cpp
   #include "Engine.h"
   #include <iostream>
   namespace engine {
     void print_message() { std::cout << "Hello Game!" << '\n'; }
   }
   ```
3. Code for project "Game":
   ```c++ Your_Project_Directory\src\Application.cpp
   #include "Engine.h"
   int main() { engine::print_message(); }
   ```

## How to Deal with Multiple Return Values in C++

Example scenario: We have a function called `parse_shader()`, it needs to return two strings.

- Return a struct cotains two strings (Cherno's choose):
  ```c++
  #include <string>
  struct ShaderProgramSource {
    std::string vertex_source;
    std::string fragment_source;
  };
  ShaderProgramSource parse_shader() {
    // ... (Some statements that process result 'vs' and 'fs')
    std::string vs, fs;
    return {vs, fs};
  }
  ```
- Use reference paremeter (Probably one of the most optimal way)
   ```c++
   #include <string>
   void parse_shader(std::string& out_vertex_source, std::string& out_fragment_source) {
     // ... (Some statements that process result 'vs' and 'fs')
     std::string vs, fs;
     std::tie(out_vertex_source, out_fragment_source) = std::tuple{vs, fs};
   }
   // Or use pointer parameter if you want to pass nullptr (ignore the output):
   void parse_shader2(std::string* outVertexSource, std::string* outFragmentSource) {
     std::string vs, fs;
     if (outVertexSource) *outVertexSource = vs;
     if (outFragmentSource) *outFragmentSource = fs;
   }
   int main() {
     std::string vertex_source, fragment_source;
     parse_shader(vertex_source, fragment_source);
     parse_shader2(nullptr, &fragment_source);
   }
   ```
- Return a `std::array` or `std::vector`
  The different is primarly the arrays can be create on the stack whereas vectors gonna store its underlying storage on the heap.
  So technically returning a standard array would be faster.
  ```c++
  #include <array>
  #include <string>
  std::array<std::string, 2> parse_shader() {
    std::string vs, fs;
    // ... (Some statements that process result 'vs' and 'fs')
    return {vs, fs};
  }
  ```
- Using `std::tuple` and `std::pair`
  ```c++
  #include <string>
  #include <tuple>
  std::tuple<std::string, int> create_person() { return {"Cherno", 24}; }
  std::pair<std::string, int> create_person2() { return {"Cherno", 24}; }
  int main() {
    // std::tuple can return more than two elements
    auto person{create_person()};    // Automatically deduce the return type
    auto& name{std::get<0>(person)}; // Get values in std::tuple
    int age{std::get<1>(person)};
    // std::pair is a little bit faster than tuple
    auto [name2, age2]{create_person2()}; // c++17 structure binding
  }
  ```

## Templates

Template can improve code reuse rate and reduce duplicate code (e.g. function overload), the essence of template is similar to macros

1. template type
  ```c++
  // In this case, template specifying how to create methods based on usage of them.
  // So if nobody call this function, it's code will not exist in compiled file,  and
  // even if there is a grammatical error in templated function code, it will still
  // compile successfully.
  #include <iostream>
  template <class T> // exactly same with 'template<typename T>'
  void print(T value) { std::cout << value << '\n'; }
  int main() {
    print(5);      // Auto deducing
    print<int>(5); // It's a good manner specifying the type explicitly
    print<char const*>("Hello");
    print<float>(5.5f);
  }
  ```
2. template argument
   ```c++
   template<class T, int N> // Multiple template targets can be in one template definition
   class Array {
   private:
     T m_arr[N];
   public:
     int get_size() const { return N; }
   };
   int main() {
     Array<int, 5> arr; // It will generate the following code:
     // class Array {
     // private:
     //   int m_arry[5];
     // public:
     //   int get_size() const { return 5; }
     // };
     std::cout << arr.get_size() << '\n';
   }
   ```
3. The keyword `typename`
   Clearify something is a type.
   ```c++
   // C++20 makes `typename` optional on where nothing but a type name can appear
   template <class T>
   T::U f(); // Return type
   template <class T>
   void f(typename T::U); // Ill-formed in global scope, without `typename`, `T::U` would be
   // considered a static member
   template <class T>
   struct S {
     T::U r; // member type
     T::P f(T::P p) { // But Ok in class scope, argument type of a methods
       return static_cast<T::R>(p); // Type in casts
     }
   };
   ```
4. Non-type template parameter (C++20)
   ```c++
   template<class T> struct X { constexpr X(T) {} };
   template <X x> struct Y {}; // Non-type template parameter
   Y<0> y; // OK, Y<X<int>(0)>
  
   template <typename T, std::size_t N>
   struct S2 { T data[N]; /* Array of dependent bound */ };
   S2 s{{1, 2, 3, 4, 5}};
   ```
5. User-defined deduction guides
   Template parameters of a class template could not be deduced from a constructor call until the introduction of deduction guides in C++17
   ```c++
   template <class T>
   struct Container {
     Container(T t) {}
     template <class Iter> Container(Iter begin, Iter end) {}
   };
   template <class Iter> // Guides compiler how to deducing elements type
   Container(Iter b, Iter e) -> Container<typename std::iterator_traits<Iter>::value_type>;
   
   std::vector<double> v{1, 2};
   Container d{v.begin(), v.end()}; // Deduces Container<double>
   ```
6. Abbreviated function template (C++20)
   ```c++
   void f1(auto); // same as template<class T> void f1(T)
   void f2(Concept1 auto);
   template<>
   f1(int); // Can be specialized like normal
   ```

### How to Make Base Class Access Derived Class's Methods (CRTP)

Curiously Recurring Template Pattern (CRTP): A derived class derives the base class with itself as a template argument.

```c++
#include <iostream>
// Template template parameter can be constrained (Though not necessary)
template <template <Addable> typename Templ_Derived, typename T>
// Requires clause also works
// template <template <typename T> requires Addable<T> typename Templ_Derived, typename T>
class Base {
private:
  Base() {}
  friend Templ_Derived<T>;
public:
  void print_derived_add(T a, T b) {
    auto& d{static_cast<Templ_Derived<T>&>(*this)};
    std::cout << d.add(a, b) << '\n';
  }
};
template <Addable T>
class Derived : public Base<Derived, T> {
public:
  T add(T a, T b) { return a + b; }
};
int main() {
  Derived<int>().print_derived_add(1, 2);
}
```

### SFINAE

*"Substitution Failure Is Not An Error"*, compiler will continue to find suitable template.

If statement in template using SFINAE
```c++
template <bool Cond, typename IfTrue, typename IfFalse>
struct conditional {
  using type = IfTrue;
};
template <typename IfTrue, typename IfFalse>
struct conditional<false, IfTrue, IfFalse> { // This will has higher priority
  using type = IfFalse;
};
```

Narrowing conversion check using SFINAE
```c++
template <typename From, typename To, typename = void>
struct is_narrowing : std::true_type {};
template <typename From, typename To>
// To{std::declval<From>()} is ill-formed in case of narrowing cast, so will prevents match this template
struct is_narrowing<From, To, std::void_t<decltype(To{std::declval<From>()})>> : std::false_type {};

static_assert(!is_narrowing<std::int8_t, std::int16_t>::value);
static_assert(!is_narrowing<std::uint8_t, std::int16_t>::value);
static_assert(!is_narrowing<float, double>::value);
static_assert( is_narrowing<double, float>::value);
static_assert( is_narrowing<int, uint32_t>::value);
```

A skillfully implementation of custom `std::formatter`. The `parse()` method is a template, since it's `constexpr` specified, any possible cases that result a `throw` will be filtered in a SFINAE way.
```c++
struct QuotableString : std::string_view {};
template <typename CharT>
struct std::formatter<QuotableString, CharT> {
  bool quoted{false};
  template <class ParseContext>
  constexpr ParseContext::iterator parse(ParseContext& ctx) {
    auto it{ctx.begin()};
    if (it == ctx.end()) return it;
    if (*it == '#') (quoted = true), ++it;
    if (it != ctx.end() && *it != '}') // How could this pass the compile?
      throw std::format_error("Invalid format args for QuotableString.");
    return it;
  }
  // template <class FmtContext>
  // FmtContext::iterator format(QuotableString s, FmtContext& ctx) const {
  //   std::ostringstream out;
  //   if (quoted) out << std::quoted(s);
  //   else out << s;
  //   return std::ranges::copy(std::move(out).str(), ctx.out()).out; // std::move is not necessary in C++20 since it do type deducing
  // }
  template <class FmtContext>
  constexpr FmtContext::iterator format(QuotableString s, FmtContext& ctx) const noexcept { // constexpr version
    return quoted ? std::format_to(ctx.out(), "{:?}", static_cast<std::string_view>(s)) : std::ranges::copy(s, ctx.out()).out;
  }
};
int main() {
  QuotableString a("be"), a2(R"( " be " )");
  QuotableString b("a question");
  std::cout << std::format("To {0} or not to {0}, that is {1}.\n", a, b);
  std::cout << std::format("To {0:} or not to {0:}, that is {1:}.\n", a, b);
  std::cout << std::format("To {0:#} or not to {0:#}, that is {1:#}.\n", a2, b);
}
```

### Ways to Print Type Name

```c++
#include <cxxabi.h>
#include <iostream>
#include <memory>
#include <source_location>
#include <string_view>
template <typename T> // One way
std::string type_name() noexcept {
  using TRR = std::remove_reference<T>::type;
  std::unique_ptr<char, std::function<void(void*)>> p_demangled_name{abi::__cxa_demangle(typeid(TRR).name(), nullptr, nullptr, nullptr),
                                                                     std::free};
  std::string ret{p_demangled_name != nullptr ? p_demangled_name.get() : typeid(TRR).name()};
  if (std::is_const<TRR>::value) ret += " const";
  if (std::is_volatile<TRR>::value) ret += " volatile";
  if (std::is_lvalue_reference<T>::value) ret += "&";
  else if (std::is_rvalue_reference<T>::value) ret += "&&";
  return ret;
}
template <typename T> // Another way using function name with signature
constexpr std::string_view type_name2() noexcept {
  std::string_view capt{std::source_location::current().function_name()}; // Or "__PRETTY_FUNCTION__" for < c++20
  // e.g. "static_string type_name2() [T = const int &]"
  return {capt.cbegin() + capt.find('=') + 2, capt.cend() - 1};
}
int& foo_lref();
int&& foo_rref();
int foo_value();
int main() {
  int i = 0;
  int const ci = 0;
  std::cout << "decltype(i) is " << type_name<decltype(i)>() << '\n';
  std::cout << "decltype((i)) is " << type_name<decltype((i))>() << '\n';
  std::cout << "decltype(ci) is " << type_name<decltype(ci)>() << '\n';
  std::cout << "decltype((ci)) is " << type_name<decltype((ci))>() << '\n';
  std::cout << "decltype(static_cast<int&>(i)) is " << type_name<decltype(static_cast<int&>(i))>() << '\n';
  std::cout << "decltype(static_cast<int&&>(i)) is " << type_name<decltype(static_cast<int&&>(i))>() << '\n';
  std::cout << "decltype(static_cast<int>(i)) is " << type_name<decltype(static_cast<int>(i))>() << '\n';
  std::cout << "decltype(foo_lref()) is " << type_name<decltype(foo_lref())>() << '\n';
  std::cout << "decltype(foo_rref()) is " << type_name<decltype(foo_rref())>() << '\n';
  std::cout << "decltype(foo_value()) is " << type_name<decltype(foo_value())>() << '\n';
}
```

### Concepts

```c++
template <typename T> concept Addable = requires(T a, T b) { a + b; };
template <typename T> concept Dividable = requires(T a, T b) { a / b; };
template <typename T> concept DivAddable = Addable<T> && Dividable<T>;

template <typename T, typename U> concept ExampleReq = requires(T x, U) {
  // simple requirement: expression must be valid
  x++;
  *x;
  // type requirement: T::value_type type must be a valid type
  typename T::value_type;
  typename std::vector<T>;
  // compound requirement: {expression}[noexcept][-> Concept];
  // {expression} -> Concept<A1, A2, ...> is equivalent to requires Concept<decltype((expression)), A1, A2, ...>
  { *x } noexcept;                                         // dereference must be noexcept
  { *x } noexcept -> std::same_as<typename T::value_type>; // dereference must return T::value_type
  // nested requirement: requires ConceptName<...>;
  requires Addable<T>;
};

template <typename T> requires Addable<T> // Use requires clause to constrain
T type_add(T a, T b) { return a + b; }
template <Addable T> // Directly use concept as template parameter (cleaner way)
T type_add2(T a, T b) { return a + b; }

export template <typename T>
consteval bool type_addable(T x) { // requires-expression render to a bool at compile-time
  if (Addable<T>) return true;
  // if (requires(T a, T b) { a + b; }) return true;
  else return false;
}

// Some STD concepts
export template <std::integral T>
void print_concept() { std::println("Integral matched"); }
export template <std::integral T> requires std::is_unsigned_v<T> void print_concept() { std::println("Unsigned integral matched"); }
export template <std::floating_point T>
void print_concept() { std::println("Floating point matched"); }
export template <std::floating_point T> requires(sizeof(T) == 8) void print_concept() { std::println("Doubled Floating point matched"); }
````

Narrowing/Non-narrowing conversion check:
```c++
#include <cstdint>
template <typename From, typename To> concept narrowing = !requires(From f) { To{f}; };
template <typename From, typename To> concept non_narrowing = requires(From f) { To{f}; }; // !narrowing_conversion<From, To>; // Or simply inverse the narrowing conversion check
static_assert(!narrowing<std::int8_t, std::int16_t>);
static_assert(!narrowing<std::uint8_t, std::int16_t>);
static_assert(!narrowing<float, double>);
static_assert(narrowing<double, float>);
static_assert(narrowing<int, uint32_t>);
int main() {}
```

## Stack vs Heap Memory in C++

Ignore...

## Macros in C++

Macros do text replace at preprocessor stage
 ```c++
 #include <iostream>
 #define WAIT std::cin.get()
 int main() {
   WAIT;
 }
 ```

Macros function and combine with the environment, environment variables can be defined at: Project settings -> C/C++ -> Preprocessor -> Preprocessor Definitions
 ```c++
 #ifdef PR_DEBUG
 #define LOG(x) std::cout << x << '\n'
 #elif defined(PR_RELEASE)
 #define LOG(x) // Do nothing
 #endif
   int main() {
   LOG("Hello");
 }
 ```

Variadic macros
```c++
main() {
#define LOG1(...) \
  __VA_OPT__(std::printf(__VA_ARGS__);)
  LOG1();                 // `__VA_OPT__` allows optional `__VA_ARGS__`
  LOG1("number is 12\n"); // `__VA_OPT__` allows to omit the trailling comma when `__VA_ARGS__` are empty
  LOG1("number is %d\n", 12);
}
```

## The "auto" Keyword

Be careful with `auto`:
```c++
#include <iostream>
#include <string>
std::string get_name() { return "Cherno"; }
// char* get_name() { return "Cherno"; }
int main() {
  auto name{get_name()};
  // Call a type specific method size(), if the return type of GetName() changed to "char*" , this will be broken
  int a = name.size();
  std::cout << a << '\n';
}
```

`auto`' can reduce type length
```c++
#include <string>
#include <unordered_map>
#include <vector>
class Device {};
class DeviceManager {
  using Device_t = std::unordered_map<std::string, std::vector<Device*>>;
private:
  Device_t m_devices;
public:
  const Device_t& GetDevices() const { return m_devices; }
};
int main() {
  DeviceManager dm;
  // The type is too messy
  const std::unordered_map<std::string, std::vector<Device*>>& devices{dm.GetDevices()};
  // You can use keyword "using" or "typedef" to make alias
  using DeviceMap = std::unordered_map<std::string, std::vector<Device*>>;
  const DeviceMap& devices2{dm.GetDevices()};
  const auto& devices3{dm.GetDevices()}; // Or auto
}
```

## Static Arrays in C++ (std::array)

TODO

## Function Pointers in C++

1. 3 ways to definite a Function Pointers
   ```c++
   void hello_world(int a) noexcept { std::println("Hello World! Value: {}", a); }
   int main() {
     auto const f1{hello_world};                // auto deducing
     void (*const f2)(int){hello_world};        // C-style function pointer
     using Balabala = std::function<void(int)>; // Using alias
     // typedef void (*Balabala)(int);             // Or C-style typedef
     Balabala const& f3{hello_world};
     f1(1), f2(2), f3(3);
   }
   ```
2. A simple usage - the ForEach function:
   ```c++
   template <typename E> void print_value(E value) { std::println("Value: {}", value); }
   template <typename Cont, typename Func> requires requires(Func func) { static_cast<std::function<void(typename Cont::value_type)>>(func); }
   void for_each(Cont const& cont, Func func) { for (typename Cont::value_type v : cont) func(v); }
   int main() {
     std::vector vec{1, 5, 4, 2, 3};
     for_each(vec, print_value<int>);
     for_each(vec, []<typename E>(E value) { std::println("Value: {}", value); }); // Or use a lambda (with template parameter)
   }
   ```

## Lambdas in C++

A lambda is basically a little throwaway function that you can write and assign to a variable quickly.

1. How to put outside variables into lambda function
   `[=]`: Pass everything in by value, the pass in variables is independent of the outside.
   `[&]`: Pass everything in by reference.
   `[a]`: Pass `a` by value
   `[&a]`: Pass `a` by reference.
2. using `mutable` keyword to allow modify pass in variables
   ```c++
   int main() {
     int a{8};
     auto f{[=]() mutable { a = 5; std::println("Value: {}",a); }};
     f();
     std::println("Value: {}", a); // x is still 8, because [=] just copy value into this lambda.
   }
   ```
3. We need to use `std::function` instand of C-style raw function pointer if lambda has pass in variables (stateful).
   ```c++
   void for_each(std::vector<int> const& values, std::function<void(int)> const& func) noexcept {
     for (int const i : values) func(i);
   }
   int main() {
     std::vector values{1, 2, 3, 4, 5};
     int state{};
     // Cannot cast to C-style void(*callback)(int)
     auto callback{[&](int value) { std::println("Current state: {}, Value: {}", state, value); }};
     state = 1;
     for_each(values, callback);
   }
   ```
4. Usage of `std::find_if` (returns an iterator to the first element when callback function returns true)
   ```c++
   int main() {
     std::vector values{1, 5, 4, 2, 3};
     auto iterator{std::find_if(values.begin(), values.end(), [](int value) { return value > 3; })};
     std::println("First element that > 3: {}", *iterator);
   }
   ```
5. Capturing Parameter Packs in Lambda
   ```c++
   constexpr int add(int a, int b) noexcept { return a + b; }
   template <typename F, typename... Args>
   constexpr auto delay_call(F&& f, Args&&... args) noexcept {
     // C++20 improved capturing parameter packs in lambda
     return [f = std::forward<F>(f), ... f_args = std::forward<Args>(args)]() constexpr noexcept { return f(f_args...); };
     // "..." is like says "take whatever on the life and unpack it accordingly"
     // If the parms pack f_args is "<int, int>{1, 2}"
     // Compiler will expand `f(args...)` to `f(1, 2)`, expand `f(args)...` to `f(arg1), f(arg2)`
   }
   int main() {
     std::println("{}", delay_call(add, 1, 2)());
   }
   ```

## Namespaces in C++

Use Namespace to
1. Avoid naming conflict: `apple::print()`, `orange::print()`
2. Avoid C library like naming: `GLFW_initialize` to `GLFW::initialize`

We can set to use only specific symbol in a namespace
```c++
namespace apple {
static char const* s_text;
void print(char const* text) { s_text = text, std::println("{}", text); }
void print_again() { std::println("{}", s_text); }
}; // namespace apple
int main() {
  using apple::print;
  print("Hello");
  apple::print_again(); // We still need 'apple::' to call print_again()
}
```

Nested namespaces can be shorten using alias:
```c++
namespace apple::functions {
void print(const char* text) { std::println("{}", text); }
} // namespace apple::functions
int main() {
  namespace a = apple::functions;
  a::print("Hello");
}
```

### Why don't "using namespace std"

Absolutely don't use `using namespace` in header files.
If you must using `using namespace`, please use it in a small scope as possible.

For example a serious issue of implicit conversion:
```c++
namespace apple {
void print(std::string const& text) { std::println("{}", text); }
} // namespace apple
namespace orange {
void print(char const* text) {
  std::string tmp{text};
  std::reverse(tmp.begin(), tmp.end());
  std::println("{}", tmp);
}
} // namespace orange
using namespace apple;
using namespace orange;
int main() {
  print("Hello"); // Which one will get called?
  // Answer: the orange::print() will be called, because the type of "Hello" is 'char*'
}
```

## Threads

If we want to do something else when we called functions that will block the current thread, we can use threads (or coroutines in C++20).

- A `std::thread` shoud have either `join()` or `detach()` called otherwise it will call `std::terminate()` in its destructor.

Here is an example:
We created a thread that will do loop on outputting "Working..",
and simultaneously the main() function is waiting for user input.
```c++
int main() {
  std::println("[Main] tid={}", std::this_thread::get_id());
  bool is_finished{false};
  // In a async operation, there are ways to store the result of a thread's processing':
  // std::promise' is use by the producer/writer, while 'std::future' is used by the consumer/reader,
  // the latter don't have ability to set/write.
  std::promise<std::string> promise;
  std::future<std::string> future{promise.get_future()};
  std::thread worker([&is_finished, promise = std::move(promise)]() mutable { // As soon as we create thread instance, it's going to immediately kick off that thread
    std::println("[thread] Started, tid={}", std::this_thread::get_id());
    while (!is_finished) {
      std::println("Working...");
      std::this_thread::sleep_for(std::chrono::seconds(1));
      // using namespace std::literals::chrono_literals; // Or use namespace for convenience
      // std::this_thread::sleep_for(1s);
    }
    promise.set_value("From thread: I'm completed!");
  });
  // Since the ownership of the 'promise' has 'std::move()'d to the thread 'worker',
  // we can no longer use it in this function
  std::cin.get();
  is_finished = true;
  worker.join(); // Let main thread to wait for this thread (block main thread)
  std::println("Finished. Get values from thread: {}", future.get());
}
```

## Coroutines

```c++
// Promise
template <typename T>
struct Co {
  struct promise_type;
  std::coroutine_handle<promise_type> handle;
  struct promise_type {
    int m_count{};
    T m_ret;
    Co get_return_object() { return Co{.handle = std::coroutine_handle<promise_type>::from_promise(*this)}; }
    auto initial_suspend() { return std::suspend_never{}; } // Called when coroutine object instantiated, can either return `std::suspend_always`, `std::suspend_never`, or construct a custom awaiter struct/class
    auto final_suspend() const noexcept { return std::suspend_always{}; } // Called when coroutine function completed
    void unhandled_exception() const { std::terminate(); }
    // void return_void() { std::println("[promise_type] return_void"); } // Called by 'co_return'
    void return_value(T value) { m_ret = value; } // Called by 'co_return <value>', can't coexist with 'return_void()'
    auto yield_value(int value) { // Called by `co_yield`, normally you should choose either `co_yield` or `co_await`
      m_count = value;
      return std::suspend_always{};
    }
    auto await_transform(std::function<T()> const& func) const {
      struct Awaiter {
        std::future<T> m_future;
        std::thread m_thr;
        bool await_ready() { return false; } // Called by 'co_await', return false to suspend
        void await_suspend(std::coroutine_handle<>) { std::println("[Awaiter] await_suspend"); } // Called at suspend
        T await_resume() { // Called at resume
          std::println("[Awaiter] Thread block? Main thread tid#{}", std::this_thread::get_id());
          return m_future.get();
        }
      };
      std::promise<T> promise;
      std::future<T> future{promise.get_future()};
      std::thread thr([&func, promise{std::move(promise)}]() mutable {
                      std::println("[co_await] tid#{}", std::this_thread::get_id());
                      T value{func()};
                      promise.set_value(value); });
      thr.detach();
      return Awaiter{std::move(future), std::move(thr)}; // Use 'std::move()' so they won't be cleaned up at method completed
    }
  };
};
int main() {
  auto co{[]() -> Co<int> { // Coroutine
    std::println("[run] before co_await");
    int ret{co_await []() { std::this_thread::sleep_for(std::chrono::seconds(2)); return 114514; }}; // Dummy function for coroutine to run
    std::println("[run] after co_await");
    co_yield 1;
    std::println("[run] after co_yield");
    co_return ret;
    std::println("[run] after co_return");
  }()};

  std::println("[main] tid#{}", std::this_thread::get_id());
  for (int i{1}; !co.handle.done(); i++) {
    std::println("\n[main]resume {} times", i);
    std::println("[main]m_count {}", co.handle.promise().m_count);
    co.handle.resume();
  }
  std::println("[main] get returned value: {}", co.handle.promise().m_ret);
  co.handle.destroy(); // Don't forget to destory, or memory leak could occur.
{}
```

## Timing

Make a Timer for statistical time-consuming
```c++
template <class Res = std::chrono::milliseconds>
class Timer {
  using Clock = std::conditional_t<std::chrono::high_resolution_clock::is_steady,
                                   std::chrono::high_resolution_clock, std::chrono::steady_clock>;
private:
  std::chrono::time_point<Clock> const m_start_time;
  std::chrono::time_point<Clock> m_last_time;
public:
  Timer() noexcept : m_start_time(Clock::now()), m_last_time{Clock::now()} { std::println("Timer start!"); }
  ~Timer() {
    std::println("Time destructored, life time {}", std::chrono::duration_cast<Res>(Clock::now() - m_start_time));
  }
  inline void count() noexcept {
    std::println("Time took: {}", std::chrono::duration_cast<Res>(Clock::now() - m_last_time));
    m_last_time = Clock::now();
  }
  inline void renew() noexcept { m_last_time = Clock::now(); }
};
int main() {
  []() {
    Timer timer; // The timer will be auto delete when run out of the scope
    std::this_thread::sleep_for(std::chrono::milliseconds(1145)); // Sleep for 114514ms
  }();
}
```

## Multidimensional Arrays

```c++
int main() {
  int** a2d{new int*[20]}; // 2D array, stores type 'int*'
  for (int i{}; i < 20; i++) { // Initiate a 20x30 2D array
    a2d[i] = new int[30];
    for (int j{}; j < 30; j++) a2d[i][j] = i * 30 + j; // Given values
  }
  for (int i{}; i < 20; i++) { // Print the 2D array
    std::print("{:3}", a2d[i][0]);
    for (int j{1}; j < 30; j++) std::print(" {:3}", a2d[i][j]);
    std::println("");
  }
  for (int i{}; i < 20; i++) delete[] a2d[i]; // First release the sub array
  delete[] a2d;

  // 3D array, you can imagine a cube with size 2x3x4 (row, column, height)
  int*** a3d{new int**[2]};
  for (int i{}; i < 2; i++) {
    a3d[i] = new int*[3];
    for (int j{}; j < 3; j++) {
      a3d[i][j] = new int[4];
      for (int k{}; k < 4; k++) a3d[i][j][k] = k * 2 * 3 + i * 3 + j;
    }
  }
  // Now print the 3D array by slicing the heights
  std::println("Layer1       Layer2     Layer3      Layer4");
  for (int i{}; i < 2; i++) {
    for (int k{}; k < 4; k++) {
      std::print("{:2}", a3d[i][0][k]);
      for (int j{1}; j < 3; j++) std::print(" {:2}", a3d[i][j][k]);
      std::print("    ");
    }
    std::println("");
  }
  // When you want to delete this array, you have to go through inner array and
  // delete all of those arrays from inside to out
  for (int i{0}; i < 2; i++) {
    for (int j{0}; j < 3; j++) delete[] a3d[i][j];
    delete[] a3d[i];
  }
  delete[] a3d;
}
```

The most issue is that the Multidimensional Arrays will results memory fragmentation. When iterating the array we have to jump to another location to read or write that data, and that results probably a cache miss which means that we're wasting time fetching data from RAM.

One of the most feasible thing you can do is just store them in a single dimensional array:
```c++
int main() {
  int* arr{new int[5 * 5]};
  for (int i{}; i < 5 * 5; i++) arr[i] = i;
  for (int i{}; i < 5 * 5; i++)
    switch (i % 5) {
    case 4: std::println(" {:2}", arr[i]); break;
    default: std::print(" ");
    case 0: std::print("{:2}", arr[i]);
    }
  delete[] arr;
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

Defined member in `union` means they all in same memory location, `union` is a common way for "Type Punning".

```c++
struct Vector2 {
  float x, y;
  constexpr Vector2(float const x, float const y) noexcept : x{x}, y{y} {}
  // overload the function "operator+()" equals redefine the behavior of '+' in this Object
  constexpr Vector2 operator+(Vector2 const& other) const noexcept { return Vector2(x + other.x, y + other.y); }
  constexpr Vector2 operator*(Vector2 const& other) const noexcept { return Vector2(x * other.x, y * other.y); }
  constexpr bool operator==(Vector2 const& other) const noexcept { return x == other.x && y == other.y; }
  constexpr bool operator!=(Vector2 const& other) const noexcept { return !operator==(other); /*Or return !(*this == other);*/ }
};
constexpr std::ostream& operator<<(std::ostream& stream, Vector2 const& other) noexcept { return stream << '[' << other.x << ", " << other.y << ']'; }

template <>
struct std::formatter<Vector2, char> {
  template <class ParseContext> constexpr ParseContext::iterator parse(ParseContext const& ctx) noexcept { return ctx.begin(); }
  template <class FmtContext>
  FmtContext::iterator constexpr format(Vector2 vec2, FmtContext& ctx) const noexcept {
    return std::format_to(ctx.out(), "[{}, {}]", vec2.x, vec2.y);
  }
};
int main() {
  Vector2 pos{4.0f, 4.0f}, spd{0.5f, 1.5f}, time{1.1f, 1.1f}, res{pos + spd * time};
  // We cannot output the variables in vector directly, we need overload the function
  // operator<<()
  std::cout << res << '\n';
  std::println("{}", res); // Or use a custom std::formatter for std::print

  // In programs such as Java we have to use equals() to compare objects,
  // but in C++ we can simply overload the operator==()
  if (res == pos) std::println("foo");
  else std::println("bar");
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

C++'s cast can do anything that C-style casts can do, those casts make you code more solid and looks better.

1. Static cast (compile time checking).
2. Reinterpret cast `reinterpret_cast<>()` (for Type Punning).
3. Dynamic cast (will return `NULL` if casting is failed)
4. Const cast `const_cast<>()` (Remove the const-ness from references or pointers that ultimately refer to something not constant)

```c++
class Base {
public:
    Base() {}
    virtual ~Base() {}
};
class Derived : public Base {
    // ...
};
int main() {
    // Static cast
    double value = 5.5;
    double sc = static_cast<int>(value);

    Base* p_base = new Base();
    Derived* ac = dynamic_cast<Derived*>(p_base); // Main class cannot be casted to sub class
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
```c++
class Entity {};

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

```c++
std::tuple<std::string, int> CreatePerson()
{
    return { "Cherno", 24 };
}

int main()
{
    // Structured binding
    auto[name, age] = CreatePerson();
    std::cout << name;

// Or std::tie for already defined variables
  std::string name2;
  int age2;
  std::tie(name2, age2) = std::tuple("", 14);
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

```c++
template <typename T>
void f(T&&) { std::println("f(T&&) matched"); }
template <typename T>
void f(T const&) { std::println("f(T const&) matched"); }

// Perfect forwarding
template <typename T>
void wrapper(T&& arg) {
  f<T>(arg); // Deducing automatically will cast arg to T&&, so we need explicitly specify f<T>.
  f<T>(std::forward<T>(arg));
}
int main() {
  wrapper(1);
  int a{1};
  wrapper(a); // Compiler will deducing to `int&`
}
```

## std::print

```c++
int main() {
  char a{'a'};
  // Fill with asterisk. Align `<` left, `>` right, `^` center
  std::println("{0:*<8},{0:*>8},{0:*^8}", a);
  // Sign `+` show plus explicitly, ` ` (space) add leading space, `-` default.
  std::println("{0:},{0:+},{0: }\n{1:},{1:+},{1: }", 1, -1);
  // `#` alternate the integer form
  // `#x` hex, `#b` bin, `#d` dec, `#0x` padding leading zeros (only field witdh)
  std::println("{0:#010x}", 1);
  // The precision `.`: for float is precision, for string is the upper bound to be output
  std::println("{0:#015.2f},{0:#015.2e}", 114514.1919810);   // minimal width 15, precision 2, padding with zeros, `f` std::fixed format, `e` scientific format
  std::println("{0:.<15.2s}", "114514.1919810"); // minimal width 15, precision 2, align left, padding with dots
  std::println("{:.<5.5s}", "🐱🐱🐱");           // "🐱🐱.", because emoji has two char wide
  // Type
  std::println()
}
```

## Compiler Optimization

- `[[likely]]`/`[[unlikely]]` used in if-statements and loops whith logical decision.
- `volatile` to tell compiler don't optimize.
- `constexpr` declares that it is possible to evaluate the value of the function or variable at compile-time. Such variables nd functions can then be used where only compile-time.
- `consteval` force evaluate expression in compile-time.
- `noexcpt` for function that never `throw` error. Destructors are implicitly `noexcept`.
  Due to strong exception guarantee, `std::vector` moves its elements (calls `Object(Object&& (calls `Object(Object&&)`))`) at rearrange only if their move constructors are `noexcept`. You cna use `static_assert(std::is_nothrow_move_constructible_v<Object>);` to check.
- `constinit` enforces variable is initialized at compile-time. Unlike `constexpr`, it allows non-trivial destructors. Therefore it can avoid the problem that the order of initialiation of static variables from different translation units is undefined, by initialize them at compile-time.
   Another use-case is with non-initializing `thread_local` declarations. In such a case, it tells the compiler that the variable is already initialized, otherwise the compiler usually adds code the check and initialize it if required on each usage.

```c++
namespace custom {
constexpr double pow(double x, long n) noexcept {
  if (n > 0) return x * pow(x, n - 1);
  else return 1;
}
constexpr long fact(long n) noexcept {
  if (n > 1) return n * fact(n - 1);
  else return 1;
}
constexpr double cos(double x) noexcept {
  constexpr long precision{16L};
  double y{};
  for (auto n{0L}; n < precision; n += 2L) y += pow(x, n) / (n & 2L ? -fact(n) : fact(n));
  return y;
}
} // namespace custom
double gen_random() noexcept {
  static std::random_device rd;
  static std::mt19937 gen(rd());
  static std::uniform_real_distribution<double> dis(-1.0, 1.0);
  return dis(gen);
}
volatile double sink{}; // ensures a side effect
int main() {
  // Check the consistency with std::cos()
  for (const auto x : {0.125, 0.25, 0.5, 1. / (1 << 26)})
    std::print("x = {1}\n{2:.{0}}\n{3:.{0}}\nIs equal: {4}\n", 53, x, std::cos(x), custom::cos(x), std::cos(x) == custom::cos(x));
  auto benchmark = [](auto&& fun, auto rem) {
    auto const start{std::chrono::high_resolution_clock::now()};
    for (auto size{1UL}; size != 10'000'000UL; ++size) sink = fun(gen_random());
    std::chrono::duration<double> const diff{std::chrono::high_resolution_clock::now() - start};
    std::println("Time: {:f} sec {}", diff.count(), rem);
  };
  benchmark(custom::cos, "(custom::cos)");
  benchmark([](double x) { return std::cos(x); }, "(std::cos)");
}
```

```c++
struct S {
  constexpr S() {}
  ~S() {} // Without constexpr makes it non-trivial
};

// constexpr S s1{}; // Error because destructor is non-trivial
constinit S s1{}; // OK

// tls_definitions.cpp
thread_local constinit int tls1{1};
thread_local int tls2{2};

// main.cpp
extern thread_local constinit int tls1;
extern thread_local int tls2;

int get_tls1() { return tls1; } // pure TLS access
int get_tls2() { return tls2; } // has implicit TLS initialization code
```

## Three-way comparison

Comparison categories (The `operator<=>()`'s return types)
- `strong_ordering`: exactly one if `a < b`, `a == b`, `a > b` must be true and if `a == b` then `f(a) == f(b)`.
- `weak_ordering`: exactly one if `a < b`, `a == b`, `a > b` must be true and if `a == b` then `f(a)` not neccessary equal to `f(b)`.
- `partial_ordering`: none of `a < b`, `a == b`, `a > b` might be true (may be incomparable) and if `a == b` then `f(a)` not neccessarily equal to `f(b)`. e.g. In `float`/`double` the `NaN` is not comparable

```c++
template <typename T1, typename T2> requires requires(T1 a, T2 b) { a<b, a <= b, a> b, a >= b, a == b, a != b; }
void f() {}

struct S2 {
  int a, b;
};
// Compiler can replace calls to <, <=, >, >= to operator<=>(), and calls to ==, != to
// operator==(). For example:
// a < b to a <=>b < 0
// a!= b to !(a <=> b == 0)
struct S1 {
  int a, b;
  constexpr auto operator<=>(S1 const&) const = default; // Homogeneous comparisons
  constexpr bool operator==(S1 const&) const = default; // Note: Defaulted operator<=>() also declares
  // defaulted operator==(), but here the operator==(S2 const&) prevents implicit default
  constexpr std::strong_ordering operator<=>(S2 const& other) const { // Heterogeneous comparisons
    if (auto cmp = a <=> other.a; cmp != 0) return cmp;
    return b <=> other.b;
  }
  constexpr auto operator==(S2 const& other) const {
    return (*this <=> other) == 0;
  }
};
int main() {
  f<S1, S1>();
  f<S1, S2>();
}
```

## Modules

Module units whose declaration has `export` are termed *module interface units*; all other module units are termed `module implementation units`.

```c++ hello.cpp
export module hello.cpp; // dots are four readability purpose
export imort :helpers; // re-export heplers partition (see below)
import :impl;
export void f() { utility(); }
```

```c++ hello.impl.cpp
module; // Global module fragment (include classical header files)
#include <vector>
module hello.cpp:impl; // Module implementation partition unit
void utility() {};
```

```c++ hello.helpers.cpp
export module hello:helpers; // Module interface partition unit
import :internals;
export void g() { utility(); }
```

```c++ main.cpp
import hello.cpp;
main () {
  f(), g();
}
```

### References

1. [C++ by The Cherno](https://www.youtube.com/playlist?list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb)
2. [All C++20 core language features with examples](https://oleksandrkvl.github.io/2021/04/02/cpp-20-overview.html#attr-likely)
3. [c++20 の coroutine 使ってみる](https://qiita.com/ousttrue/items/0572c7cec966bb33067f)
