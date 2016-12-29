+++
Categories = ["Algorithm"]
Description = ""
Tags = ["LeetCode", "Bit Manipulation"]
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
- [LeetCode 137] (https://leetcode.com/problems/single-number-ii/)

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
- [LeetCode 268] (https://leetcode.com/problems/missing-number/)

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
# Integer Replacement
- [LeetCode 397](https://leetcode.com/problems/integer-replacement/)

Given a positive integer n and you can do operations as follow:

If n is even, replace n with n/2.

If n is odd, you can replace n with either n + 1 or n - 1.

What is the minimum number of replacements needed for n to become 1?

## Example
Input: 8

Output: 3

Explanation: 8 -> 4 -> 2 -> 1

Input: 7

Output: 4

Explanation: 7 -> 8 -> 4 -> 2 -> 1 or 7 -> 6 -> 3 -> 2 -> 1

## Solution

Recursion will cause StackOverFlow. There are two ways for this problem. One uses iteration, one use bit manipulation.

### Iteration

 When n is odd it can be written into the form n = 2k+1 (k is a non-negative integer.). That is, n+1 = 2k+2 and n-1 = 2k. Then, (n+1)/2 = k+1 and (n-1)/2 = k. So one of (n+1)/2 and (n-1)/2 is even, the other is odd. And the "best" case of this problem is to divide as much as possible. Because of that, always pick n+1 or n-1 based on if it can be divided by 4. The only special case of that is when n=3 you would like to pick n-1 rather than n+1.

```
    public int integerReplacement(int n) {
        if(n == Integer.MAX_VALUE) return 32;
        int count = 0;
        while(n != 1) {
            if(n % 2 == 0) {
                n /= 2;
            } else {
                if((n + 1) % 4 == 0 && n != 3)
                    n += 1;
                else
                    n -= 1;
            }
            count ++;
        }
        return count;
    }
```

### Bit Manipulation

We can use the same idea to do bit manipulation. When n is odd, we only need to check the last two bits of n. If the second least significant bit is 1, then increase n by 1, unless n is 3.

```
    public int integerReplacement(int n) {
        int count = 0;
        while(n != 1) {
            if(n % 2 == 0)
                n >>>= 1;
            else if(n == 3 || ((n >>> 1) & 1) == 0)
                n --;
            else 
                n ++;
            count ++;
        }
        return count;
    }
```


