+++
Categories = ["Algorithm"]
Description = ""
Tags = ["LeetCode", "Union Find"]
date = "2017-01-03T00:05:36-05:00"
menu = "main"
title = "Union Find for board connection"

+++

# Union Find

This data structure can be used to solve dynamic connectivity problem. Usually, if we care about the path between two connected node, we need to use dfs. However, for checking the connectivity itself, union find can work. Also, dfs is suitable for static graph while union find is better for changing graph.

## Initialization
```
int[] unionSet;
// Initialize the union root of every element to be itself.
unionSet[i] = i;

// Sometime use another array to record the size of the union for the purpose of weighted union
int[] size = new int[unionSet.length];

```

## Weighted Union
```
    private void union(int p, int q) {
        int rootP = find(p);
        int rootQ = find(q);
		if(size[rootP] < size[rootQ]) {
        	unionSet[rootP] = rootQ;
			size[rootQ] += size[rootP];
		} else {
			unionSet[rootQ] = rootP;
			size[rootP] += size[rootQ];
		}
    }

```

## Find with path compression
Two ways: recursion and loop

```
// recursion: set the height of the union to be 1
    private int find(int p) {
        if(unionSet[p] == p)
            return p;
        unionSet[p] = find(unionSet[p]);
        return unionSet[p];
    }

// loop: set the parent of a node to be its grandparent
    private int find(int p) {
        while(unionSet[p] != p){
           	unionSet[p] = unionSet[unionSet[p]];
			p = unionSet[p];
		}
        return p;
    }
```

# Surrounded Regions
- [LeetCode 130](https://leetcode.com/problems/surrounded-regions/)

Given a 2D board containing 'X' and 'O' (the letter O), capture all regions surrounded by 'X'.

A region is captured by flipping all 'O's into 'X's in that surrounded region.

## Example
```
X X X X
X O O X
X X O X
X O X X
```
After running your function, the board should be:
```
X X X X
X X X X
X X X X
X O X X
```

## Solution with union find

There is another solution which first use dfs from the edges to turn every edgeO to '1', then turn every not-transfered 'O' to 'X', then turn all '1' back to 'O'. Here, we give the union find solution.
```
public class Solution {
    int[] unionSet;
    boolean[] hasEdgeO;
    
    public void solve(char[][] board) {
        if(board == null || board.length == 0 || board[0].length == 0)
            return;
        int m = board.length, n = board[0].length;
        unionSet = new int[m * n];
        hasEdgeO = new boolean[unionSet.length];
        for(int i = 0; i < m; i ++) {
            for(int j = 0; j < n; j ++) {
                int index = i * n + j;
                unionSet[index] = index;
                if(board[i][j] == 'X') continue;
                if(i == 0 || j == 0 || i == m-1 || j == n-1)
                    hasEdgeO[index] = true;
                if(i > 0 && board[i-1][j] == 'O')
                    union(index, index-n);
                if(j > 0 && board[i][j-1] == 'O')
                    union(index, index-1);
            }
        }
        for(int i = 1; i < m; i ++) {
            for(int j = 1; j < n; j ++) {
                if(board[i][j] == 'O' && !hasEdgeO[find(i * n + j)])
                    board[i][j] = 'X';
            }   
        }
    }
    
    private int find(int p) {
        if(unionSet[p] == p)
            return p;
        unionSet[p] = find(unionSet[p]);
        return unionSet[p];
    }
    
    private void union(int p, int q) {
        int rootP = find(p);
        int rootQ = find(q);
        unionSet[rootP] = rootQ;
        hasEdgeO[rootQ] = hasEdgeO[rootP] || hasEdgeO[rootQ];
    }
}
```
