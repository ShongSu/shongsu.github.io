---
layout: post
title: Notes from Expert C Programming / C专家编程笔记
date: 2016-05-02 23:20
categories: [blog ]
tags: [C/C++, ]
description:
---


## Chapter 1: C Through the Mists of Time

### Notes

+ Avoid unnecessary complexity by minimizing your use of unsigned types; use a signed type like `int` and you won't have to worry about boundary cases in the detailed rules for promoting mixed types; only use unsigned types for bitfields or binary masks. Use casts in expressions, to make all the operands `signed` or `unsigned`, so the compiler does not have to choose the result type.

for example:


      int array[] = { 23, 34, 12, 17, 204, 99, 16 };
      #define TOTAL_ELEMENTS (sizeof(array) / sizeof(array[0]))
      int main(){
        int d= -1;
        if (d <= TOTAL_ELEMENTS-2)
        {
          /* ... */
        }
      }

The defined variable `TOTAL_ELEMENTS` has type `unsigned int` (because the return type of sizeof is "unsigned"). The test is comparing a signed `int` with an `unsigned int` quantity. So `d` is promoted to `unsigned int`. Interpreting -1 as an `unsigned int` yields a big positive number, making the clause `false`.


## Chapter 2: It's Not a Bug, It's a Language Feature

### Notes

+ Any time you encounter the string `malloc(strlen(str));` it is almost always sure to be an error, where `malloc(strlen(str)+1);` was meant. This is because almost all the other string handling routines include the room needed for the trailing `'\0'` terminator, so people get used to not making the special provision for it that strlen needs.

+ The one "l" `NUL` ends an ASCII string. The two "l" `NULL` points to no thing.

+ In `switch` statement, the `default` case (if present) can appear anywhere in the list of cases, and will be executed if none of the cases match.

+ All the cases are optional, and any form of statement, including labelled statements, is permitted, means that some errors can't be detected even by `lint`. A hard problem was mistyped `defau1t` for the `default` label (i.e., mistyped a digit "1" for the letter "l").

+ Some Symbol Overloading in C:

`static`:

Inside a function, retains its value between calls; At the function level, visible only in this file.

`extern`:

Applied to a function definition, has global scope (and is redundant); Applied to a variable, defined elsewhere.

`void`:

As the return type of a function, doesn't return a value; In a pointer declaration, the type of a generic pointer; In a parameter list, takes no parameters.



## Chapter 3: Unscrambling Declarations in C

### Notes

+ A declaration involving a pointer and a const has several possible orderings:


        const int * grape;
        int const * grape;
        int * const grape_jelly;

The last of these cases makes the pointer read-only, whereas the other two make the object that it points at read-only; and of course, both the object and what it points at might be constant. Either of the following equivalent declarations will accomplish this:


        const int * const grape_jam;
        int const * const grape_jam;


+ a function returning a pointer to a function is allowed: `int (* fun())()`

  a function returning a pointer to an array is allowed: `int (* foo())[]`

  an array holding pointers to functions is allowed: `int (*foo[])()`

  an array can hold other arrays, so you'll frequently see `int foo[][]`


+ The Precedence Rule

<table style="width:100%">
  <tr>
    <th colspan="3">The Precedence Rule for Understanding C Declarations</th>
  </tr>
  <tr>
    <th colspan="2">A</th>
    <th>Declarations are read by starting with the name and then reading in precedence order.</th>
  </tr>
  <tr>
    <th colspan="2">B</th>
    <th>The precedence, from high to low, is:</th>
  </tr>
  <tr>
    <td></td>
    <td>B.1</td>
    <td>parentheses grouping together parts of a declaration</td>
  </tr>
  <tr>
    <td></td>
    <td>B.2</td>
    <td>the postfix operators:</br>parentheses () indicating a function, and</br> square brackets [] indicating an array.</td>
  </tr>
  <tr>
    <td></td>
    <td>B.3</td>
    <td>the prefix operator: the asterisk denoting "pointer to".</td>
  </tr>
  <tr>
    <th colspan="2">C</th>
    <th>If a const and/or volatile keyword is next to a type specifier (e.g. int, long, etc.) it applies to the type specifier. Otherwise the const and/or volatile keyword applies to the pointer asterisk on its immediate left.</th>
  </tr>
</table>

+ A Example Solving a Declaration Using the Precedence Rule

<table style="width:100%">
  <tr>
    <th colspan="3">char* const *(*next)();</th>
  </tr>
  <tr>
    <th colspan="2">A</th>
    <th>First, go to the variable name, "next", and note that it is directly enclosed by parentheses.</th>
  </tr>
  <tr>
    <td></td>
    <td>B.1</td>
    <td>So we group it with what else is in the parentheses, to get "next is a pointer to...".</td>
  </tr>
  <tr>
    <th colspan="2">B</th>
    <th>Then we go outside the parentheses, and have a choice of a prefix asterisk, or a postfix pair of parentheses.</th>
  </tr>
  <tr>
    <td></td>
    <td>B.2</td>
    <td>Rule B.2 tells us the highest precedence thing is the function parentheses at the right, so we have "next is a pointer to a function returning…"</td>
  </tr>
  <tr>
    <td></td>
    <td>B.3</td>
    <td>Then process the prefix "*" to get "pointer to".</td>
  </tr>
  <tr>
    <th colspan="2">C</th>
    <th>Finally, take the "char * const", as a constant pointer to a character</th>
  </tr>
</table>

Then put it all together to read:

"`next` is a pointer to a function returning a pointer to a const pointer-to-char"

+ Another Example: `char *(*c[10])(int **p);`

"`c` is an array[0..9] of pointer to a function returning a pointer-to-char"


## Chapter 4: The Shocking Truth: C Arrays and Pointers Are NOT the Same!

### Notes

+ Defined as array NOT Same AS external declaration as pointer. The pointer variable itself will always be at the same address, but its contents can change to point to many different ints at different times. Each of those different ints can have different values. The array can't move around to different places. At different times it can be filled with different values, but it always refers to the same n consecutive memory locations.

+ Differences Between Arrays and Pointers

<table style="width:100%">
   <tr>
    <th>Pointer</th>
    <th>Array</th>
  </tr>
  <tr>
   <td>Holds the address of data</td>
   <td>Holds data</td>
  </tr>
  <tr>
    <td>Data is accessed indirectly, so you first retrieve the
contents of the pointer, load that as an address (call it "L"),
then retrieve its contents.
If the pointer has a subscript [i] you instead retrieve the
contents of the location 'i' units past "L"</td>
    <td>Data is accessed directly, so for a[i] you
simply retrieve the contents of the
location i units past a.</td>
  </tr>
  <tr>
   <td>Commonly used for dynamic data structures</td>
   <td>Commonly used for holding a fixed number of elements of the same type of data</td>
  </tr>
  <tr>
   <td>Commonly used with `malloc()`, `free()`</td>
   <td>Implicitly allocated and deallocated</td>
  </tr>
  <tr>
   <td>Typically points to anonymous data</td>
   <td>Is a named variable in its own right</td>
  </tr>
</table>


+ A string literal created by a pointer initialization is defined as read-only in ANSI C; In contrast to a pointer, an array initialized by a literal string is writable. The individual characters can later be changed.




### To be continued...
