---
layout: post
title: "LeetCode Questions: Solutions in JavaScript"
date: 2025-12-01
categories: leetcode javascript
---

# LeetCode Questions: Solutions in JavaScript

This post covers solutions to several LeetCode problems, including initial versions and improvements. Each solution is in JavaScript, with 1-2 follow-up questions.

## 1. Two Sum

**Problem:** Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

**Initial Solution (Brute Force):**
```js
function twoSum(nums, target) {
    for (let i = 0; i < nums.length; i++) {
        for (let j = i + 1; j < nums.length; j++) {
            if (nums[i] + nums[j] === target) {
                return [i, j];
            }
        }
    }
    return [];
}
```
**Improvement (Hash Map):**
```js
function twoSum(nums, target) {
    const map = new Map();
    for (let i = 0; i < nums.length; i++) {
        const complement = target - nums[i];
        if (map.has(complement)) {
            return [map.get(complement), i];
        }
        map.set(nums[i], i);
    }
    return [];
}
```

**Follow-ups:**
1. What if the array is sorted? Can you solve it in O(n) time?
2. Handle duplicates in the array?

## 121. Best Time To Buy and Sell Stock

**Problem:** You are given an array `prices` where `prices[i]` is the price of a given stock on the i-th day. You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

**Initial Solution (Brute Force):**
```js
function maxProfit(prices) {
    let max = 0;
    for (let i = 0; i < prices.length; i++) {
        for (let j = i + 1; j < prices.length; j++) {
            max = Math.max(max, prices[j] - prices[i]);
        }
    }
    return max;
}
```
**Improvement (One Pass):**
```js
function maxProfit(prices) {
    let minPrice = Infinity;
    let maxProfit = 0;
    for (let price of prices) {
        minPrice = Math.min(minPrice, price);
        maxProfit = Math.max(maxProfit, price - minPrice);
    }
    return maxProfit;
}
```

**Follow-ups:**
1. What if you can make multiple transactions?
2. What if there's a transaction fee?

## 57. Insert Interval

**Problem:** You are given a set of non-overlapping intervals, sorted by start time. Insert a new interval into the intervals (merge if necessary).

**Initial Solution (Iterative):**
```js
function insert(intervals, newInterval) {
    const result = [];
    let i = 0;
    while (i < intervals.length && intervals[i][1] < newInterval[0]) {
        result.push(intervals[i]);
        i++;
    }
    while (i < intervals.length && intervals[i][0] <= newInterval[1]) {
        newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
        newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
        i++;
    }
    result.push(newInterval);
    while (i < intervals.length) {
        result.push(intervals[i]);
        i++;
    }
    return result;
}
```
**Improvement:** This is already optimal, but ensure handling edge cases like empty intervals.

**Follow-ups:**
1. What if intervals are not sorted?
2. Merge all overlapping intervals in a list?

## 15. 3Sum

**Problem:** Given an integer array nums, return all the triplets [nums[i], nums[j], nums[k]] such that i != j != k, and nums[i] + nums[j] + nums[k] == 0.

**Initial Solution (Brute Force):**
```js
function threeSum(nums) {
    const result = [];
    nums.sort((a, b) => a - b);
    for (let i = 0; i < nums.length - 2; i++) {
        for (let j = i + 1; j < nums.length - 1; j++) {
            for (let k = j + 1; k < nums.length; k++) {
                if (nums[i] + nums[j] + nums[k] === 0) {
                    result.push([nums[i], nums[j], nums[k]]);
                }
            }
        }
    }
    return result;
}
```
**Improvement (Two Pointers):**
```js
function threeSum(nums) {
    const result = [];
    nums.sort((a, b) => a - b);
    for (let i = 0; i < nums.length - 2; i++) {
        if (i > 0 && nums[i] === nums[i - 1]) continue;
        let left = i + 1, right = nums.length - 1;
        while (left < right) {
            const sum = nums[i] + nums[left] + nums[right];
            if (sum === 0) {
                result.push([nums[i], nums[left], nums[right]]);
                while (left < right && nums[left] === nums[left + 1]) left++;
                while (left < right && nums[right] === nums[right - 1]) right--;
                left++;
                right--;
            } else if (sum < 0) {
                left++;
            } else {
                right--;
            }
        }
    }
    return result;
}
```

**Follow-ups:**
1. What if you need to find quadruplets?
2. Handle duplicates efficiently?

## 238. Product of Array Except Self

**Problem:** Given an integer array nums, return an array answer such that answer[i] is equal to the product of all the elements of nums except nums[i].

