- [Preparation](#preparation)
- [C](#c)
  * [1. Example question](#1-example-question)
  * [2. Variables](#1-variables)
- [CPP](#cpp)
  * [1. Example question](#1-example-question-1)
- [Base knowledge](#base-knowledge)


# Preparation

**[How to format this file](https://guides.github.com/features/mastering-markdown)**

**[Awesome README](https://github.com/matiassingers/awesome-readme)**

**[Generate Table of Content](https://ecotrust-canada.github.io/markdown-toc)**

# C

## 1. Example question

Example answer

[Link to example code](https://github.com/Letero/KnowledgeSharing/blob/master/Examples/C_code/main.c)


## 2. Difference between initial values of unitialized static and non-static variables

Each variable in C must have:
 *  Name
 *	Type
 *	Value
 
 I repeat: each variable in C must have a value!
 * **Static variables** - both global (file) scope and local (eg. function) scope are automatically initialized with 0.
 * **Non-static global** variables are initialized with 0.
 * **Non-static local** variables are initialized with random value from stack.
 

```c
static int s_global;      // global static variables are initalized with 0
int global;               // global non-static variables** are initialized with 0
 
void foo()
{
	static int s_local;   // static local variables are initialized with 0, just like global variables
	int local;            // non-static local variables will be indetermine, probably 
	
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

# CPP

## 1. Example question
Example answer

[Link to example code](https://github.com/Letero/KnowledgeSharing/blob/master/Examples/CPP_code/main.cpp)

# Base knowledge


