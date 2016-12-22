+++
Categories = ["Algorithm"]
Description = ""
Tags = ["Leetcode", "Bit Manipulation"]
date = "2016-11-13T10:46:27-05:00"
menu = "main"
title = "Bit Manipulation"

+++

# Clever Tricks
### Get the rightmost set bit
```
	num &= -num;
```
### Set the rightmost 1 to 0
```
	num &= (num-1)
```
### Facts
- ~(num-1) == -num

# Single Number III
- [LeetCode 260](https://leetcode.com/problems/single-number-iii/)

Given an array of numbers nums, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once.

## Example:
Given nums = [1, 2, 1, 3, 2, 5], return [3, 5].

## Solution
In the first pass of for loop, we XOR all elements in the array, and get the XOR of the two numbers we need to find.

The second pass is hard to come up with, we use the trick in the first session to get the rightmost set bit of the XOR of the two target number. We can divide all elements in two groups, and the target two numbers will fall into different groups.

```
    public int[] singleNumber(int[] nums) {
        int diff = 0;
        for(int num: nums) {
            diff ^= num;
        }
        diff &= -diff;
        int[] result = {0,0};
        for(int num: nums) {
            if((num & diff) == 0)
                result[0] ^= num;
            else
                result[1] ^= num;
        }
        return result;
    }
```

# Single Number II
- [LeetCode] (https://leetcode.com/problems/single-number-ii/)

Given an array of integers, every element appears three times except for one. Find that single one.

## Solution
```
    public int singleNumber(int[] nums) {
        int ones = 0, twos = 0;
        for(int num: nums) {
            ones = (ones ^ num) & ~twos;
            twos = (twos ^ num) & ~ones;
        }
        return ones;
    }
```

# Missing Number
- [LeetCode] (https://leetcode.com/problems/missing-number/)

Given an array containing n distinct numbers taken from 0, 1, 2, ..., n, find the one that is missing from the array.

## Example
Given nums = [0, 1, 3] return 2.

## Solution

```
    public int missingNumber(int[] nums) {
        int result = nums.length;
        for(int i = 0; i < nums.length; i ++) {
            result ^= i;
            result ^= nums[i];
        }
        return result;
    }
```


