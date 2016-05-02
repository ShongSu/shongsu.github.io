---
layout: post
title: Notes from C Traps and Pitfalls / C陷阱与缺陷笔记  
date: 2016-04-27 00:26
categories: [blog ]
tags: [C/C++, ]
description:
---

## Chapter 1: Lexical Pitfalls (词法陷阱)

### Notes

+ A character enclosed in single quotes is just another way of writing the integer that corresponds to the given character in the implementation's collating sequence. A string enclosed in double quotes is a short-hand way of writing a pointer to the initial character of a nameless array that has been initialized with the characters between the quotes and an extra character whose binary value is zero (`'\0'`).

### Exercise

1-1. Some C compilers allow nested comments. Write a C program that finds out if it is being run on such a compiler without any error messages. In other words, the program should be valid under both comment rules, but should do different things in each. Hint. A comment symbol `/*` inside a quoted string is just part of the string; a double quote `" "` inside a comment is part of the comment.

      /*/**/"*/"/*"/**/

This expression is equivalent to `"*/"` if comments nest and `"/*"` if they don't.

      /*/*/0*/**/1

This expression is equivalent to `1` if comments nest and `0*1` if they don't.


## Chapter 2: Syntactic Pitfalls (语法陷阱)

### Notes

+  Wrong examples of operator precedence:


        if (flags & FLAG != 0)  ===  if (flags & (FLAG != 0))
        r = hi<<4 + low  === r = hi<< (4 + low)
        r = (hi<<4) + low === r = hi<<4 | low
        *p++ === *(p++)
        while(c=getc(in) != EOF) === while(c = (getc(in) != EOF))

+ Do not forget `break` in each `case` of `switch` statement.

+ C requires a function call to have an argument list even if there are no arguments. `f();` is a statement that calls the function, but `f;`does nothing at all. More precisely, it evaluates the address of the function
but does not call it.

+ An `else` is always associated with the closest unmatched `if` inside the same pair of braces.



## Chapter 3: Semantic Pitfalls (语义陷阱)

### Notes

+ Apply `sizeof()` for Arrays and Pointers:


      int calendar[12][31];  /* sizeof(calendar) = 31 * 12 * sizeof(int) */
      calendar[4];   /* sizeof(calendar[4]) = sizeof(*monthp) = 31 * sizeof(int) */
      int *p=calendar[4];   /* sizeof(p) = 2 * sizeof(int); sizeof(*p) = 1 * sizeof(int) */
      int (*monthp)[31]=calendar;  /* sizeof(monthp) = 2 * sizeof(int) */

In addtion, `calendar[4][7] == *(calendar[4] + 7) == *((calendar + 4) + 7)`.

+ Copying a pointer does not copy the thing it addresses.

+ Using asymmetric bounds, that is, when traverse a array `int a[10]`, using `for(i = 0; i < 10; i++ )` rather than `for(i = 0; i <= 9; i++ )`.

+ Pointer declaration syntax overview:


      char cff [5][5];    /* array of arrays of chars; a char can be any sign */
      char *cfp [5];      /* array of pointers to chars */
      char **cpp;         /* pointer to pointer to chars ("double pointer") */
      char (*cpf) [5];    /* pointer to an array of chars */
      char *cpF();        /* function which returns a pointer to chars */
      char (*CFp)();      /* pointer to a function which returns chars */
      char (*cfpF*())[5]; /* function which returns pointers to an array of chars */
      char (*cpFf[5])();  /* an array of pointers to functions, which all return chars */

The `()` and `[]` have a higher priority than `*`.

+ the `&&` and `||` operators do not even evaluate their right-hand operands if their results can be determined from their left-hand operands. `&` and `|` unlike `&&`, must always evaluate both of its operands.

+ Overflow can occur if both operands are signed; the result of an overflow is undefined. To prevent overflow, try use `if ((unsigned) a + (unsigned) b > INT_MAX)` or `if (a > INT_MAX - b)` instead of `if (a + b < 0)`. P.S. ANSI C defines `INT_MAX` in `<limits.h>`.

+ The `main` is presumed to yield an int value if no other return type is declared for it. Typically, a 0 return indicates success and any other value indicates failure. If you forgot write a `return` in your main function, it usually implicitly returns some garbage integer.


### Exercise

3-3. Write a function to do a binary search in a sorted table of integers. Its input is a pointer to the beginning of the table, a count of the elements in the table, and a value to be sought. Its output is a pointer to the element sought or a NULL pointer if the element is not present.

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


## Chapter 4: Linkage (链接)

### Notes

+ The declaration `extern int a;` is not a definition of `a`, such a declaration is a reference to the external object `a` but does not define it.

+ A program that includes `extern int a;` must say `int a;` somewhere else, either in the same program file or a different one.

