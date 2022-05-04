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
