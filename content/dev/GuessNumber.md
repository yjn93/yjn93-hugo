+++
Categories = ["Algorithm"]
Description = ""
Tags = ["LeetCode", "DP"]
date = "2016-12-24T13:02:42-05:00"
menu = "main"
title = "GuessNumber"

+++

# Guess Number Higher or Lower II

- [LeetCode](https://leetcode.com/problems/guess-number-higher-or-lower-ii/)

We are playing the Guess Game. The game is as follows:

I pick a number from 1 to n. You have to guess which number I picked.

Every time you guess wrong, I'll tell you whether the number I picked is higher or lower.

However, when you guess a particular number x, and you guess wrong, you pay $x. You win the game when you guess the number I picked.

## Example
```
n = 10, I pick 8.

First round:  You guess 5, I tell you that it's higher. You pay $5.
Second round: You guess 7, I tell you that it's higher. You pay $7.
Third round:  You guess 9, I tell you that it's lower. You pay $9.

Game over. 8 is the number I picked.

You end up paying $5 + $7 + $9 = $21.
```

## Solution

For each number x in range[i~j]

we do: result_when_pick_x = x + max{DP([i~x-1]), DP([x+1, j])}

--> // the max means whenever you choose a number, the feedback is always bad and therefore leads you to a worse branch.

then we get DP([i~j]) = min{xi, ... ,xj}

--> // this min makes sure that you are minimizing your cost.

### Recursive DP
```
    public int getMoneyAmount(int n) {
        int[][] table = new int[n+1][n+1];
        return dp(table, 1, n);
    }
    
    public int dp(int[][] table, int start, int end) {
        if(start >= end) return 0;
        if(table[start][end] != 0) return table[start][end];
        int result = Integer.MAX_VALUE;
        for(int k = start; k <= end; k ++) {
            result = Math.min(result, k + Math.max(dp(table, start, k-1), dp(table, k+1, end)));
        }
        table[start][end] = result;
        return result;
    }
```

### Bottom-up DP

Fill the table's every column. For each column, it represents the up limit of the range, and we should fill the column from (j-1, j) to (1, j).

Because for example, if i == 1, j == 5, when k = 3, we need table[4][5] be exist.

Also notice the base case is when i + 1 == j, we choose i over j.
```
    public int getMoneyAmount(int n) {
        int[][] table = new int[n+1][n+1];
        for(int j = 2; j <= n; j ++) {
            for(int i=j-1; i>0; i--) {
                int globalMin = Integer.MAX_VALUE;
                for(int k = i + 1; k < j; k ++) {
                    int localMax = k + Math.max(table[i][k-1], table[k+1][j]);
                    globalMin = Math.min(globalMin, localMax);
                }
                table[i][j] = i + 1 == j ? i : globalMin;
            }
        }
        return table[1][n];
    }

```


