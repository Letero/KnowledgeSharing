- [Preparation](#preparation)
- [C](#c)
  * [1. Difference between initial values of unitialized static and non-static variables](#1-difference-between-initial-values-of-unitialized-static-and-non-static-variables)
  * [2. Does *const volatile* make any sense? IQN](#2-does--const-volatile--make-any-sense--iqn)
  * [3. Restrict keyword - what is it used for?](#3-restrict-keyword---what-is-it-used-for-)
  * [4. Void* pointer in C! IQN](#4-void--pointer-in-c--iqn)
- [CPP](#cpp)
  * [1. Copy elision](#1-copy-elision)
  * [2. Virtual Destructor](#2-virtual-destructor)
  * [3. When will copy constructor be called? IQN](#3-when-will-copy-constructor-be-called--iqn)
  * [4. When will deconstructor be called? IQN](#4-when-will-deconstructor-be-called--iqn)
  * [5. Differences: free vs delete   IQN](#5-differences--free-vs-delete---iqn)
  * [6. Differences: malloc vs new    IQN](#6-differences--malloc-vs-new----iqn)
- [Base knowledge](#base-knowledge)
  * [1. Memory layout of C/CPP programs /IQN](#1-memory-layout-of-c-cpp-programs--iqn)
  * [2. Compilation process C/CPP](#2-compilation-process-c-cpp)
  * [3. Preprocessor directives IQN](#3-preprocessor-directives-iqn)
  * [4. Translation unit  IQN](#4-translation-unit--iqn)

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

## 3. Restrict keyword - what is it used for?

Restrict keyword is used in pointer declaration. It tells that this pointer is the only way to access object that it's pointing to.
It does not add new functionality
Compiler won't prevent programmer from creating another pointer to restricted address. It is promise made by programmer.
Not following to restrict contract results in undefined behavior.
Restrict keyword is used so compiler can make optimizations.
It was defined in C99 standard

## 4. Void* pointer in C! IQN

TODO!
[Link for now](https://bytes.com/topic/c/answers/872557-what-use-void-pointer)

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


## 3. When will copy constructor be called? IQN

TODO!
[Link for now](https://stackoverflow.com/questions/21206359/in-which-situations-is-the-c-copy-constructor-called)

## 4. When will deconstructor be called? IQN

TODO!
[Link for now](https://stackoverflow.com/questions/10081429/when-is-a-c-destructor-called)


## 5. Differences: free vs delete	IQN 

TODO!
[Link for now](https://stackoverflow.com/questions/328834/c-delete-vs-free-and-performance)


## 6. Differences: malloc vs new	IQN

TODO!
[Link for now](https://www.geeksforgeeks.org/malloc-vs-new/)

# Base knowledge

## 1. Memory layout of C/CPP programs /IQN

For now use this source, decent explanation
[Link](https://www.geeksforgeeks.org/memory-layout-of-c-program/)

I will elaborate on this once I'm done with binary operations.

## 2. Compilation process C/CPP

TODO!
[Link for now](https://www.youtube.com/watch?v=wDKeJ79TBsg)

## 3. Preprocessor directives IQN

TODO!
[Link for now](http://www.cplusplus.com/doc/tutorial/preprocessor/)

## 4. Translation unit	IQN

TODO!
[Link for now](https://stackoverflow.com/questions/8342185/translation-unit-in-c-and-c)

