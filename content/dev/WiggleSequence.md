+++
Categories = ["Algorithm"]
Description = ""
menu = "main"
Tags = ["LeetCode", "DP"]
date = "2016-12-24T19:38:38-05:00"
title = "Wiggle Subsequence"

+++

# Wiggle Subsequence

- [LeetCode 376](https://leetcode.com/problems/wiggle-subsequence/)

A sequence of numbers is called a wiggle sequence if the differences between successive numbers strictly alternate between positive and negative. The first difference (if one exists) may be either positive or negative. A sequence with fewer than two elements is trivially a wiggle sequence.

## Example
```
Input: [1,7,4,9,2,5]
Output: 6
The entire sequence is a wiggle sequence.

Input: [1,17,5,10,13,15,10,5,16,8]
Output: 7
There are several subsequences that achieve this length. One is [1,17,10,13,10,16,8].

Input: [1,2,3,4,5,6,7,8,9]
Output: 2
```

## Solution

Use two variable up and down to track current maximum length of subsequence that end with up and down respectively.

```
    public int wiggleMaxLength(int[] nums) {

        if(nums.length < 2) return nums.length;

        int up = 1, down = 1;

        for(int i = 1; i < nums.length; i ++) {

            if(nums[i] > nums[i-1])

                up = down + 1;

            else if (nums[i] < nums[i-1])

                down = up + 1;

        }

        return Math.max(up, down);

    }
```

