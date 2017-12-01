---
layout: post
title: Introduction to Python / Python入门
date: 2017-11-30 15:36
categories: [blog ]
tags: [Python]
description:
---

This post is my study note when learning Python programming based on [online tutorial][py] provided by University of Waterloo.

## 0 Hello World

> print("Hello, World!")

## 1 Variables & Errors

You don't need keywords like `var` or `int` to decare variables.

The following code will results to a `TypeError: Can't convert 'int' object to str implicitly`.

> print("you cannot add text and numbers" + 12)

Q: What is the technical difference between syntax and run-time errors?

A: The program with the run-time error created some output, but the one with the syntax error did not. This is because Python runs in two steps:
1. Python checks if your program has correct syntax, in order to determine its structure and parts.
2. If no syntax errors were encountered in step 1, then the program is executed.
So, a program with a syntax error will execute no steps at all, but a program with a run-time error will execute the steps that happened before the error occured.

The extra spaces in most situations had no effect on the output. However, be careful that extra space at the beginning of a line, called indenting, has a special meaning that can cause errors if used incorrectly.

## 2 Functions

Q: Code scramble: make the program sort the three numbers x, y and z into increasing order, so that x has the smallest value, y has the next smallest value, and z has the largest value. (only use max(a,b) and min(a,b)).

A:
  tmp = max(x,y)
  x = min(x,y)
  y = tmp
  tmp = max(y,z)
  y=min(y,z)
  z = tmp
  tmp = max(x,y)
  x = min(x,y)
  y = tmp

## 3 Comments and Quotes

Any line of instructions containing the # symbol

> print(1) # here is a comment
> # print(2)
> print(3) # another comment @!#!@$

If a pound sign # appears in a string, then it does not get treated as a comment.

Use character " inside of a string:

> print('I said "Wow!" to him.')
> print("This is an \"escape\" of a double-quote")

## 4 Types









To be continued...




[py]: https://cscircles.cemc.uwaterloo.ca/
