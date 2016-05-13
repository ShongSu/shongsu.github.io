---
layout: post
title: Algorithm(２)-Dynamic Programming / 算法(2)-动态规划
date: 2016-05-09 23:52
categories: [blog ]
tags: [Algorithm, ]
description:
---

<center>**------English (英文)------**</center>

## Problem 1. Minimum Coin Change

Given a amount `A` and `n` coins, `v1<v2<v3<...<vn`. Write a pro­gram to find out min­i­mum num­bers of coins required to make the change for the amount `A`.

Example.

        Amount: 5
        Coins [] = 1, 2, 3.
        No. of ways to make the change are : { 1,1,1,1,1} , {1,1,1,2}, {2,2,1},{1,1,3} and {3,2}.
        So as we can see minimum number of coins required are 2 (3+2=5).


### Solutions:

            /** int coins[]: set of value of coins.
                int n: size of coins[], that is types of coins.
                int amount: total value for change.
            */
            int change(int coins[], int n, int amount)
            {
                int *d = new int[amount+1]; // this will store the optimal solution for all the values -- from 0 to given amount.
            	int *CC = new int[n];    // resets for every smaller problems

                d[0] = 0; // 0 coins are required to make the change for 0

            	// now solve for all the amounts
            	for (int amt = 1; amt <= amount; amt++)
            	{
            		for (int j = 0; j < n; j++)
            		{
            			CC[j] = -1;
            		}

            		// Now try taking every coin one at a time and fill the solution in the CC[]
            		for (int j = 0; j < n; j++)
            		{
            			if (coins[j] <= amt) // check if coin value is less than amount
            			{
            			    // select the coin and add 1 to solution of (amount-coin value)
            				CC[j] = d[amt - coins[j]] + 1;
            			}
            		}

            		//Now solutions for amt using all the coins is stored in CC[] take out the minimum (optimal) and store in d[amt]
            		d[amt]=-1;
            		for(int j=0;j<n;j++)
            		{
            		    if(CC[j]>0 && (d[amt]==-1 || d[amt]>CC[j]))
            		    {
            				d[amt]=CC[j];
            			}

            		}
            	}

            	//return the optimal solution for amount
            	return d[amount];

            }



## Problem 2. 0-1 Knapsack

We have `n` items want to store in a bag, and the bag has `v` space, item `i` has `value[i]` and takes size `weight[i]`. Now we want to the items have combined size at most `v`, and the total value of items is as large as possible. Note that we can not store parts of items, it is the whole item or nothing.


Example.

        items: n = 5, space: v = 10.
        weight [] = 2, 2, 6, 5, 4.
        values [] =  6, 3, 5, 4, 6.
        Thus, the ideal solution is pick up 1st, 2nd and 5th items to make the maximum values.


### Solutions:


        #include <stdio.h>
        #define max(a,b) ((a)>(b)?(a):(b))  

        int main()
        {
            int v = 10 ;    
            int n = 5 ;      

            int value[] = {6 , 3 , 5 , 4 , 6};       
            int weight[] = {2 , 2 , 6 , 5 , 4};

            int kp[6][11]={};   // initialize kp[][] = 0
            int x[5]={};        // initialize x[] = 0

            for(int i=0;i<=n;i++)
                for(int j=0;j<=v;j++)
                    if(i>0 && weight[i-1]<=j)
                        kp[i][j] = max( kp[i-1][j], kp[i-1][j-weight[i-1]]+value[i-1]);    


            printf("The total amount is %d\n", kp[n][v]);

            // /* print array kp to show details */
            // for(int i=0;i<=n;i++)   
            // {
            //     for(int j=0;j<=v;j++)
            //         printf("%4d ",kp[i][j]);
            //     printf("\n");
            // }

            int j = 10;
          	for(int i=n; i>0; --i)
          	{
          		if(kp[i][j] > kp[i-1][j])
          		{
          		    x[i-1] = 1;
          			j = j - weight[i-1];
          		}
          	}

          	printf("Items state in the bag: ");
          	for(int i=0; i<n; ++i)
          	    printf("%d ", x[i]);
          	printf("\n");

        }




## To be continued....
