---
layout: post
title: Algorithm(２)-Dynamic Programming / 算法(2)-动态规划
date: 2016-05-09 23:52
categories: [blog ]
tags: [Algorithm, ]
description:
---

<center>**------English (英文)------**</center>

## Problem 1.

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


## To be continued....