+ The declaration `static int a;` means the same thing as `int a;` within a single source program file, but a is hidden from other files.

+ Arguments(形参), parameters(实参)。

+ A function called before it is defined or declared is assumed to return `int`.

+ A program with this definition: `char filename[] = "/etc/passwd";` in one file and this declaration: `extern char *filename;` in another. Although arrays and pointers are very similar in some contexts, they are not the same.


## Chapter 5: Library functions (库函数)

### Notes

+ use `setbuf` in a program that copies its standard input to its standard output:

        #include <stdio.h>
        main()
        {
            int c;
            char buf[BUFSIZ];  // error in this line.
            setbuf(stdout, buf);

            while (c = getchar()) != EOF)
            putchar (c);
        }

There are two ways to prevent this sort of trouble: First, make the `buffer` static, either by declaring it explicitly as static:

    static char buf[BUFSIZ];

or by moving the declaration outside the main program entirely.

Another possibility is to allocate the buffer dynamically and never free it:

    char *malloc();
    setbuf(stdout, malloc(BUFSIZ);

+ `stdarg.h` is a header in the C standard library hat allows functions to accept an indefinite number of arguments. Example 1:

        #include <stdio.h>
        #include <stdarg.h>

        /* print all args one at a time until a negative argument is seen; all args are assumed to be of int type */
        void printargs(int arg1, ...)
        {
          va_list ap;
          int i;

          va_start(ap, arg1);
          for (i = arg1; i >= 0; i = va_arg(ap, int))
          printf("%d ", i);
          va_end(ap);
          putchar('\n');
        }

        int main(void)
        {
          printargs(5, 2, 14, 84, 97, 15, -1, 48, -1);
          printargs(84, 51, -1);
          printargs(-1);
          printargs(1, -1);
          return 0;
        }

This program yields the output:

    5 2 14 84 97 15
    84 51

    1


Example 2:


      #include <stdarg.h>

      int printf(char *format, ...)
      {
        va_list ap;
        int n;

        va_start(ap,format);
        n = vprintf(format, ap);
        va_end(ap);
        return n;
      }



## Chapter 6: Preprocessor (预处理器)

### Notes

+ It is important to to enclose each parameter in parentheses and parenthesize the entire result expression to defend against using the macro in a larger expression.

+ Some examples:


      #define abs(x) (((x)>=0) ? (x) : (-x) )
      #define max(a,b) ((a)>(b) ? (a):(b))
      #define toupper(c) ((c)=>'a' && (c)<='z' ? (c + ('A'-'a')) : (c))
      #define assert(e) ((e) || assert_error(__FILE__, __LINE__))

+ Macros are not type definitions. Using `typedef` instead of `#define`. For example, if you defined `#define T1 struct foo *` and `typedef struct foo *T2`. These definitions make T1 and T2 conceptually equivalent to a pointer to
a struct foo. But when we try to use them with more than one variable:


      T1 a, b;
      T2 c, d;

The first declaration gets expanded to `struct foo * a, b;` This defines `a` to be a pointer to a structure, but defines `b` to be a structure (not a pointer). The second declaration, in contrast, defines both `c`
and `d` as pointers to structures, because `T2` behaves as a true type.

### Exercise

6-1. Write a macro version of max with integer arguments that evaluates its arguments only once.

    static int max_temp1, max_temp2;
    #define max(p,q) (max_temp1=(p),max_temp2=(q),\
            max_temp1>max_temp2? max_temp1:max_temp2)

This will work as long as calls to max are not nested; making it work in that case may be impossible.

## Chapter 7: Portability pitfalls (可移植性缺陷)

### Notes

+ The three sizes of integers are nondecreasing. That is, a `short` integer can contain only values that will also fit in a `int` integer and a `int` integer can contain only values that will also fit in a `long` integer.

+ The ANSI standard requires `long` integers to be at least 32 bits and `short` or `int` integers to be at least 16 bits.

+ In most C, the remainder has the same sign as the dividend.


### Exercise

7-2. Write a portable version of the `atol` function, which takes a pointer to a null-terminated character string as its argument and returns the corresponding `long` value. Assume:

1. The input will always represent a valid `long` integer, so `atol` need not check for the input being out of bounds;

2. The only valid input characters are digits and `+` and `-` signs. The input ends at the first invalid character.


        long atol(char *s)
        {
            long r = 0;
            int neg = 0;
            switch (*s){
              case '-' :
                  neg = 1;
                  /* no break here*/
              case '+' :
                  s++;
                  break;
            }
            while (*s >= '0' && *s <= '9'){
                int n = *s++ - '0';
                if(neg)
                    n = -n;
                r = r * 10 + n;
            }

            return r;
        }
