---
tags:
  - c
  - types
  - volatile
  - static
  - scope
  - global
  - local
  - examageddon
  - stdint
  - stdbool
  - pending_review
---
A variable is a symbol that stores a value in a specific location in the memory of our system. A variable can be many types: an integer, a decimal, a boolean, a character, etc. The C programming language is a statically-typed language. What does this mean? It means that before you can execute the program (before compiling it), variables must have an assigned type. If not, the compilation will throw an error. And be careful because this is not Python or JavaScript: the type of the variable must be respected throughout the program, and it is not possible, for example, to store a text directly in a numeric variable without causing errors. Because of this, the C programming language is considered a strongly-typed language.

Variable types in C are grouped into: basic, enumerated, derived, and void types.

We also have pointers, which we will cover separately later.

# Basic types

Basic types correspond to numeric types. These include integers and decimals. Within these, there are multiple types depending on the size they occupy in memory (which will determine their range and resolution) and whether they are signed or not. Focusing on the size of variables, the size of a type depends on the system on which the code is running. You know that there are multiple systems that can run code: 8-bit systems, 16-bit systems, 32-bit systems, or 64-bit systems, like most desktop computers. Below is a table of the different types for integers and decimals for 32-bit ARM systems, which will be used in class.

## Integers

| Type                                             | Size (bytes) |                          Range                          |
| :----------------------------------------------- | :----------: | :-----------------------------------------------------: |
| `char`                                           |      1       |                        0 to 255                         |
| `unsigned char`                                  |      1       |                       -128 to 127                       |
| `int`                                            |      4       |             -2,147,483,648 to 2,147,483,647             |
| `unsigned int`                                   |      4       |                    0 to 4,294,967,29                    |
| `short`                                          |      2       |                    -32,768 to 32,767                    |
| `unsigned short`                                 |      2       |                       0 to 65,535                       |
| `long` or `long int`                             |      4       |             -2,147,483,648 to 2,147,483,647             |
| `unsigned long` or `unsigned long int`           |      4       |                   0 to 4,294,967,295                    |
| `long long` or `long long int`                   |      8       | -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 |
| `unsigned long long` or `unsigned long long int` |      8       |             0 to 18,446,744,073,709,551,615             |

## Floats

| Type                      | Size (bytes) |                    Range                    |
| :------------------------ | :----------: | :-----------------------------------------: |
| `float`                   |      4       |  1.2E-38 to 3.4E+38 with 6 decimal places   |
| `double` or `long double` |      8       | 2.3E-308 to 1.7E+308 with 15 decimal places |

# The void

The `void` type indicates that there is no value. For example, it is used in functions when they do not return anything and/or when they do not accept parameters. The `funcA` function in the following example does not return any value, while the `funcB` function returns an integer but does not accept any parameters:

```c title:"Example 1"
void funcA(unsigned int x){
	x = x + 1;
}

unsigned int funcB(void){
	return 1;
}

```

It is also used to declare pointers to undefined variables, but this is an advanced topic that we will leave aside.

# Enumerated types

Enums are a type of variable that only allow a series of integer values restricted by the user/programmer. Let's look at an example:

```c title:"Example 2"
enum color{
	COLOR_RED,
	COLOR_BLUE,
	COLOR_GREEN,
};

enum color background_color;

background_color = COLOR_RED;
```

In the example, we've defined an enum `color` that can take three values: `COLOR_RED`, `COLOR_BLUE`, and `COLOR_GREEN`. It can only take these values and no others. If you assign other values, the compiler will give an error. Once the enum is defined, you can declare variables of that enum type. The example specifies that it is an enum, then the type of enum, and finally the name of the variable. Assigning a value to this variable is done as usual, but using the values defined in the enum.

You might wonder where the integers I mentioned earlier are. Each value defined in the enum has an associated integer value. If an integer is not specified for an enum value, by default, they start at 0 and increment by order. The previous example would be equivalent to:

```c title:"Example 3"
enum color{
	COLOR_RED   = 0,
	COLOR_BLUE  = 1,
	COLOR_GREEN = 2,
};
```

And in fact, this definition is valid and allows us to define values different from the default ones:

```c title:"Example 4"
enum color{
	COLOR_RED   = -2,
	COLOR_BLUE  = 5,
	COLOR_GREEN = 4,
};
```

As you can see, enums can take negative values.

The values of enums are stored in the smallest integer type that can accommodate the enum values.

