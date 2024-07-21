---
title: c-notes
toc: true
category: computer
date: 2022-05-03 16:55:46
tags:
---

Workflow: C + GDB + Makefile

<!-- more -->

## Makefile Introduction

```makefile
# Variables
# Simple assignment ':=' and Recursive assignment '=' (More likely a pointer)
A := Foo
B := ${A}
C = ${A}
A = Bar
# Result: A = Bar, B = Foo, C = Bar

# Conditional assignment '?=', assigns only if does not have a value.
A ?= Fooo
# Result: A = Bar

# Appending '+='
A += Foo
# Result: A = Bar Foo

# ${A} and $(A) are the same, I personally like to use '$()' to express that there is a function called.

# Explicity tell Make those are phony target. So Make will not be confused if there is a file called 'clean' in the directory
.PHONY: clean
# The target first appears in Makefile is the default target, this means barely run 'make' will execute target 'all'.
# We can override this behavior by using a special phony target called '.DEFAULT_GOAL'
#.DEFAULT_GOAL := sub_target
all: sub_target target.c
    Recipe

# prequisites is the required files or other sub targets or a local variable.
sub_target: LOCAL_VAR = Foobar
sub_target: prequisites
    Recipe

# For targets don't build a file is called phony target, mostly 'clean' is like to be a phony target
phony_target:
    # Use '@' to suppress echoing
    @echo "Hello World"
```

### A Common Makefile for C

```makefile
.PHONY = all clean

CC = gcc
LINKERFLAG = -lm

# '$(wildcard pattern)' is one of the functions for filenames.
# In this case all files with the '.c' extension will be stored in variable '${SRCS}'
SRCS := $(wildcard *.c)
# '$(VARNAME:%pattern1=pattern2%)' is substitution reference
# In this case it removes the extension '.c'
BINS := $(SRCS:%.c=%)
# Same as
#BINS := $(basename ${SRC})

all: ${BINS}

# '%' matches any target name, '$<' is a patten to match first prerequisite, '$@' matches the target name
%: %.o
    @echo 'Linking...'
    ${CC} ${LINKERFLAG} $< -o $@
# If ${BINS} = foo bar, then it is equavalent to
#foo: foo.o
#    @echo 'Linking...'
#    gcc -lm foo.o -o foo
#
#bar: bar.o
#    @echo 'Linking...'
#    gcc -lm bar.o -o bar

# The targets order does not manner
%.o: %.c
    @echo 'Compile to objects...'
    ${CC} -c $<

clean:
   @echo 'Cleaning up...'
   rm -rvf *.o ${BINS}
```

## Basic Data Types and its length

| Data Types                                      | Length (Bytes) | Range               | Format Specifier             |
| --                                              | --             | --                  | --                           |
| `\[signed\] int`                                | 4              | -(2^31)~(2^31-1)    | %d (Dec), %o (Oct), %x (Hex) |
| `unsigned int`                                  | 4              | 0~(2^32-1)          | %u                           |
| `short \[int\]`                                 | 2              | -(2^15)~(2^15-1)    | %hd                          |
| `unsigned short \[int\]`                        | 2              | 0~(2^16-1)          | %hu                          |
| `\[long\] long \[int\]`                         | 8              | -(2^63)~(2^63-1)    | %[l]ld                       |
| `unsigned \[long\] long \[int\]` (aka `size_t`) | 8              | 0~(2^64-1)          | %[l]lu                       |
| `float`                                         | 4              | -3.4e-38~3.4e38     | %f                           |
| `double`                                        | 8              | -1.7e-308~1.7e308   | %lf                          |
| `long double`                                   | 16             | -1.2e-4932~1.7e4932 | %Lf                          |
| `char`                                          | 1              | -128~127            | %c                           |
| `unsigned char`                                 | 1              | 0~255               | %c                           |
| `char *` (String)                               | NA             | NA                  | %s                           |

