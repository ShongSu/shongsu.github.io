---
layout: post
title: Notes from Expert C Programming / C专家编程笔记
date: 2016-05-02 23:20
categories: [blog ]
tags: [C/C++, ]
description:
---


## Chapter 1: C Through the Mists of Time

### notes

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

### notes

+ Any time you encounter the string `malloc(strlen(str));` it is almost always sure to be an error, where `malloc(strlen(str)+1);` was meant. This is because almost all the other string handling routines include the room needed for the trailing `'\0'` terminator, so people get used to not making the special provision for it that strlen needs.

+ The one "l" `NUL` ends an ASCII string. The two "l" `NULL` points to no thing.

+ In `switch` statement, the `default` case (if present) can appear anywhere in the list of cases, and will be executed if none of the cases match.

+ All the cases are optional, and any form of statement, including labelled statements, is permitted, means that some errors can't be detected even by `lint`. A hard problem was mistyped `defau1t` for the `default` label (i.e., mistyped a digit "1" for the letter "l").

+ Some Symbol Overloading in C:

`static`:
Inside a function, retains its value between calls;

At the function level, visible only in this file.

`extern`:
Applied to a function definition, has global scope (and is redundant);

Applied to a variable, defined elsewhere.

`void`:
As the return type of a function, doesn't return a value;

In a pointer declaration, the type of a generic pointer;

In a parameter list, takes no parameters.


## Chapter 3: Unscrambling Declarations in C

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




### To be continued...