# Derived types

Derived types are those constructed from the types shown so far. These derived types are: arrays, structures, and unions.

## Arrays

Arrays are finite collections of a single type. These types can be numeric, enums, structures, unions, and pointers. They are declared by adding the size of the array (the number of elements it contains) in square brackets after the variable name. For example, to create an array of 5 `unsigned int` integers, we would write:

```c title:"Example 5"
unsigned int x[5];
```

If we want to access a value in the array, either to read it or to write it, we use square brackets to specify the element of interest:

```c title:"Example 6"
unsigned int x[5];

x[2] = 4;
```

The index of the array (the number used to identify an object within the array) starts at 0. Therefore, to access the first element from the previous example, we would use `x[0]`, while to access the last element, we would use `x[4]`.

> [!WARNING]
> We will cover this in more depth in [[Buffer overflow]], but the compiler will not complain if you read or write beyond the array's bounds. In the previous example, `x[87] = 76` is perfectly valid and the compiler will not raise an error, but you will be writing to a memory location outside the array and might affect the values of other variables stored in memory, causing [catastrophic effects](https://www.edn.com/toyotas-killer-firmware-bad-design-and-its-consequences/).

Array elements can be initialized at the same time the array is declared:

```c title:"Example 7"
unsigned int x[5] = { 12, 45, 2, 0, 3 };
```

And all the elements of an array can be initialized to 0:

```c title:"Example 8"
unsigned int x[5] = { 0 };
```

## Structures

Structures are variables that group together a set of sub-variables, which can be of different types. The following shows how to define a structure:

```c title:"Example 9"
struct person {
	unsigned int id;
	unsigned char age;
	float weight;
	float height;
}
```

In the example, a structure of type `person` is defined, which contains the variables `id`, `age`, `weight`, and `height`. To declare a variable of type `person` and access its members, we do it as follows:

```c title:"Example 10"
struct person andres;

andres.id     = 2087522;
andres.age    = 27;
andres.weight = 69.3;
andres.height = 171.5;
```

A structure can be initialized at the time of declaration. There are two ways to do this:

```c title:"Example 11"
struct person andres = {
	2087522,
	27,
	69.3,
	171.5,
};
```

By providing the values in the order in which the members of the structure were defined. You can also initialize using the names of the structure members preceded by a dot:

```c title:"Example 12"
struct person andres = {
	.id     = 2087522,
	.age    = 27,
	.weight = 69.3
	.height = 171.5,
};
```

As with arrays, all elements can also be initialized to 0:

```c title:"Example 13"
struct person andres = { 0 };
```

## Unions

The union follows a syntax similar to structures:

```c title:"Example 14"
union number {
	unsigned int x;
	long y;
	unsigned char z;
};
```

Accessing the members is done in the same way as with structures.

However, there is a critical difference between structures and unions. In structures, each member resides in its own memory space. However, the members of a union share the same memory space. The size of a structure is the result of the sum of the sizes of each of its members (ignoring possible memory alignments that the compiler might perform). The size of a union is equal to the size of the largest member.

![[Variables - Data types, declaration, definition, scope and attributes 2024-09-10 23.30.12.excalidraw|5000]]

# Typedef

The `typedef` attribute is used to create aliases for existing variable types. In the next section, we will see a clear example of using `typedef`, but for now, we can create a new variable type called `device_id` that corresponds to an `unsigned long int`:

```c title:"Example 15"
typedef unsigned long int device_id;

device_id oximeter_id;

oximeter_id = 198742;
```

In the example, we created a variable `oximeter_id` to store the identifier of an oximeter. The question now is, “why not use `unsigned long int oximeter_id` directly?” Well, there are two good reasons. The first is that it improves code readability. Seeing that a variable is of type `device_id` allows us to identify that this variable contains a device identifier. If we only saw `unsigned long int`, we wouldn’t know what type of information it contains. 

Second, and more importantly, imagine we have a program with thousands of lines (which is quite common) and we represent device identifiers as `unsigned long int`. What if the product requirements change and device identifiers need to be represented as an `int`? We would have to replace all the `unsigned long int` occurrences in the project (likely thousands) to accommodate the requirement change. However, if we use `typedef`, we only need to modify the variable type in the `typedef`, and our entire project will automatically comply with the new requirement. Practical, right?

# <stdint.h>