Different compilers may differ, this results was come from GCC11.2.0 (x86\_64), you can test by using the C codes below:
```c
#include <stdio.h>

int main()
{
    int a;
    signed int b;
    unsigned int c;

    short d;
    long e;
    long long f;
    
    float g;
    double h;
    long double i;

    char j;
    printf("int size %lu bytes \n", sizeof(a));
    printf("signed int size %lu bytes \n", sizeof(b));
    printf("unsigned int size %lu bytes \n", sizeof(c));
    printf("short size %lu bytes \n", sizeof(d));
    printf("long size %lu bytes \n", sizeof(e));
    printf("long long size %lu bytes \n", sizeof(f));
    printf("float size %lu bytes \n", sizeof(g));
    printf("double size %lu bytes \n", sizeof(h));
    printf("long double size %lu bytes \n", sizeof(i));
    printf("char size %lu bytes \n", sizeof(j));
}
```

Some notes on format specifier:
- `%m.ns`: Output string, has length `>=m` (add space from left if less), only read left `n` chars in a string.
- `%-m.ns`: Same as above, but add space from right if less than `m`.
- `%m.nf`: Same as above, but for float variables.
- `%e`: Shows the data in exponential form.
- `%g`: Auto select `%f` or `%e` determined by size.
- `%%`: Twice `%` to print '%'.

## C Compound Assignment

Ten Compound Assignment: `{+,-,*,/,%,<<,>>,&,^,|}=`.

It was computed from right to left:
```c
int a=12;

a+=a*=a/=a-6;
// Is same as
a=a/(a-6); // a=12/(12-6)=2
a=a*a;     // a=2*2=4
a=a+a;     // a=4+4=8
```

## Assignment Expression Does NOT Do Variable Define

```c
// Not allowed
int a=b=c=5;

// Allowed
int b=2,c=3;
int a=b=c=5;
```

## Library Functions in C

Mathematics:
`#include <math.h>`
- `hypot(a,b)` Return hypotenuse length from its neighbor edge a, b.
- `sqrt(a)`
- `int abs(int x)` / `long labs(long x)` / `double fabs(double x)`
- `double `{`sin`,`cos`,`tan`}`(double x)`: Here `x` is radian.
- `double `{`exp`,`log`,`log10`}`(double x)`: \\(e^x\\) / \\(\ln x\\) / \\(\log_{10}x\\)

Memory:
`#include<stdlib.h>`
- `void *malloc(unsigned int size)` allocate and return the memory address.
- `void free(void *p)`

- `int system(const char *command)` running a shell command, return -1 if failed.

## Basic Types, Array, Object

```c
// Variable definition auto create a memory space which has size equal to its data type (for integer it is 4 bytes)
int i=233;

int j=i;   // equivalent to  int j = 233;
int *k=&i; // the address of variable 'i'

int l[3]={1,2,3}; // 'l' is a pointer to the address of first element in array, its type is 'int *'
int *m=l;         // '&l' is not needed since 'l' has type 'int *'

// Be aware for char pointer arrays
char *str={'h','e','l','l','o','\0'}; // This is wrong, indeed it is same as 'char *str = 'h';'
// Using
char *str=(char[]) {'h','e','l','l','o','\0'};
// Or
char str[]={'h','e','l','l','o','\0};
// Or
const char *str="hello"; // "string" implicitly creates a const memory area

// No, there is no objects in C
Object l;   
Object *m; 
// BTW, in C++ what object variable store is the copy construct function of that object '=()'. By default it copies all its member variables' value into new object

int (*n)[5]; // Create a pointer 'n' point to type 'int (*)[5]' and 'int [][5]' (a 2-dimensional integer array which explicitly has 5 columns)

struct time
{
    long int day;
    long int hour;
}

struct time time1 = {1,0}; // struct variable does not store address, it might be only available in preprocessing stage.

struct time time2 = time1;
// Same as (similar to copy construct function in C++)
struct time time2;
time2.day=time1.day;
time2.hour=time1.hour;
```

