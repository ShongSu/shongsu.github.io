---
layout: post
title: Max Subsequence Sum Problem / 最大子序列和问题
date: 2015-11-17 21:33
categories: [blog ]
tags: [Algorithm, ]
description:
---

<center>**------English (英文)------**</center>

Given (possibly negative) integers a1, a2, ..., an, find the maximum value of ∑ ak. The maximum subsequence sum is defined to be 0 if all the integers are negative.

For example, given the sequence -2, 11, -4, 13, -5, -2, the maximum subsequence sum is 20: a2 through a4.


The most obvious approach is an algorithm with O(N^3).

<1> Algorithm 1 -- `O(N^3)`

```
int MaxSubsequenceSum(const int A[], int N)
{
```
int ThisSum, MaxSum, i, j, k;
```

```

```

```
MaxSum = 0;
for(i = 0; i < N; i++)
```
for(j = i; j < N; j++)
{
    ThisSum = 0;
    for(k = i; k <= j; k++ )
        ThisSum += A[k];
```
```

```

```

```
```
    if(ThisSum > MaxSum)
        MaxSum = ThisSum;
}
```
return MaxSum;
```
}
```

A better approach may perform as O(N^2).

<2> Algorithm 2 -- `O(N^2)`

```
int MaxSubsequenceSum(const int A[], int N)
{
```
int ThisSum, MaxSum, i, j;
```

```

```

```
MaxSum = 0;
for(i = 0; i < N; i++)
{
```
ThisSum = 0;
for(j = i; j < N; j++)
{
    ThisSum += A[j];
    if(ThisSum > MaxSum)
        MaxSum = ThisSum;
}
```
}
```

```

```

```
return MaxSum;
```
}
```


We also may apply divide-and-conquer approach for this problem.

<3> Algorithm 3 -- `O(NlogN)`

```
int MaxSubSum(const int A[], int Left, int Right)
{
```
int MaxLeftSum, MaxRightSum;
int MaxLeftBorderSum, MaxRightBorderSum;
int LeftBorderSum, RightBorderSum;
int Center, i;
```

```

```

```
if(Left == Right)
{
```
if(A[Left] > 0)
    return A[Left];
else
    return 0;
```
}
```

```

```

```
Center = (Left + Right) / 2;
MaxLeftSum = MaxSubSum(A, Left, Center);
MaxRightSum = MaxSubSum(A, Center + 1, Right);
```

```

```

```
MaxLeftBorderSum = 0; LeftBorderSum = 0;
for(i = Center; i >= Left; i--)
{
```
LeftBorderSum += A[i];
if(LeftBorderSum > MaxLeftBorderSum)
    MaxLeftBorderSum = LeftBorderSum;
```
}
```

```

```

```
MaxRightBorderSum = 0; RightBorderSum = 0;
for(i = Center; i <= Right; i++)
{
```
RightBorderSum += A[i];
if(RightBorderSum > MaxRightBorderSum)
    MaxRightBorderSum = RightBorderSum;
```
}
```

```

```

```
return Max3(MaxLeftSum, MaxRightSum, MaxLeftBorderSum + MaxRightBorderSum);
```

```

```
}
```

```
int MaxSubsequenceSum(const int A[], int N)
{
```
return MaxSubSum(A, 0, N - 1);
```
}
```

However, there has an optimal solution can achieve O(N).

<4> Algorithm 4 -- `O(N)`

```
int MaxSubsequenceSum(const int A[], int N)
{
```
int ThisSum, MaxSum, i;
```

```

```

```
ThisSum = 0; MaxSum = 0;
for(i = 0; i < N; i++)
{
```
ThisSum += A[i];
if(ThisSum > MaxSum)
    MaxSum = ThisSum;
else if(ThisSum < 0)
    ThisSum = 0;
```
}
```

```

```

```
return MaxSum;
```
}
```
