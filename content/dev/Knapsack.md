+++
Categories = ["Algorithm"]
Description = "Using 01 knapsack to Partition Equal Subset Sum"
Tags = ["LeetCode", "Basic algorithm", "DP"]
date = "2016-12-21T12:19:33-05:00"
menu = "main"
title = "Knapsack"

+++

# Knapsack problem

Given weights (wt[i]) and values (val[i]) of n items, put these items in a knapsack of capacity W to get the maximum total value in the knapsack. 

## Basic dp
```
K[i][v]=max{K[i-1][v],K[i-1][v-wt[i]]+val[i]}

int knapSack(int W, int wt[], int val[], int n)
{
   int i, w;
   int K[n+1][W+1];
 
   // Build table K[][] in bottom up manner
   for (i = 0; i <= n; i++)
   {
       for (w = 0; w <= W; w++)
       {
           if (i==0 || w==0)
               K[i][w] = 0;
           else if (wt[i-1] <= w)
                 K[i][w] = max(val[i-1] + K[i-1][w-wt[i-1]],  K[i-1][w]);
           else
                 K[i][w] = K[i-1][w];
       }
   }
 
   return K[n][W];
}
```

## Space optimization

To make sure in ith loop, K[v-wt[i]] stores previous K[i-1][v-wt[i]], the inner for loop should index in decreasing order.

```
for i = 1 .. N
	for v = W .. 0
		K[v]=max{K[v],K[v-wt[i]]+val[i]}

```

# Partition Equal Subset Sum
- [LeetCode 416](https://leetcode.com/problems/partition-equal-subset-sum/)

Given a non-empty array containing only positive integers, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

## Example:
```
Input: [1, 5, 11, 5]

Output: true

Explanation: The array can be partitioned as [1, 5, 5] and [11].

Input: [1, 2, 3, 5]

Output: false

Explanation: The array cannot be partitioned into equal sum subsets.

```

## Solution
```
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for(int num: nums) {
            sum += num;
        }
        if(sum % 2 != 0) return false;
        sum /= 2;
        boolean[] dp = new boolean[sum + 1];
        dp[0] = true;
        for(int i = 0; i < nums.length; i ++) {
            for(int j = sum; j >= nums[i]; j --) {
                dp[j] = dp[j] || dp[j - nums[i]];
            }
        }
        
        return dp[sum];
    }
```