## Frequently Used Functions

### IO

`#include<stdio.h>`

- `scanf("pattern",&address)`
  Default delimiter is space ' ', you can change it by using an pattern, for example, to use comma as delimiter:
  ```c
  float x, y, z;
  scanf("%f,%f,%f",&x,&y,&z); // Then you can type '1.0,2.0,3.0' in terminal
  ```

### String

- `puts(char *)`: Ouput a char array to STDOUT
- `gets(char *)`: Similar to `scanf()` but use `\n` as delimiter instead of space ' '.
- `strcat(char *str1,char *str2)`: Corcatenate `str2` to the end of `str1`, changes apply to `str1`, return `str1`.
- `strcmp(char *str1,char *str2)`: Compare by first different char (using sort of ASCII), return 0 if `str1`=`str2`; return positive if `str1`>`str2`; return negative if `str1`<`str2`.
- `strlen(char *)`: Return the length of a char array.
- `str`{`lwr`,`upr`}`(char *)`: Lowercase / Uppercase a char array.
- `char *strchr(char *str, char ch)`: Return a pointer to the first occurance of the character `ch` in string `str` or return `NULL` if the character doesn't found.
- `int isalpha(int ch)`: Return non-zero if `ch` is a letter in alphabet.
  `int isdigit(int ch)`
  `int isalnum(int ch)`: Return non-zero if `ch` is whether a letter or number.

## Functions

- If a function has type `int`, there is not necessary to have a declearation first (I defenitely recommand to have a declaration first):
  ```c
  #include<stdio.h>
  int main()
  {
      int len;
      char str[100];
      printf("please input a string:\n");
      gets(str);
      len=length(str);
      printf("the string has %d characters.",len);
  }
  int length(char *p)
  {
      int n=0;
      while(*p!='\0')
      {
          n++;
          p++;
      }
      return n;
  }
  ```
 - There is a little bit clearer way to describe a recursion
   We have this code to do a factorial:
   ```c
   int factor1(int n)
   {
       int result;
       if(n==1)
           return 1;
       result=factor(n-1)*n;
       return result;
   }
   ```
   Now we describe it by using a "stair graph"(assume we call `factor(5)`):
   First stage we go down the stair
   ```plaintext
   factor(5)=factor(4)*5
   ----------
            | factor(4)=factor(3)*4
            ----------
                     | factor(3)=factor(2)*3
                     ----------
                              | factor(2)=factor(1)*2
                              ----------
                                       | factor(1)=1
                                       ----------

                  Down the stair
   ```
   Second stage we go up the stair
   ```plaintext
                                 factor(5)=24*5=120 |
                                           ----------
                          factor(4)=6*4=24 |
                                  ----------
                  factor(3)=2*3=6 |
                         ----------
         factor(2)=1*2=2 |
               ----------
   factor(1)=1 |
      ----------
                  Up the stair
   ```

## Veriable Store Types

- `auto`: For C language it is the default store types for variables. `int i;` equals `auto int i;`
- `static`: For local variable its memory was keep along the program running. For global variables and functions it means that they are only allowed to be called in this translation unit. (Local function)
- `register`: Variable will try to store in Registers. Optimize for frequency accessed variable.
- `extern`: TODO

## Struct & Union & Emum

- Struct format:
  ```c
  #include<stdio.h>
  
  //struct <[struct_name]>
  //{
  //} [<var1,var2,...>];
  
  struct Student
  {
      int num;
      char name[20];
      char sex;
      int age;
      float score;
  };
  
  int main()
  {
      struct student student1={1001,"Liming",'M',20,93.5};
      printf("num: %d\n",student1.num);
      printf("name: %s\n",student1.name);
      printf("sex: %c\n",student1.sex);
      printf("age: %d\n",student1.age);
      printf("score: %5.1f\n",student1.score);
  }
  ```
