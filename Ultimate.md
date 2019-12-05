- [Preparation](#preparation)
- [C](#c)
  * [1. Difference between initial values of unitialized static and non-static variables](#1-difference-between-initial-values-of-unitialized-static-and-non-static-variables)
  * [2. IQN Does *const volatile* make any sense?](#2-iqn-does--const-volatile--make-any-sense-)
  * [3. IQN Void* pointer in C](#3-iqn-void--pointer-in-c)
  * [4. IQN Register keyword](#4-iqn-register-keyword)
  * [5. Auto in C?](#5-auto-in-c-)
  * [6. Token](#6-token)
  * [7. When the 'address of' operator (&) cannot be used?](#7-when-the--address-of--operator-----cannot-be-used-)
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
  * [12. Serialisation and deserialisation](#12-serialisation-and-deserialisation)
- [Interesting facts](#interesting-facts)
  * [1. Keyword "import"](#1-keyword--import-)
  * [2. Odd array indexing](#2-odd-array-indexing)
  * [3. Token whitespace](#3-token-whitespace)
  * [4. Comma operator](#4-comma-operator)

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

## 6. Token

A C program consists of various tokens and a token is either a keyword, an identifier, a constant, a string literal, or a symbol.

Even space can be token - checkout [3. Token whitespace](#3-token-whitespace) to see interesting example.

## 7. When the 'address of' operator (&) cannot be used?

It cannot be used on variable which are declared using register storage class.

```c
#include <stdio.h>

int main(void) 
{
  register int someValue = 6;
  printf("%p", &someValue);
  return 0;
}
```
Ouput:
```c
prog.c: In function ‘main’:
prog.c:9:2: error: address of register variable ‘someValue’ requested
  printf("%p", &someValue);
```

It is doable in C++ tho.


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

Difference is purely conceptual. Those two are different names for same area in memory.

*malloc*, *calloc*... and *free* use **heap**.

*new* and *delete* use **stack**. 

 It *could* be compiler specific and those two *could* designate a different memory spaces, but I don't think that you will ever see something like that. 


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

## 12. Serialisation and deserialisation

**Serialisation** (or serialization) is the process of translating data structures or object state into a format that can be stored (for example, in a file or memory buffer) or transmitted (for example, across a network connection link) and reconstructed later (possibly in a different computer environment). When the resulting series of bits is reread according to the serialization format, it can be used to create a semantically identical clone of the original object

**Deserialization** is the opposite operation, extracting a data structure from a series of bytes

![Example](https://github.com/Letero/KnowledgeSharing/blob/master/Images/serialisation.jpeg)

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

It is one of very few examples when whitespace becomes token 

## 4. Comma operator

This is useful, but not widely used operator. It can also be tricky.

Few examples:
```c
int j = 6, 6, 6;
printf("%d", j);	//prints 6
```
- *Why?*
Each statement is evaluated, but the value of the expression will be that of the last statement evaluated.

Similar, a little bit more complicated example:
```c
int j = (6,6,6) + (4,4,4) + (3,3,3,3,3);
printf("%d", j); // 6+4+3 = 13. Each bracket statement is 
```

Let's go further.
```c
#include <stdio.h>

int main(void) 
{
	int j = (printf("Assigning variable j\n"), 555);
	printf("%d\n", j);
	return 0;
}
```
Output:
```c
Success	#stdin #stdout 0s 9424KB
Assigning variable j
555
```

You can use this operator pretty much everywhere.
```c
for (int i = 0; i < 101; ++i, functionCall())	// iterate and call function
{
	// do something else
}
```

## 5. Printf() and %n 

Example:
```c
int a = 0;
printf("Something%n", &a);	
printf("\n %d", a);
```
Output:
```c
Something
 9
```
%n counts all characters preceding it and saves this value under given address (&a in our case). Below useful use case.

Input:
```c
Welcome!
```
```c
int main(void) 
{
	char str[100];
	int length;
	scanf("%s%n", &str, &length);
	printf("Length = %d", length);
	return 0;
}
```
Output:
```c
Length = 8
```
