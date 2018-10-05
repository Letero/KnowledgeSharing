- [Preparation](#preparation)
- [C](#c)
  * [1. Difference between initial values of unitialized static and non-static variables](#1-difference-between-initial-values-of-unitialized-static-and-non-static-variables)
  * [2. Does *const volatile* make any sense? IQN](#2-does--const-volatile--make-any-sense--iqn)
- [CPP](#cpp)
- [Base knowledge](#base-knowledge)
  * [1. Memory layout of C programs /IQN](#1-memory-layout-of-c-programs--iqn)
  * [2. Stack](#2-stack)
  * [3. Heap](#3-heap)
  * [4.](#4)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>


# Preparation

**[How to format this file](https://guides.github.com/features/mastering-markdown)**

**[Awesome README](https://github.com/matiassingers/awesome-readme)**

**[Generate Table of Content](https://ecotrust-canada.github.io/markdown-toc)**

# C


## 1. Difference between initial values of unitialized static and non-static variables

Keep in mind that we are talking about **unitialized** variables!
Checkout out [Memory layout](#1-memory-layout-of-c-programs--iqn), you will be able to understand why it works this way.

Each variable in C must have:
 *  Name
 *	Type
 *	Value
 
 I repeat: each variable in C must have a value!
 * **Static variables** - both global (file) scope and local (eg. function) scope are automatically initialized with 0.
 * **Non-static global** variables are initialized with 0.
 * **Non-static local** variables are usually initialized with random value from stack.
 

```c
static int s_global;      // global static variables are initalized with 0
int global;               // global non-static variables** are initialized with 0
 
void foo()
{
	static int s_local;   // static local variables are initialized with 0, just like global variables
	int local;            // non-static local variable's value will be indetermined, but usually it is random value from stack
	
	printf("static local: %i\n", s_local);
	printf("local: %i\n", local);
}

int main(int argc, char* argv[])
{
	foo();
	printf("global: %i\n", s_global);
	printf("global: %i\n", global);
	return 0;
}
```
## 2. Does *const volatile* make any sense? IQN

That is kind of tricky question.

Simply put: yes. *Volatile* keyword itself prevents an optimizing compiler from optimizing away subsequent reads or writes and thus incorrectly reusing a stale value or omitting writes. Thanks to *const* we make sure that programmer won't be able to change it by mistake.

Usually we use *const* specifier to make sure that variable's value won't be changed.

Value of *const volatile* variable might be changed during execution of program, what might make you think that using it doesn't make any sense. In this case we use *const* mainly to prevent programmer from doing unacknowledged mistake, rather than to make sure value won't be changed at all.


# CPP

# Base knowledge

## 1. Memory layout of C programs /IQN




## 2. Stack

## 3. Heap

## 4. 
