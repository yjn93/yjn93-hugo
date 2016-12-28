+++
date = "2016-12-24T21:26:42-05:00"
title = "hIndex"
Description = ""
menu = "main"
Categories = ["Algorithm"]
Tags = ["LeetCode", "Bucket sort"]

+++

# H Index

- [LeetCode 274](https://leetcode.com/problems/h-index/)

Given an array of citations (each citation is a non-negative integer) of a researcher, write a function to compute the researcher's h-index.

"A scientist has index h if h of his/her N papers have at least h citations each, and the other N − h papers have no more than h citations each."

For example, given citations = [3, 0, 6, 1, 5], which means the researcher has 5 papers in total and each of them had received 3, 0, 6, 1, 5 citations respectively. Since the researcher has 3 papers with at least 3 citations each and the remaining two with no more than 3 citations each, his h-index is 3.

## Solution

Use bucket sort. Suppose we have len papers, then h-index cannot exceed len, so we can create len+1 buckets. Each bucket stores the number of papers that have the according citation. Then we iterate from the back to the front of the buckets, whenever the total count exceeds the index of the bucket, meaning that we have the index number of papers that have reference greater than or equal to the index. Which will be our h-index result. The reason to scan from the end of the array is that we are looking for the greatest h-index.

```
    public int hIndex(int[] citations) {

        int len = citations.length;

        int[] bucket = new int[len+1];

        for(int c: citations) {

            if(c > len) bucket[len] ++;

            else bucket[c] ++;

        }

        int index = 0;

        for(int i = len; i >= 0; i --) {

            index += bucket[i];

            if(index >= i)

                return i;

        }

        return 0;

    }
```

