---
layout: post
title: Notes from C Traps and Pitfalls / C陷阱与缺陷笔记  
date: 2016-04-27 00:26
categories: [blog ]
tags: [C/C++, ]
description:
---


## Chapter 3: Semantic Pitfalls (语义陷阱)

Notes

+ the `&&` and `||` operators do not even evaluate their right-hand operands if their results can be determined from their left-hand operands. `&` and `|` unlike `&&`, must always evaluate both of its operands.

+ Overflow can occur if both operands are signed; the result of an overflow is undefined. To prevent overflow, try use `if ((unsigned) a + (unsigned) b > INT_MAX)` or `if (a > INT_MAX - b)` instead of `if (a + b < 0)`. P.S. ANSI C defines `INT_MAX` in `<limits.h>`.

+ The `main` is presumed to yield an int value if no other return type is declared for it. Typically, a 0 return indicates success and any other value indicates failure. If you forgot write a `return` in your main function, it usually implicitly returns some garbage integer.


Exercise 3-3. Write a function to do a binary search in a sorted table of integers. Its input is a pointer to the beginning of the table, a count of the elements in the table, and a value to be sought. Its output is a pointer to the element sought or a NULL pointer if the element is not present.

"pure pointer" form

    int *bsearch(int *t, int n, int x)
    {
      int *lo = t, *hi = t + n; // we cannot just initialize hi to t+n-1 because t+n-1 is an invalid address if n is 0
      while (lo < hi) {
        int *mid = lo + ((hi - lo) >> 1); // NOT mid = (lo + hi) / 2
        if(x < *mid)
          hi = mid; // NOT hi = mid -1
        else if (x > *mid)
          lo = mid + 1;
        else
          return mid;
      }
      return NULL;
    }

"array" form

    int *bsearch(int *t, int n, int x)
    {
      int lo = 0, hi = n - 1;
      while(lo <= hi)
      {
        int mid = (lo + hi) / 2;
        if(x < t[mid])
          hi = mid - 1;
        else if(x > t[mid])
          lo = mid + 1;
        else
          return t + mid;
      }
      return NULL;
    }



------Updated at 27th April, 2016------
