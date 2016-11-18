+++
Categories = ["Algorithm"]
Description = ""
Tags = ["LeetCode", "DP", "Bit Manipulation"]
date = "2016-11-12T15:10:24-05:00"
menu = "main"
title = "Counting Bits"

+++
# Problem description
- [LeetCode 338](https://leetcode.com/problems/counting-bits/)

Given a non negative integer number num. For every numbers i in the range 0 ≤ i ≤ num calculate the number of 1's in their binary representation and return them as an array.


## Example:
For num = 5 you should return [0,1,1,2,1,2]

## My DP Solution
This problem can be solved by dynamic programming because the count of bits follow certains rules as the number grows:

```
		0	0
		1	1
		2	10
		3	11
```

The number 2 is adding another bit 1 in front of number 0; the number 3 is adding another bit 1 at the beginning of number 1. So my solution is to track the previous size of numbers that can be represented by a fix number of bits. For example, we have 0, 1 initially, the size is 2. Then for 3, 4, we apply the formula:

```
dp[i] = dp[i-size] + 1
dp[2] = dp[2 - 2] + 1 = dp[0] + 1 
dp[3] = dp[3 - 2] + 1 = dp[1] + 1

```
Then, for number 4, we need more bits to represent it, so the previous size is updated to 4, and dp[4], dp[5], dp[6], dp[7] can be calculated by dp[0], dp[1], dp[2], dp[3]. My code is as following:

```
    public int[] countBits(int num) {
        if(num == 0) return new int[]{0};
        int[] dp = new int[num+1];
        dp[0] = 0;
        dp[1] = 1;
        int size = 2;
        for(int i = 2; i <= num; i ++) {
            if(i - size == size)
                size = i;
            dp[i] = 1 + dp[i-size];
        }
        return dp;
    }

```

## Another DP solution combining Bit Manipulation
Instead of adding the bit 1 infront of the previous cycle, this brilliant solution think it as deleting the last bit to obtain the number of bits without last bit, then add another counting for last bit if the last bit is 1.

4-line:
```
    public int[] countBits(int num) {
        int[] dp = new int[num+1];
        for(int i = 1; i <= num; i ++) 
            dp[i] = dp[i>>1] + (i&1);
        return dp;
    }
```

Notice the priority for & operator is lower than +, so we need to use () for (i&1).

