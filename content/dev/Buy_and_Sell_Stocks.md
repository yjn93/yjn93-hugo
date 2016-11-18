+++
Categories = ["Algorithm"]
Description = ""
Tags = ["Leetcode", "dp"]
date = "2016-11-18T09:54:11-05:00"
menu = "main"
title = "Buy and Sell Stocks"

+++
# One transaction
- [LeetCode 121](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

## Solution
### My approach
Track the min value, update the maxProfit.

```
    public int maxProfit(int[] prices) {
        int min = Integer.MAX_VALUE;
        int max = 0;
        for(int price: prices) {
            min = Math.min(min, price);
            max = Math.max(max, price-min);
        }
        return max;
    }
```

### Kadane's Algorithm Solution
Kadane's Algorithm is used to solve Maximum Subarray problem
Here, the logic is to calculate the difference (maxCur += prices[i] - prices[i-1]) of the original array, and find a contiguous subarray giving maximum profit. If the difference falls below 0, reset it to zero.
```
    public int maxProfit(int[] prices) {
        int curMax = 0;
        int max = 0;
        for(int i = 1; i < prices.length; i ++) {
            curMax = Math.max(0, curMax+prices[i]-prices[i-1]);
            max = Math.max(max, curMax);
        }
        return max;
    }
```
## Maximum Subarray Problem
- [LeetCode 53](https://leetcode.com/problems/maximum-subarray/)

### Example
given the array [-2,1,-3,4,-1,2,1,-5,4], the contiguous subarray [4,-1,2,1] has the largest sum = 6.

### Solution
Using Kadane's algorithm, which consists of a scan through the array values, computing at each position the maximum (positive sum) subarray ending at that position. This subarray is either empty (in which case its sum is zero) or consists of one more element than the maximum subarray ending at the previous position.

```
    public int maxSubArray(int[] nums) {
        int max = Integer.MIN_VALUE;
        int leftMax = 0;
        for(int num: nums) {
            max = Math.max(max, leftMax+num);
            leftMax = Math.max(0, leftMax+num);
        }
        return max;
    }
```
### Notice
The corner case: all elements are negative.

# Many transactions
- [LeetCode 122](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

## Peak and Valley approach

```
    public int maxProfit(int[] prices) {
        int result = 0;
        int idx = 0;
        while(idx < prices.length-1) {
            while(idx+1 < prices.length && prices[idx+1] <= prices[idx])
                idx ++;
            int low = prices[idx];
            while(idx+1 < prices.length && prices[idx+1] >= prices[idx])
                idx ++;
            result += prices[idx]-low;
            idx ++;
        }
        return result;
    }
```

## Simple one pass
we can directly keep on adding the difference between the consecutive numbers of the array if the second number is larger than the first one, and at the total sum we obtain will be the maximum profit.


```
    public int maxProfit(int[] prices) {
        int max = 0;
        for(int i = 1; i < prices.length; i ++) {
            if(prices[i] > prices[i-1])
                max += prices[i]-prices[i-1];
        }
        return max;
    }
```

# Multiple transactions with cooldown
- [LeetCode 309](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)


## Example

```
prices = [1, 2, 3, 0, 2]
maxProfit = 3
transactions = [buy, sell, cooldown, buy, sell]
```
## Solution
Dynamic programming: tracking the maxProfit for ending with sell and buy

```
buy[i] = max(sell[i-2]-price, buy[i-1])
sell[i] = max(buy[i-1]+price, sell[i-1])
```
Can reduce the space:

```
preBuy = curBuy;
curBuy = max(preSell-num, preBuy);
preSell = curSell;
curSell = max(preBuy+num, preSell);
```

Java code

```
    public int maxProfit(int[] prices) {
        int preSell = 0, curSell = 0;
		int curBuy = Integer.MIN_VALUE, preBuy;
        for(int price: prices) {
            preBuy = curBuy;
            curBuy = Math.max(preSell - price, preBuy);
            preSell = curSell;
            curSell = Math.max(preBuy + price, preSell);
        }
        return curSell;
    }

```


