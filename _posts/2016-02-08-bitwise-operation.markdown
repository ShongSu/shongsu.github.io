---
layout: post
title: Classical Programming Problem - Bitwise Operation / 经典编程问题 - 位运算
date: 2016-02-08 21:48
post-link:
---

1．判断奇偶
只要根据最未位是0还是1来决定，为0就是偶数，为1就是奇数。因此可以用if ((a & 1) == 0)代替if (a % 2 == 0)来判断a是不是偶数。

下面程序将输出0到100之间的所有奇数。

    for (i = 0; i < 100; ++i)  
        if (i & 1)  
            printf("%d ", i);  
    putchar('\n');


2．交换两数

    void Swap(int &a, int &b)  
    {  
        if (a != b)  
        {  
            a ^= b;  
            b ^= a;  
            a ^= b;  
        }  
    }  

3．变换符号
变换符号就是正数变成负数，负数变成正数。

    int SignReversal(int a)  
    {  
        return ~a + 1;  
    }



4．求绝对值

位操作也可以用来求绝对值，对于负数可以通过对其取反后加1来得到正数。对-6可以这样：

      1111 1010(二进制) –取反->0000 0101(二进制) -加1-> 0000 0110(二进制)

来得到6。

因此先移位来取符号位，int i = a >> 31;要注意如果a为正数，i等于0，为负数，i等于-1。然后对i进行判断——如果i等于0，直接返回。否之，返回~a+1。完整代码如下：

    int my_abs(int a)  
    {  
        int i = a >> 31;  
        return i == 0 ? a : (~a + 1);  
    }  

现在再分析下。对于任何数，与0异或都会保持不变，与-1即0xFFFFFFFF异或就相当于取反。因此，a与i异或后再减i（因为i为0或-1，所以减i即是要么加0要么加1）也可以得到绝对值。所以可以对上面代码优化下：

    int my_abs(int a)  
    {  
        int i = a >> 31;  
        return ((a ^ i) - i);  
    }

5．二进制中1的个数

统计二进制中1的个数可以直接移位再判断，当然像《编程之美》书中用循环移位计数或先打一个表再计算都可以。本文详细讲解一种高效的方法。以34520为例，可以通过下面四步来计算其二进制中1的个数二进制中1的个数。

第一步：每2位为一组，组内高低位相加

      10 00 01 10  11 01 10 00

  -->01 00 01 01  10 01 01 00

第二步：每4位为一组，组内高低位相加

      0100 0101 1001 0100

  -->0001 0010 0011 0001

第三步：每8位为一组，组内高低位相加

      00010010 00110001

  -->00000011 00000100

第四步：每16位为一组，组内高低位相加

      00000011 00000100

  -->00000000 00000111

这样最后得到的00000000 00000111即7即34520二进制中1的个数。类似上文中对二进制逆序的做法不难实现第一步的代码：

       x = ((x & 0xAAAA) >> 1) + (x & 0x5555);

好的，有了第一步，后面几步就请读者完成下吧，先动动笔再看下面的完整代码

    #include <stdio.h>  
    template <class T>  
    void PrintfBinary(T a)  
    {  
        int i;  
        for (i = sizeof(a) * 8 - 1; i >= 0; --i)  
        {  
            if ((a >> i) & 1)  
                putchar('1');  
            else   
                putchar('0');  
            if (i == 8)  
                putchar(' ');  
        }  
        putchar('\n');  
    }  
    int main()  
    {  

        unsigned short a = 34520;  
        printf("原数    %6d的二进制为:  ", a);  
        PrintfBinary(a);  

        a = ((a & 0xAAAA) >> 1) + (a & 0x5555);  
        a = ((a & 0xCCCC) >> 2) + (a & 0x3333);  
        a = ((a & 0xF0F0) >> 4) + (a & 0x0F0F);  
        a = ((a & 0xFF00) >> 8) + (a & 0x00FF);     
        printf("计算结果%6d的二进制为:  ", a);     
        PrintfBinary(a);  
        return 0;  
    }

另一种方法：

    int BitCount(unsigned int n)
    {
        int count = 0; // 计数器
        while (n > 0)
        {
            if((n & 1) == 1) // 当前位是1
                count++ ; // 计数器加1
            n = n >> 1 ; // 移位
        }
        return count ;
    }


6．寻找只出现一次的数

    int main()  
    {          
        const int MAXN = 15;  
        int a[MAXN] = {1, 347, 6, 9, 13, 65, 889, 712, 889, 347, 1, 9, 65, 13, 712};  
        int lostNum = 0;  
        for (int i = 0; i < MAXN; i++)  
            lostNum ^= a[i];  
        printf("缺失的数字为:  %d\n", lostNum);     
        return 0;  
    }  
