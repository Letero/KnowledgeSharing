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


## 2. Variables
Variables

Each variable in C must have:
 *  Name
 *	Type
 *	Value
 
 Each variable in C must have a value. 
 * Static variables file scope and function scope are automatically initialized to zero.
 * Non-static global variables are initialized to zero.
 * Non-static local variables contains indetermine value.
 
 c99 standard says:
 3.17.2
 1 indeterminate value
 either an unspecified value or a trap representation
 

```c
static int s_global;      // static file scope will be initialized to 0
int global;               // global non-static will be indetermine
 
void foo()
{
	static int s_local;    // static local will be initialized to 0
	int local;	 	      // non-static local will be indetermine
	
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


