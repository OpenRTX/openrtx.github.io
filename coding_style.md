# Coding style guidelines

This page provides a brief summary of the coding style used in the OpenRTX repository and is meant to provide a reference to the contributors.

## General guidelines
* Code width must not exceed the limit of 80 columns.
* Variables' must be explicative of their role and written in camel case (i.e. `aVarInCamelCase`) when necessary.
* Integer variables must be declared using the following data types, clearly specifying their width: `uint64_t, uint32_t, uint16_t, uint8_t, int64_t, int32_t, int16_t, int8_t`. Use of `char` and `unsigned char` for signed and unsigned 8 bit variables is not allowed. These datatypes are defined in `stdint.h` and `cstdint` C and C++ headers, respectively.
* When multiple conditions connected by logical operators are inside `if` and `while` body, each condition must be surrounded by round brackets
```
if((a == 0) && (b == 1))
{
    [...]
```
* A space must be put between angle brackets and template argument in C++ code
```
std::vector< int > aVector;
```
* When a line of code contains more than one mathematical operation between one or more variables, the order of operations must be clearly indicated by using round brackets.
```
int something = (a * b) + c;
```

## Code spacing and inlining
* Only spaces are allowed for code indentation and the indentation with amounts to four spaces.
* When multiple variables are defined and/or initialised at subsequent lines, their names and the assignment operators must be aligned.
```
int  aVar      = 0;
char secondVar = 'c';
```
* Spaces must be put between variables and mathematical, bitwise, logical and assignment operators.
```
int c = a + b;
if((a == 0) || (c != 0))
```
* Is preferable to break condition statements inside `if` and `while` body exceeding the 80 columns limit over multiple lines, dedicating each line to one condition
```
if((foo == 0) &&
   (goo != 0) &&
   (zoo == 3))
```

## Brackets
* The Allmann code style must be used. The space between the end of the function name or operator and the open round bracket is optional.
```
void foo (int d)
{
    if ((d + 3) != 0)
    {
        bar();
    }
}
```
or, alternatively
```
void foo(int d)
{
    if((d + 3) != 0)
    {
        bar();
    }
}
```

## Comments and documentation
* All the function prototypes defined in header files must be documented using the following Doxygen syntax
```
/**
 * Description...
 *
 * \param ptr: ...
 * \return ...
 */
 int func(void *ptr);
```
