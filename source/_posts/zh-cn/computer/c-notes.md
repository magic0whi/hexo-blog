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

| Data Types                       | Length (Bytes) | Range               | Format Specifier             |
| --                               | --             | --                  | --                           |
| `\[signed\] int`                 | 4              | -(2^31)~(2^31-1)    | %d (Dec), %o (Oct), %x (Hex) |
| `unsigned int`                   | 4              | 0~(2^32-1)          | %u                           |
| `short \[int\]`                  | 2              | -(2^15)~(2^15-1)    | %hd                          |
| `unsigned short \[int\]`         | 2              | 0~(2^16-1)          | %hu                          |
| `\[long\] long \[int\]`          | 8              | -(2^63)~(2^63-1)    | %[l]ld                       |
| `unsigned \[long\] long \[int\]` | 8              | 0~(2^64-1)          | %[l]lu                       |
| `float`                          | 4              | -3.4e-38~3.4e38     | %f                           |
| `double`                         | 8              | -1.7e-308~1.7e308   | %lf                          |
| `long double`                    | 16             | -1.2e-4932~1.7e4932 | %Lf                          |
| `char`                           | 1              | -128~127            | %c                           |
| `unsigned char`                  | 1              | 0~255               | %c                           |
| `char *` (String)                | NA             | NA                  | %s                           |

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

## Math Function in C

`#include <math.h>`

- `hypot(a,b)` Return hypotenuse length from its neighbor edge a, b.
- `sqrt(a)`

## Basic Types, Array, Object

```c
// Variable definition auto create a memory space which has size equal to its data type (for integer it is 4 bytes)
int i = 233;

int j = i;   // equivalent to  int j = 233;
int *k = &i; // the address of variable 'i'

int l[3] = {1, 2, 3}; // 'l' is a pointer to the address of first element in array, its type is 'int *'
int *m = l;           // '&l' is not needed since 'l' has type 'int *'

// No, there is no objects in C
Object l;   
Object *m; 
// BTW, in C++ what object variable store is the copy function of that object. By default it copies all its member variables' value into new object
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
- `str{lwr,upr}(char *)`: Lowercase / Uppercase a char array.