As we discussed with basic types, these types can have different sizes depending on the system on which the program is executed. To avoid this ambiguity, there is the standard library `stdint`. To use the `stdint` library, simply add the following at the top of your code file:

```c title:"Example 16"
#include <stdint.h>
```

Each platform implements the `stdint` library. If we took a look, we would see something like:

```c title:"Example 17"
typedef signed char		        int8_t;
typedef short int		        int16_t;
typedef int			            int32_t;
typedef long long int		    int64_t;
typedef unsigned char		    uint8_t;
typedef unsigned short int	    uint16_t;
typedef unsigned int		    uint32_t;
typedef unsigned long long int	uint64_t;
```

As we can see, the `stdint` library defines new types that explicitly state the sign and size of the variable. For example, the type `uint8_t` is an 8-bit unsigned integer (`u`), and the type `int32_t` is a 32-bit signed integer. Each platform declares these variables with the equivalent type according to the size.

It is [recommended](<Code style & good practices>) to use this library for declaring numeric variables.

## <stdbool.h>

At this point, you might be missing a variable type present in other programming languages: the boolean. This type of variable can take the value of true or false. However, you should know that initially, C did not have a boolean type. In C, an integer equal to 0 is considered false, while any non-zero integer is considered true. It wasn't until the C99 standard that the boolean type `_Bool` was introduced. A `_Bool` variable can only have the value 0 or 1. If a number other than 0 or 1 is assigned, it is directly converted to 1.

However, the standard library `stdbool` was also introduced. This library allows you to use `bool` instead of `_Bool` and provides the [macros](Macros) `true` and `false`, making the code more readable.

To use the `stdbool` library, simply add the following at the beginning of your code file:

```c title:"Example 18"
#include <stdbool.h>
```

And we can declare and operate with booleans following the example below:

```c title:"Example 19"
bool closed;

closed = true;
closed = false;
```

# Declaration and definition

The action of setting the type of a variable is called declaration. The instruction to declare a variable consists of the type of the variable and its name. For example, we can declare a variable named `x` with the type `unsigned int` (unsigned integer) with the following instruction:

```c title:"Example 20"
unsigned int x;
```

While assigning a value to a variable is its definition or assignment. For example, to assign the value 10 to the variable `x`, we do it with the following instruction:

```c title:"Example 21"
x = 10;
```

It is possible and often common to declare and initialize a variable in a single statement as follows:

```c title:"Example 22"
unsigned int x = 10;
```

Although it is a [not recommended practice](<Code style & good practices>), it is possible to declare multiple variables of the same type in a single statement:

```c title:"Example 23"
unsigned int x, y;
```

And also initialize them in the same statement:

```c title:"Example 24"
unsigned int x = 10, y = 20;
```

It is not possible to declare multiple variables of different types in a single statement.

# Scope

Variables can be of two types according to their scope: local and global.

## Local

Local variables are those declared within a block. A block is a portion of code enclosed in curly braces `{}`. Local variables can only be used within the block in which they are declared and in its sub-blocks. Consider the following example:

```c error:12 title:"Example 25"
unsigned int func1(void){
	unsigned int y = 0;
	unsigned int x = 10;

	for(unsigned short i = 0; i < x; i++){
		y = i;
	}
	return y;
}

unsigned int func2(void){
	return x;  // Error!
}
```

In the following example, the variables `x` and `y` can be used within the block that defines the function `func1` and within its sub-blocks, such as the `for-loop` block. However, if we use the variable `x` in the block of the function `func2`, the compiler will give an error because the variable `x` declared in a different scope cannot be accessed.

It is possible to declare a variable with the same name and type in sub-blocks. For example:

```c title:"Example 26"
unsigned int func1(void){
	unsigned int y = 0;
	unsigned int x = 10;

	for(unsigned short i = 0; i < x; i++){
		unsigned int y = 0;
		y = i;
	}
	return y;
}
```

In this case, the variable declared in the closest scope has "priority." In the previous example, the variable `y` modified within the `for-loop` is not the `y` variable created at the beginning of the `func1` function, but rather the `y` variable declared within the `for-loop`.

Another characteristic of this type of variable is that once the execution of the block in which they are declared is finished, they cease to exist and do not retain their value.

## Global

A global variable is a variable declared outside of any block. This type of variable is available to all blocks within the file of code in which it has been declared. For example:

