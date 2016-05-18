---
layout: post
title: Notes from JavaScript:The Definitive Guide / JavaScript权威指南笔记
date: 2016-05-04 17:32
categories: [blog ]
tags: [JavaScript, ]
description:
---


## Part I. Core JavaScript

### Ch 2. Lexical Structure

+ The `return`, `break`, and `continue` statements often stand alone, but they are sometimes followed by an identifier or expression. If a line break appears after any of these words (before any other tokens), JavaScript will always interpret that line break as a semicolon. 

For example, if you write:

			return 
					true;

JavaScript assumes you meant:

			return; true;


### Ch 3. Types, Values, and Variables

+ Objects and arrays are mutable. Numbers, booleans, null, and undefined are immutable, strings are immutable too.

+ Arithmetic in JavaScript does not raise errors in cases of overflow, underflow, or divi- sion by zero. Print `Infinity` and `-Infinity` instead.

+ Division by zero is not an error in JavaScript: it simply returns infinity or negative infinity. There is one exception, however: zero divided by zero does not have a well- defined value, and the result of this operation is the special not-a-number value, printed as NaN. NaN also arises if you attempt to divide infinity by infinity, or take the square root of a negative number or use arithmetic operators with non-numeric operands that cannot be converted to numbers.

+ The not-a-number value has one unusual feature in JavaScript: it does not compare equal to any other value, including itself. This means that you can’t write `x == NaN` to determine whether the value of a variable `x` is `NaN`. Instead, you should write `x != x`. That expression will be true if, and only if, `x` is `NaN`.

+ The negative zero value is also somewhat unusual.
        
        
        var zero = 0;  // Regular zero
        var negz = -0;  // Negative zero
        zero === negz    // => true: zero and negative zero are equal
        1/zero === 1/negz  // => false: infinity and -infinity are not equal


+ Because of rounding error,the difference between the approximations of `.3` and `.2` is not exactly the same as the difference between the approximations of `.2` and `.1`.


        var x = .3 - .2;  // thirty cents minus 20 cents
        var y = .2 - .1;  // twenty cents minus 10 cents
        x == y            // => false: the two values are not the same!

In JS, `0.3-0.2=0.09999999999999998;`


+ Remember that strings are immutable in JavaScript. Methods like `replace()` and `toUpperCase()` return new strings: they do not modify the string on which they are invoked.

+ Diff between `null` and `undefined`:


        typeof(null)  // => object
        typeof(undefined) // => undefined
        null == undefined  // =>true
        null === undefined  // =>false
 
### Ch 4. Expressions and Operators



## To be Continued ...