- Struct array
  ```c
  struct student
  {
      int num;
      char name[20];
      float score;
  } stu[5] =
      {
          {101,"liming",89},
          {102,"zhanghong",95},
          {103."lili",89},
          {104,"weichen",85},
          {105,"yangfan",75}
      };
  ```
- Struct pointer
  ```c
  struct student *p;
  p=&student1;
  printf("%s",(*p).name);
  printf("%s",p->name);
  ```
- Linked list
  ```c
  struct Student
  {
      char name[15];
      float mark;
      struct student *next;
  };
  typedef struct Student Node;
  typedef Node *Link;
  // Or
  typedef struct Student
  {
      char name[15];
      float mark;
      struct student *next;
  } Node, *Link;
  ```
- Union: their members share the same memory
  ```c
  union <[union_name]>
  {
      char name[];
      int age;
      char sex;
      float labor_age;
  } [<union_var1, union_var2,...>]
  ```
- Enum:
  ```c
  //enum <[enum_name]>
  //{
  //    E1,
  //    E2,
  //    E3
  //} [<enum_var1,enum_var2,...>]

  enum Weekday
  {
      SUN,MON,TUE,WED,THU,FRI,SAT
  }
  ```

## Bit Operation

- And `&` can be used as a filter.
- XOR `^` can be used to flip specific bits:
  ```plaintext
    01000101
  ^ 00001111
  ----------
    01001010
  ```
- Right shift `>>` do logical shift (for unsigned) or arithmetic shift (for signed; fill '1' on the leftmost bits)
- Bit fields
  ```c
  struct packed_data
  {
      // This bit fields distribute four bytes (see 'sizeof(unsigned int)'). They must be defined continuously, and can only have types '[signed] int', 'unsigned int'.
      unsigned int a:2;
      unsigned int b:1;
      unsigned int c:1;
      int i;
  }

  // It has the follow memory form:
  // -------------------------------------------------------------------------------------------------------------
  // |  a  |  b  | c |                        Not used                         |                i                |
  // -------------------------------------------------------------------------------------------------------------
  //    |     |    |                             |                                              |
  //   2bit  2bit 1bit                          24bit                                         32 bit
  ```

## Macro

- `#define` with parameters
  ```c
  #include<stdio.h>
  #define MIN(a, b) (a<b)?a:b
  #define SWAP(a,b) {int c;c=a;a=b;b=c;}

  // When you call SWAP(x,y), it just paste '{int c;c=x;x=y;y=c;}'
  // It doesn't paste text in ""
  // e.g. printf("SWAP(x,y)");
  ```
- Conditional compilation
  ```c
  #define NUM 200
  main()
  {
  #if NUM>100
      printf("the number is larger than 100!");
  #elif NUM==100
      printf("the number is equal to 100!");
  #else
      printf("the number is less than 100");
  #endif
  ```
  `#ifdef MACRO_NAME`: True if `MACRO_NAME` is defined.
  `#ifndef MACRO_NAME`: True if `MACRO_NAME` is not defined.
  `#undef MACRO_NAME`: Delete a defined macro `MACRO_NAME`.

## File

- Write / Read a file by char 
  - `int fputs(const char *filename, FILE *fp)`
  - `int fgetc(FILE *fp)`: Read a single char
  - Example:
    ```c
    #include<stdio.h>
    #include<stdlib.h>
    int main()
    {
        FILE *fp; // File pointer
        char ch;
        char filename[]="text.txt";

        // Write Start
        if((fp=fopen(filename,"w"))==NULL)
        {
            printf("cannot open file\n");
            exit(1);
        }
        ch=getchar();
        ch=getchar();
        while(ch!='#')
        {
            fputc(ch,fp);
            ch=getchar();
        }
        fclose(fp);
        // Write End

        // Read Start
        if((fp=fopen(filename,"r"))==NULL)
        {
            printf("cannot open file\n");
            exit(1);
        }
        while((ch=fgetc(fp))!=EOF)
        {
            putchar(ch);
        }
        fclose(fp);
        // Read End
    }
    ```