```c title:"Example 27"
unsigned int x = 5;

unsigned int func1(void){
	unsigned int y = 0;
	unsigned int x = 10;

	for(unsigned short i = 0; i < x; i++){
		y = i;
	}
	return y;
}

unsigned int func2(void){
	return x;
}
```

Now the variable `x` can be used in both `func1` and `func2`. However, it's worth noting that, just like with local variables, if a variable with the same name and type is declared in a sub-block, that sub-block will use the variable declared in the nearest scope. In the case of `func1`, the local variable is used, not the global one.

Unlike local variables, global variables maintain their value throughout the entire execution of the program.

Finally, by default, global variables are external. This means they can be used in other code files. However, depending on the compiler's configuration, you might receive a warning indicating that an external variable is being used implicitly. It is a warning, not an error, and does not usually cause problems in extremely simple applications. However, it is [recommended](<Code style & good practices>) to always make explicit declarations of global variables used in other code files. This is done using the `extern` attribute. Suppose we have a file `file1.c`:

```c title:"Example 28"
unsigned int x = 5;

unsigned int func1(void){
	unsigned int y = 0;
	unsigned int x = 10;

	for(unsigned short i = 0; i < x; i++){
		y = i;
	}
	return y;
}

unsigned int func2(void){
	return x;
}
```

And suppose we have a file `file2.c` where we want to use the global variable declared in `file1.c`. Simply, in `file2.c`, we would do:

```c title:"Example 29"
extern unsigned int x;

unsigned int func3(void){
	return x;
}
```

## Special attributes

Aside from the scope relevant to each variable, it is common to encounter two other attributes: `static` and `volatile`.

### static

We make a variable `static` when we want to limit a global variable to the file in which it has been declared, making it inaccessible from another source file even when using the `extern` attribute. We also use the `static` attribute for local variables that we do not want to lose their value between executions. That is, as we have seen, a local variable is destroyed and loses its value once the block in which it is declared has finished executing. By adding the `static` attribute, the local variable will not be destroyed and will retain its value between executions.

The `static` attribute is placed right before the type of the variable:

```c title:"Example 30"
static unsigned int x;
```

If we declare a local variable as static and also initialize it in the same statement, the value assignment will occur only during the first execution. For example:

```c title:"Example 31"
unsigned int func4(void){
	static unsigned int x = 20;
	x = x + 1;
	return x;
}
```

In this example, the first time we call the function `func4`, it will return 21. The second time we call it, it will return 22, the next time 23, and so on. However, in the following example:

```c title:"Example 32"
unsigned int func4(void){
	static unsigned int x;
	x = 20;
	x = x + 1;
	return x;
}
```

The function `func4` will always return 21.

### volatile

The `volatile` attribute is sometimes difficult to understand because it requires a deeper knowledge of how processors operate at a low level, but here we'll provide a basic explanation to help us understand what the `volatile` attribute does.

A processor, when performing operations with a variable, does not directly operate with values in RAM. Instead, it first copies the value to a register. A register is an internal memory of the processor used to hold a single value, and it is directly connected to the [ALU (Arithmetic Logic Unit)](https://en.wikipedia.org/wiki/Arithmetic_logic_unit) of the processor, allowing for high-speed operations that would be slower if done directly in RAM. While this provides the advantage of computational speed, it also has a disadvantage: what happens if a function modifying a variable `x` is executed, and at the same time, the processor receives an [interrupt](Theory/Interrupts) and executes another function that modifies the same variable `x`? Imagine both functions are incrementing the value of `x`.

```c title:"Example 33"
unsigned int x = 0;

void func5(void){
	x = x + 1;
}

void func6(void){
	x = x + 1;
}
```

When the function `func5` is executing, the processor will copy the value of the variable `x` from RAM to register R1 and increment it there from 0 to 1. If, before this value is copied back from register R1 to RAM, function `func6` is executed, the processor will copy the value of the variable `x` from RAM to another register R2, increment it from 0 to 1, and store it back in RAM. The processor will then resume executing `func5`, copying the value 1 from register R1 back to RAM. The result? We intended to increment the variable `x` twice, but it only increments from 0 to 1 instead of 0 to 2. This is known as a "race condition." The solution? Avoid operating on registers and perform the operations directly in RAM. This way, all functions operate on the same variable. Consequences? Slower read and write operations. Is it critical? In some systems, yes, such as in a car's airbag system.

In summary, the `volatile` attribute should be used for non-critical variables that need to be modified from different points in your program.