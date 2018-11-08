- [Preparation](#preparation)
- [C](#c)
  * [1. Difference between initial values of unitialized static and non-static variables](#1-difference-between-initial-values-of-unitialized-static-and-non-static-variables)
  * [2. IQN Does *const volatile* make any sense?](#2-iqn-does--const-volatile--make-any-sense-)
  * [3. IQN Void* pointer in C](#3-iqn-void--pointer-in-c)
  * [4. IQN Register keyword](#4-iqn-register-keyword)
  * [5. Auto in C?](#5-auto-in-c-)
  * [16. Token](#16-token)
- [CPP](#cpp)
  * [1. Copy elision](#1-copy-elision)
  * [2. Virtual Destructor](#2-virtual-destructor)
  * [3. IQN When will copy constructor be called?](#3-iqn-when-will-copy-constructor-be-called-)
  * [4. IQN When will deconstructor be called?](#4-iqn-when-will-deconstructor-be-called-)
  * [5. IQN Differences: free vs delete](#5-iqn-differences--free-vs-delete)
  * [6. IQN Differences: malloc vs new](#6-iqn-differences--malloc-vs-new)
  * [7. IQN Differences: Free store and heap.](#7-iqn-differences--free-store-and-heap)
  * [8. IQN Singleton](#8-iqn-singleton)
  * [9. Restrict keyword - what is it used for?](#9-restrict-keyword---what-is-it-used-for-)
  * [10. What areas are not statically (compile-time) type safe?](#10-what-areas-are-not-statically--compile-time--type-safe-)
- [Base knowledge](#base-knowledge)
  * [1. IQN Memory layout of C/CPP programs](#1-iqn-memory-layout-of-c-cpp-programs)
  * [2. Compilation process C/CPP](#2-compilation-process-c-cpp)
  * [3. Preprocessor directives](#3-preprocessor-directives)
  * [4. IQN Translation unit](#4-iqn-translation-unit)
  * [5. IQN Binary operations](#5-iqn-binary-operations)
  * [6. Endianness](#6-endianness)
  * [7. IQN Memory allocation for 2D and 3D array](#7-iqn-memory-allocation-for-2d-and-3d-array)
  * [8. Include guard](#8-include-guard)
  * [9. Structural padding and packing](#9-structural-padding-and-packing)
  * [10. Dangling pointer](#10-dangling-pointer)
  * [11. Wild pointer](#11-wild-pointer)
- [Interesting facts](#interesting-facts)
  * [1. Keyword "import"](#1-keyword--import-)
  * [2. Odd array indexing](#2-odd-array-indexing)
  * [3. Token whitespace](#3-token-whitespace)

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
## 2. IQN Does *const volatile* make any sense?

That is kind of tricky question.

Simply put: yes. *Volatile* keyword itself prevents an optimizing compiler from optimizing away subsequent reads or writes and thus incorrectly reusing a stale value or omitting writes. Thanks to *const* we make sure that programmer won't be able to change it by mistake.

Usually we use *const* specifier to make sure that variable's value won't be changed.

Value of *const volatile* variable might be changed during execution of program, what might make you think that using it doesn't make any sense. In this case we use *const* mainly to prevent programmer from doing unacknowledged mistake, rather than to make sure value won't be changed at all.

## 3. IQN Void* pointer in C

TODO!
[Link for now](https://bytes.com/topic/c/answers/872557-what-use-void-pointer)

## 4. IQN Register keyword

[Link](https://stackoverflow.com/a/578213)

## 5. Auto in C?

What is keyword auto for?
**auto** is modifier like for e.g. **static**. 
By default every local variable of the function is automatic (auto). In the below function both the variables ‘i’ and ‘j’ are automatic variables.
```c
void f() {
   int i;
   auto int j;
}
```
*auto* is archaic, it exists in C because before the C language there was a **B language** in which that keyword was necessary for declaring local variables. (B was developed into NB, which became C).

## 16. Token

A C program consists of various tokens and a token is either a keyword, an identifier, a constant, a string literal, or a symbol.

Even space can be token - checkout [3. Token whitespace](#3-token-whitespace) to see interesting example.

# CPP

## 1. Copy elision

Copy elision is compiler optimization technique in c++ programming. It ommits unnecessary copy of object. Copy can be elided even if it contains crucial logic. 
To disable copy elision you should compile the program using flag: "-fno-elide-constructors"
	
```cpp
#include <iostream>

class Base
{
public:
	Base()
	{
		value = 0;
		std::cout << "Default constructor" << std::endl;
        }
	Base(const Base& base)
	{
		value = base.value;
		++value;
		std::cout << "Copy constructor" << std::endl;
	}
		
	void printValue()
	{
		std::cout << "Value: " << value << std::endl;
	}
			
private:
	int value;
};

Base func()
{
        Base b;
	return b;
}

int main()
{
	Base b = func();
	b.printValue();
	return 0;
}
```
	
![Output ](https://github.com/Letero/KnowledgeSharing/blob/master/Images/ce.png)

This topic is not finished yet.


## 2. Virtual Destructor

If base class does not have virtual destructor and we delete derived class instance through base type pointer the **behaviour is undefined**. Most compiler implementations will call base destructor.
If class has even one virtual method, then it should have virtual destructor.


![Example ](https://github.com/Letero/KnowledgeSharing/blob/master/Images/VirtualDestructor.png)


## 3. IQN When will copy constructor be called?

TODO!
[Link for now](https://stackoverflow.com/questions/21206359/in-which-situations-is-the-c-copy-constructor-called)

## 4. IQN When will deconstructor be called? 

TODO!
[Link for now](https://stackoverflow.com/questions/10081429/when-is-a-c-destructor-called)


## 5. IQN Differences: free vs delete

TODO!
[Link for now](https://stackoverflow.com/questions/328834/c-delete-vs-free-and-performance)


## 6. IQN Differences: malloc vs new

TODO!
[Link for now](https://www.geeksforgeeks.org/malloc-vs-new/)

## 7. IQN Differences: Free store and heap.

Well, [this should be complete answer](https://stackoverflow.com/questions/1350819/c-free-store-vs-heap).

I will write summary here later on.

## 8. IQN Singleton

TODO!
[Link for now!](https://stackoverflow.com/questions/1008019/c-singleton-design-pattern)

## 9. Restrict keyword - what is it used for?

Restrict keyword is used in pointer declaration. It tells that this pointer is the only way to access object that it's pointing to.
It does not add new functionality
Compiler won't prevent programmer from creating another pointer to restricted address. It is promise made by programmer.
Not following to restrict contract results in undefined behavior.
Restrict keyword is used so compiler can make optimizations.
It was defined in C99 standard

## 10. What areas are not statically (compile-time) type safe?

Ideally, a program would be completely statically (compile-time) type safe. Unfortunately, that is not possible. Problem areas:

* unions
* casts
* array decay
* range errors
* narrowing conversions

These areas are sources of serious problems (e.g., crashes and security violations)

# Base knowledge

## 1. IQN Memory layout of C/CPP programs

For now use this source, decent explanation
[Link](https://www.geeksforgeeks.org/memory-layout-of-c-program/)

I will elaborate on this once I'm done with binary operations.

## 2. Compilation process C/CPP

TODO!
[Link for now](https://www.youtube.com/watch?v=wDKeJ79TBsg)

## 3. Preprocessor directives

TODO!
[Link for now](http://www.cplusplus.com/doc/tutorial/preprocessor/)

## 4. IQN Translation unit

TODO!
[Link for now](https://stackoverflow.com/questions/8342185/translation-unit-in-c-and-c)

## 5. IQN Binary operations

TODO!
[Link for now](https://github.com/Letero/Small-tasks/blob/master/BitwiseOperations/BitwiseOperations.c)

## 6. Endianness

[Link](https://www.geeksforgeeks.org/little-and-big-endian-mystery/)

## 7. IQN Memory allocation for 2D and 3D array

TODO!
[Link to example](https://github.com/Letero/Small-tasks/tree/master/Allocation)

## 8. Include guard

[Link](https://en.wikipedia.org/wiki/Include_guard)

## 9. Structural padding and packing

TODO!
[Link for now](https://www.youtube.com/watch?v=QSuBwGmFQqA)

## 10. Dangling pointer

In short it is a pointer pointing to non-existing memory location.

Dangling pointers arise when an object is deleted or de-allocated, without modifying the value of the pointer, so that the pointer still points to the memory location of the de-allocated memory. 

It creates the problem because the pointer is still pointing the memory that is not available. When the user tries to dereference the daggling pointers than it shows the undefined behavior and can be the cause of the segmentation fault.

## 11. Wild pointer

A pointer that is not initialized properly prior to its first use is known as the wild pointer. Uninitialized pointers behavior is totally undefined because it may point some arbitrary location that can be the cause of the program crash, that’s is the reason it is called a wild pointer.

In the other word, we can say every pointer in programming languages that are not initialized either by the compiler or programmer begins as a wild pointer.

Good practice is to initialize new pointer with **NULL** if you don't want to assign some other value to it at the moment of declaration.

# Interesting facts

## 1. Keyword "import"

[Link](https://stackoverflow.com/questions/39280248/what-is-the-difference-between-import-and-include-in-c)

## 2. Odd array indexing

Look at this code: 

```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
  int *arr = (malloc(sizeof(int) * 3));
  2[arr] = 55;
  printf("%d %d\n", arr[2], 2[arr]);
  
  free(arr);
  return 0;
}
```
Output:
```c
Success	#stdin #stdout 0s 9424KB
55
55
```

Why does it work this way?
[Link to the answer](https://stackoverflow.com/questions/381542/with-arrays-why-is-it-the-case-that-a5-5a)

## 3. Token whitespace

```c
#include <stdio.h>

int main(void) 
{
	printf("%d\n", 0xE);	// Output: 14

	printf("%E\n", 0xE+3);	//Output: error invalid suffix "+3" on integer constant
	
	printf("%d\n", 0xE + 3);	// Output: 17
	
	return 0;
}
```
What is going on? 

0xE+3 is misinterpreted as scientific notation.

0xE + 3 is interpreted as it should be, output is 17. 

Compiler sees every number the same way, hexadecimal representation is only user-friendly feature. Hence 14 + 3;