- Write / Read a file by string
  - `int fputs(const char *filename, FILE *fp)`
  - `char *fgets(char *str, int n, FILE *fp)`: Store to char array `str`
  - Example:
    ```c
    #include<stdio.h>
    #include<stdlib.h>
    int main()
    {
        FILE *fp;
        char str[100];
        char filename[]="test.txt";
        
        // Write Start
        if((fp=fopen(filename,"w"))==NULL)
        {
            printf("cannot open file\n");
            exit(1);
        }
        fputs("hello world", fp);
        fclose(fp);
        // Write End

        // Read Start
        if((fp=fopen(filename,"rb"))==NULL)
        {
            printf("cannot open file\n");
            exit(1);
        }
        while(fgets(str,sizeof(str),fp))
            printf("%s",str);
        fclose(fp);
        // Read End
    }
    ```
- Open mode:
  {`r`,`w`,`a`}[`b`] {read,write,append} only (binary)
  {`r`,`w`}[`b`]`+` read & write (binary)
  `a`[`b`]`+` read & append (binary
- Write / Read a file by block
  - `size_t fwrite(void *buf, size_t write_size, size_t count, FILE *fp)`
  - `size_t fread(void *buf, size_t read_bytes, size_t count, FILE *fp)`
  - Example:
    ```c
    #include<stdio.h>
    #include<stdlib.h>
    
    struct student_score
    {
        char Name[10];
        int Num;
        int Chinese;
        int Math;
        int English;
    } score[100];
    
    int main()
    {
        FILE *fp;
        int i, n;
        char filename[50];
    
        printf("how many students in your class?\n");
        scanf("%d", &n);
        printf("please input filename:\n");
        scanf("%s", filename);
        printf("please input Name, Number, Chinese, Math, English:\n");
        for(i=0; i<n; i++)
        {
            printf("NO%d", i);
            scanf("%s%d%d%d%d", score[i].Name, &score[i].Num, &score[i].Chinese, &score[i].Math, &score[i].English);
        }
    
        // Write Start
        if((fp=fopen(filename, "wb")) == NULL)
        {
            printf("cannot open file\n");
            exit(1);
        }
        for(i=0; i<n; i++)
            if(fwrite(score, sizeof(struct student_score), 1, fp) != 1)
                printf("file write error\n");
        fclose(fp);
        // Write End
        
        // Read Start
        if((fp=fopen(filename,"rb")) == NULL)
        {
            printf("cannot open file\n");
            exit(1);
        }
        for(i=0; i<n; i++)
        {
            fread(score, sizeof(struct student_score), 1, fp);
            printf("%-10s%4d%4d%4d%4d\n",score[i].Name, score[i].Num, score[i].Chinese, score[i].Math, score[i].English);
        }
        fclose(fp);
        // Read End
    }
    ```
    Input Example:
    ```plaintext
    how many students in your class?
    5
    please input filename:
    test.bin
    please input Name, Number, Chinese, Math, English:
    lili 1001 89 69 86
    mimi 1002 66 56 98
    qiqi 1003 99 56 23
    yuyu 1004 69 88 56
    xixi 1005 85 65 79
    ```
- Write / Read a file in random position
  - `int fseek(FILE *fp, long int offset, int whence)`
    The `whence` has three enums: `SEEK_SET` (Beginning of file), `SEEK_CUR` (Current position), `SEEK_END` (End of file).
  - `int feof(FILE *fp)` return non-zero if `fp` is in End-of-File.
  - `void rewind(FILE *fp)` set `fp` to beginning of the file.
  - `long int ftell(FILE *fp)` return `fp`'s current position.
  - `int ferror(FILE *fp)` return non-zero if some operation (read, write) to `fp` result an error.

  
