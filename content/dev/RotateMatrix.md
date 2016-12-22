+++
Categories = ["Algorithm"]
Description = ""
Tags = ["LeetCode", "Trick"]
date = "2016-12-22T13:15:37-05:00"
menu = "main"
title = "RotateMatrix"

+++

# Rotate Image

You are given an n x n 2D matrix representing an image.

Rotate the image by 90 degrees (clockwise).

## Trick
I did the rotation in a dummy way. There is a trick:

The idea was firstly transpose the matrix and then flip it symmetrically.

## Sample
Transpose: swap(matrix[i][j], matrix[j][i])
```
1  2  3             
4  5  6
7  8  9
```

Then flip the matrix horizontally. (swap(matrix[i][j], matrix[i][matrix.length-1-j])

```
7  4  1
8  5  2
9  6  3

```

## Solution

```
    public void rotate(int[][] matrix) {
        if(matrix == null || matrix.length == 0 || matrix[0].length == 0)
            return;
        int len = matrix.length;
        for(int i = 0; i < len; i ++) {
            for(int j = i + 1; j < len; j ++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }
        for(int i = 0; i < len; i ++) {
            for(int j = 0; j < len/2; j ++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[i][len - j - 1];
                matrix[i][len - j - 1] = temp;
            }
        }
    }
```


