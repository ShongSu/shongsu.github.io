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


### To be continued...
