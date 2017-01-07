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

# Number of Connected Components in an Undirected Graph

- [LeetCode 323](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/)

Given n nodes labeled from 0 to n - 1 and a list of undirected edges (each edge is a pair of nodes), write a function to find the number of connected components in an undirected graph.

## Example

```
	 0          3
     |          |
     1 --- 2    4
Given n = 5 and edges = [[0, 1], [1, 2], [3, 4]], return 2.

     0           4
     |           |
     1 --- 2 --- 3
Given n = 5 and edges = [[0, 1], [1, 2], [2, 3], [3, 4]], return 1.

```

## Solution with straitforward dfs

```
    public int countComponents(int n, int[][] edges) {
        boolean[] visited = new boolean[n];
        List<Integer>[] adjs = new List[n];
        for(int i = 0; i < n; i ++)
            adjs[i] = new ArrayList();
        for(int[] edge: edges) {
            if(!adjs[edge[0]].contains(edge[1]))
                adjs[edge[0]].add(edge[1]);
            if(!adjs[edge[1]].contains(edge[0]))
                adjs[edge[1]].add(edge[0]);
        }
        int count = 0;
        for(int i = 0; i < n; i ++) {
            if(!visited[i]) {
                dfs(adjs, visited, i);
                count ++;
            }
        }
        return count;
    }
    
    public void dfs(List<Integer>[] adjs, boolean[] visited, int cur) {
        
        visited[cur] = true;
        for(int neighbor: adjs[cur])
            if(!visited[neighbor])
                dfs(adjs, visited, neighbor);
    }

```

## Solution with Union-Find

```
    public int countComponents(int n, int[][] edges) {
        int[] root = new int[n];
        for(int i = 0; i < n; i ++) {
            root[i] = i;
        }
        for(int[] edge: edges) {
            int i = find(root, edge[0]);
            int j = find(root, edge[1]);
            if(i != j) {
                root[i] = j;
                n --;
            }
        }
        return n;
    }
    
    public int find(int[] root, int p) {
        if(root[p] == p)
            return p;
        root[p] = find(root, root[p]);
        return root[p];
    }

```

# Number of Islands

- [LeetCode](https://leetcode.com/problems/number-of-islands/)

Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

## Example

```
11110
11010
11000
00000

Answer: 1

11000
11000
00100
00011

Answer: 3

```

## DFS solution

```
    public int numIslands(char[][] grid) {
        int count = 0;
        for(int i = 0; i < grid.length; i ++) {
            for(int j = 0; j < grid[0].length; j ++) {
                if(grid[i][j] == '1') {
                    dfs(grid, i, j);
                    count ++;
                }
            }
        }
        return count;
    }
    
    public void dfs(char[][] grid, int i, int j) {
        if(i < 0 || j < 0 || i >= grid.length || j >= grid[0].length || grid[i][j] != '1' ) 
            return;
        grid[i][j] = '0';
        dfs(grid, i-1, j);
        dfs(grid, i+1, j);
        dfs(grid, i, j-1);
        dfs(grid, i, j+1);
    }

``` 

## Union-Find solution

2d version of the above connected components problem.

```
    public int numIslands(char[][] grid) {
        if(grid == null || grid.length == 0 || grid[0].length == 0)
            return 0;
        int count = 0;
        int m = grid.length, n = grid[0].length;
        int[] root = new int[m * n];
        for(int i = 0; i < m; i ++) {
            for(int j = 0; j < n; j ++) {
                if(grid[i][j] == '1') {
                    count ++;
                }
                int index = i * n + j;
                root[index] = index;
            }
        }
        
        for(int i = 0; i < m; i ++) {
            for(int j = 0; j < n; j ++) {
                if(grid[i][j] == '0') continue;
                int index = i * n + j;
                if(i > 0 && grid[i-1][j] == '1') {
                    int root1 = find(root, index);
                    int root2 = find(root, index-n);
                    if(root1 != root2) {
                        root[root1] = root2;
                        count --;
                    }
                }
                if(j > 0 && grid[i][j-1] == '1') {
                    int root1 = find(root, index);
                    int root2 = find(root, index-1);
                    if(root1 != root2) {
                        root[root1] = root2;
                        count --;
                    }
                }
            }
        }
        return count;
    }
    
    public int find(int[] root, int p) {
        if(root[p] == p) 
            return p;
        root[p] = find(root, root[p]);
        return root[p];
    }

```

