+++
Categories = ["Algorithm"]
Description = ""
Tags = ["LeetCode", "Two pointers"]
date = "2016-12-23T11:06:24-05:00"
menu = "main"
title = "TwoPointers"

+++

# Container With Most Water

-[LeetCode 11](https://leetcode.com/problems/container-with-most-water/)

Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

## Sample
Reference from LeetCode top solution.

Draw a matrix where the row is the first line, and the column is the second line. For example, say n=6.

In the figures below, x means we don't need to compute the volume for that case: (1) On the diagonal, the two lines are overlapped; (2) The lower left triangle area of the matrix is symmetric to the upper right area.

We start by computing the volume at (1,6), denoted by o. Now if the left line is shorter than the right line, then all the elements left to (1,6) on the first row have smaller volume, so we don't need to compute those cases (crossed by ---).

```
		  1 2 3 4 5 6
		1 x ------- o
		2 x x
		3 x x x 
		4 x x x x
		5 x x x x x
		6 x x x x x x
```

Next we move the left line and compute (2,6). Now if the right line is shorter, all cases below (2,6) are eliminated.

```
		  1 2 3 4 5 6
		1 x ------- o
		2 x x       o
		3 x x x     |
		4 x x x x   |
		5 x x x x x |
		6 x x x x x x

```

And no matter how this o path goes, we end up only need to find the max value on this path, which contains n-1 cases.

```
		  1 2 3 4 5 6
		1 x ------- o
		2 x x - o o o
		3 x x x o | |
		4 x x x x | |
		5 x x x x x |
		6 x x x x x x
```
## Solution
```
    public int maxArea(int[] height) {
        int left = 0, right = height.length-1;
        int max = 0;
        while(left < right) {
            max = Math.max(max, Math.min(height[left], height[right]) * (right - left));
            if(height[left] < height[right])
                left ++;
            else 
                right --;
        }
        return max;
    }
```

# Sort Colors
-[LeetCode 75](https://leetcode.com/problems/sort-colors/)

Given an array with n objects colored red, white or blue, sort them so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

## Solution

```
    public void sortColors(int[] nums) {
        int left = 0, right = nums.length - 1;
        for(int i = 0; i <= right; i ++) {
            while(nums[i] == 2 && i < right)
                swap(nums, i, right --);
            while(nums[i] == 0 && i > left) 
                swap(nums, i, left ++);
        }
    }
```

