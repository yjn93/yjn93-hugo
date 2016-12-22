+++
Categories = ["Algorithm"]
Description = ""
Tags = ["LeetCode", "DP", "BackTracking"]
date = "2016-11-26T10:30:22-05:00"
menu = "main" 
title = "Combination Sum"

+++
# Combination Sum 3
- [LeetCode 216] (https://leetcode.com/problems/combination-sum-iii/)

Find all possible combinations of k numbers that add up to a number n, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.

## Example
```
k = 3, n = 7
[[1,2,4]]

k = 3, n = 9
[[1,2,6], [1,3,5], [2,3,4]]
```

## Solution
BackTracking
```
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        helper(result, new ArrayList<Integer>(), k, n, 1);
        return result;
    }
    
    public void helper(List<List<Integer>> result, List<Integer> cur, int k, int n, int start) {
        if(k == 0 && n == 0) {
            result.add(new ArrayList(cur));
            return;
        }
        if(k == 0 || start > n) return;
        for(int i = start; i <= 9 && i <= n ; i ++) {
            cur.add(i);
            helper(result, cur, k-1, n-i, i+1);
            cur.remove(cur.size()-1);
        }
    }
```


# Combination Sum 4
- [LeetCode 377] (https://leetcode.com/problems/combination-sum-iv/)

Given an integer array with all positive numbers and no duplicates, find the number of possible combinations that add up to a positive integer target.

## Example:
```
nums = [1, 2, 3]
target = 4

The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)

Note that different sequences are counted as different combinations.

Therefore the output is 7.
```

## Solution
I used recursive solution first, but raised Time Limit Exceeded. This problem actually can be solve in DP because we only need to calculate the possible combinations for a subTarget once.

```
    public int combinationSum4(int[] nums, int target) {
        int[] dp = new int[target+1];
        dp[0] = 1;
        for(int i = 1; i <= target; i ++) {
            for(int j = 0; j < nums.length; j ++) {
                if(i - nums[j] >= 0)
                    dp[i] += dp[i-nums[j]];
            }
        }
        return dp[target];
    }
```

