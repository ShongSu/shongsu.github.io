---
layout: post
title: Advanced Data Structure / 高级数据结构
date: 2017-01-04 17:09
categories: [blog ]
tags: [Algorithm, ]
description:
---

## 1 - Circular Buffer / 环形缓冲区

Circular Buffer 又叫做 Ring Buffer，这里用C语言数组实现一个简单的环形缓冲区。


      #include<stdio.h>
      #include<ctype.h>

      #define NMAX 8

      int iput = 0; /* 环形缓冲区的当前放入位置 */
      int iget = 0; /* 缓冲区的当前取出位置 */
      int n = 0; /* 环形缓冲区中的元素总数量 */

      double buffer[NMAX];

      /* 环形缓冲区的地址编号计算函数，如果到达唤醒缓冲区的尾部，将绕回到头部。
       * 环形缓冲区的有效地址编号为：0到(NMAX-1)
       */
      int addring (int i) {
        return (i+1) == NMAX ? 0 : i+1;
      }

      /* 从环形缓冲区中取一个元素 */

      double get(void) {
        int pos;
        if (n > 0) {
          pos = iget;
          iget = addring(iget);
          n--;
          return buffer[pos];
        } else {
          printf("Buffer is empty\n");
          return 0.0;
        }
      }

      /* 向环形缓冲区中放入一个元素*/
      void put(double z) {
        if(n < NMAX) {
          buffer[iput] = z;
          iput = addring(iput);
          n++;
        } else {
          printf("Buffer is fulln");
        }
      }

      int main(void)
      {
        char opera[5];
        double z;
        do {
          printf("Please input p|g|e?");
          scanf("%s",&opera);
          switch(tolower(opera[0])) {
            case 'p': printf("Please input a float number?");
                      scanf("%lf", &z);
                      put(z);
                      break;

            case 'g': z = get();
                      printf("%8.2f from Buffer/n", z);
                      break;

            case 'e': printf("Endn");
                      break;

            default: printf("%s - Operation command error! \n", opera);
          }/* end switch */
        } while(opera[0] != 'e');

        return 0;
      }