**Initial Solution (Division):**
```js
function productExceptSelf(nums) {
    const total = nums.reduce((a, b) => a * b, 1);
    return nums.map(num => total / num);
}
```
**Improvement (No Division):**
```js
function productExceptSelf(nums) {
    const result = [];
    let left = 1;
    for (let i = 0; i < nums.length; i++) {
        result[i] = left;
        left *= nums[i];
    }
    let right = 1;
    for (let i = nums.length - 1; i >= 0; i--) {
        result[i] *= right;
        right *= nums[i];
    }
    return result;
}
```

**Follow-ups:**
1. What if the array contains zeros?
2. Can you do it in O(1) extra space?

## 39. Combination Sum

**Problem:** Given an array of distinct integers candidates and a target integer target, return a list of all unique combinations of candidates where the chosen numbers sum to target.

**Initial Solution (Recursion):**
```js
function combinationSum(candidates, target) {
    const result = [];
    function backtrack(start, current, sum) {
        if (sum === target) {
            result.push([...current]);
            return;
        }
        for (let i = start; i < candidates.length; i++) {
            if (sum + candidates[i] > target) continue;
            current.push(candidates[i]);
            backtrack(i, current, sum + candidates[i]);
            current.pop();
        }
    }
    backtrack(0, [], 0);
    return result;
}
```
**Improvement:** Add sorting and pruning for efficiency.

**Follow-ups:**
1. What if candidates have duplicates?
2. Find combinations that sum to target with exactly k numbers?

## 56. Merge Intervals

**Problem:** Given an array of intervals where intervals[i] = [starti, endi], merge all overlapping intervals.

**Initial Solution (Sort and Merge):**
```js
function merge(intervals) {
    if (intervals.length === 0) return [];
    intervals.sort((a, b) => a[0] - b[0]);
    const result = [intervals[0]];
    for (let i = 1; i < intervals.length; i++) {
        if (result[result.length - 1][1] >= intervals[i][0]) {
            result[result.length - 1][1] = Math.max(result[result.length - 1][1], intervals[i][1]);
        } else {
            result.push(intervals[i]);
        }
    }
    return result;
}
```
**Improvement:** This is optimal.

**Follow-ups:**
1. What if intervals are not sorted initially?
2. Find the maximum number of overlapping intervals?

## 169. Majority Element

**Problem:** Given an array nums of size n, return the majority element (appears more than n/2 times).

**Initial Solution (Hash Map):**
```js
function majorityElement(nums) {
    const map = {};
    for (let num of nums) {
        map[num] = (map[num] || 0) + 1;
        if (map[num] > nums.length / 2) return num;
    }
}
```
**Improvement (Boyer-Moore Voting):**
```js
function majorityElement(nums) {
    let candidate = nums[0];
    let count = 1;
    for (let i = 1; i < nums.length; i++) {
        if (count === 0) {
            candidate = nums[i];
            count = 1;
        } else if (nums[i] === candidate) {
            count++;
        } else {
            count--;
        }
    }
    return candidate;
}
```

**Follow-ups:**
1. What if there is no majority element?
2. Find all elements that appear more than n/3 times?

## 75. Sort Colors

**Problem:** Given an array nums with n objects colored red, white, or blue, sort them in-place so that objects of the same color are adjacent.

**Initial Solution (Sort):**
```js
function sortColors(nums) {
    nums.sort((a, b) => a - b);
}
```
**Improvement (Dutch National Flag):**
```js
function sortColors(nums) {
    let low = 0, mid = 0, high = nums.length - 1;
    while (mid <= high) {
        if (nums[mid] === 0) {
            [nums[low], nums[mid]] = [nums[mid], nums[low]];
            low++;
            mid++;
        } else if (nums[mid] === 1) {
            mid++;
        } else {
            [nums[mid], nums[high]] = [nums[high], nums[mid]];
            high--;
        }
    }
}
```

**Follow-ups:**
1. What if there are more than 3 colors?
2. Sort in O(n) time with constant space?

## 217. Contains Duplicate

**Problem:** Given an integer array nums, return true if any value appears at least twice in the array.

**Initial Solution (Set):**
```js
function containsDuplicate(nums) {
    const set = new Set();
    for (let num of nums) {
        if (set.has(num)) return true;
        set.add(num);
    }
    return false;
}
```
**Improvement:** This is efficient.

**Follow-ups:**
1. Find the first duplicate?
2. What if you need to find all duplicates?
