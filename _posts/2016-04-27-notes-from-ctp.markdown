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
