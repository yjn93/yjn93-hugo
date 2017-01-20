+++
Categories = ["Algorithm"]
Description = ""
Tags = ["LeetCode", "BIT", "Segment Tree"]
date = "2017-01-20T00:15:59-05:00"
menu = "main"
title = "Binary Indexed Trees"

+++

# BIT

Binary Indexed Trees is often used for storing frequencies and manipulating cumulative frequency tables.

- [TopCoder explanation](https://www.topcoder.com/community/data-science/data-science-tutorials/binary-indexed-trees/)


## Init tree

```
    void init(int[] nums) {
        maxIdx = nums.length;
        //The index of BIT starts from 1
        BIT = new int[maxIdx + 1];
        for(int i = 0; i < maxIdx; i ++)
            updateBIT(i+1, nums[i]);
    }

```

## Read

```
    int readBIT(int idx) {
        int sum = 0;
        while(idx > 0) {
            sum += BIT[idx];
            idx -= (idx & -idx);
        }
        return sum;
    }
```

## Update

```
    void updateBIT(int idx, int diff) {
        while(idx <= maxIdx) {
            BIT[idx] += diff;
            idx += (idx & -idx);
        } 
    }
```


# Range Sum Query - Mutable

Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.

The update(i, val) function modifies nums by updating the element at index i to val.

## Example

```
Given nums = [1, 3, 5]

sumRange(0, 2) -> 9
update(1, 2)
sumRange(0, 2) -> 8

```

## Solution - Segment Tree

Segement Tree is like a complete binary tree. Each node represent sum of some range (from a start index to an end index). The children of one node has a subrange of the range.

```
    segmentNode root;
    int[] nums;

    public NumArray(int[] nums) {
        this.nums = nums;
        if(nums.length != 0)
        root = buildTree(nums, 0, nums.length-1);
    }

    segmentNode buildTree(int[] nums, int start, int end) {
        segmentNode root = new segmentNode(start, end);
        if(start == end)
            root.sum = nums[start];
        else {
            int mid = (end-start)/2 + start;
            root.left = buildTree(nums, start, mid);
            root.right = buildTree(nums, mid+1, end);
            root.sum = root.left.sum + root.right.sum;
        }
        return root;
    }
    
    void update(int i, int val) {
        int diff = nums[i] - val;
        nums[i] = val;
        updateNode(root, i, diff);
    }
    
    void updateNode(segmentNode root, int i, int diff) {
        if(root == null || root.start > i || root.end < i) return;
        root.sum -= diff;
        updateNode(root.left, i, diff);
        updateNode(root.right, i, diff);
    }

    public int sumRange(int i, int j) {
        return sumRange(root, i, j);
    }
    
    int sumRange(segmentNode root, int i, int j) {
        if(root == null || root.start > j || root.end < i) return 0;
        if(root.start >= i && root.end <= j) return root.sum;
        return sumRange(root.left, i, j) + sumRange(root.right, i, j);
    }
    
}

class segmentNode {
    int start;
    int end;
    int sum;
    segmentNode left;
    segmentNode right;
    
    segmentNode(int start, int end) {
        this.start = start;
        this.end = end;
        this.sum = 0;
        left = null;
        right = null;
    }
}

```

## Solution - BIT

```
    int[] nums;
    int[] BIT;
    int maxIdx;

    public NumArray(int[] nums) {
        this.nums = nums;
        init(nums);
    }

    void init(int[] nums) {
        maxIdx = nums.length;
        //The index of BIT starts from 1
        BIT = new int[maxIdx + 1];
        for(int i = 0; i < maxIdx; i ++)
            updateBIT(i+1, nums[i]);
    }
    
    void updateBIT(int idx, int diff) {
        while(idx <= maxIdx) {
            BIT[idx] += diff;
            idx += (idx & -idx);
        } 
    }
    
    void update(int i, int val) {
        int diff = val - nums[i];
        nums[i] = val;
        updateBIT(i+1, diff);
    }
    
    int readBIT(int idx) {
        int sum = 0;
        while(idx > 0) {
            sum += BIT[idx];
            idx -= (idx & -idx);
        }
        return sum;
    }
    
    public int sumRange(int i, int j) {
        return readBIT(j+1) - readBIT(i);
    }

```

# Range Sum Query 2D - Mutable

Given a 2D matrix matrix, find the sum of the elements inside the rectangle defined by its upper left corner (row1, col1) and lower right corner (row2, col2).

## Example

```
Given matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]

sumRegion(2, 1, 4, 3) -> 8
update(3, 2, 2)
sumRegion(2, 1, 4, 3) -> 10

```

## Solution

```
    int[][] BIT;
    int[][] nums;
    int m;
    int n;

    public NumMatrix(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return;
        m = matrix.length;
        n = matrix[0].length;
        nums = matrix;
        BIT = new int[m+1][n+1];
        for(int i = 0; i < m; i ++) {
            for(int j = 0; j < n; j ++) {
                updateBIT(i+1, j+1, matrix[i][j]);
            }
        }
    }

    public void update(int row, int col, int val) {
        int diff = val - nums[row][col];
        nums[row][col] = val;
        updateBIT(row+1, col+1, diff);
    }
    
    public void updateBIT(int x, int y, int diff) {
        for(int i = x; i <= m; i += (i & -i))  {
            for(int j = y; j <= n; j += (j & -j)) {
                BIT[i][j] += diff;
            }
        }
    }
    
    public int readBIT(int x, int y) {
        int sum = 0;
        for(int i = x; i > 0; i -=(i & -i)) {
            for(int j = y; j > 0; j -= (j & -j)) {
                sum += BIT[i][j];
            }
        }
        return sum;
    }

    public int sumRegion(int row1, int col1, int row2, int col2) {
        return readBIT(row2+1, col2+1) - readBIT(row2+1, col1) - readBIT(row1, col2+1) + readBIT(row1, col1);
    }

```

