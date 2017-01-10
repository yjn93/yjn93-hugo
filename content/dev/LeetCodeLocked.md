+++
Categories = ["Algorithm"]
Description = "Locked problems"
Tags = ["LeetCode"]
date = "2017-01-05T14:34:10-05:00"
menu = "main"
title = "LeetCodeLocked"

+++

# Range Addition

Assume you have an array of length n initialized with all 0's and are given k update operations.

Each operation is represented as a triplet: [startIndex, endIndex, inc] which increments each element of subarray A[startIndex ... endIndex] /(startIndex and endIndex inclusive) with inc.

Return the modified array after all k operations were executed.

## Example

```
Given:

    length = 5,
    updates = [
        [1,  3,  2],
        [2,  4,  3],
        [0,  2, -2]
    ]

Output:

    [-2, 0, 3, 5, 3]
```

## Explanation

```
Initial state:
[ 0, 0, 0, 0, 0 ]

After applying operation [1, 3, 2]:
[ 0, 2, 2, 2, 0 ]

After applying operation [2, 4, 3]:
[ 0, 2, 5, 5, 3 ]

After applying operation [0, 2, -2]:
[-2, 0, 3, 5, 3 ]
```
## Solution
Only update the start and end+1 index every time is sufficient.

```
    public int[] getModifiedArray(int length, int[][] updates) {
        int[] result = new int[length];
        for(int[] update: updates) {
            result[update[0]] += update[2];
            if(update[1] < length-1)
                result[update[1]+1] -= update[2];
        }
        
        for(int i = 1; i < length; i ++)
            result[i] += result[i-1];
        return result;
    }
```

# Shortest Word Distance
Given a list of words and two words word1 and word2, return the shortest distance between these two words in the list.

For example,

Assume that words = ["practice", "makes", "perfect", "coding", "makes"].

Given word1 = “coding”, word2 = “practice”, return 3.

Given word1 = "makes", word2 = "coding", return 1.

## Solution

```
    public int shortestDistance(String[] words, String word1, String word2) {
        int p1 = -1, p2 = -1;
        int min = words.length;
        for(int i = 0; i < words.length; i ++) {
            if(words[i].equals(word1))
                p1 = i;
            if(words[i].equals(word2))
                p2 = i;
            if(p1 != -1 && p2 != -1)
                min = Math.min(Math.abs(p2 - p1), min);
        }
        return min;
    }
```

## Follow up 1
Now you are given the list of words and your method will be called repeatedly many times with different parameters. How would you optimize it?

Design a class which receives a list of words in the constructor, and implements a method that takes two words word1 and word2 and return the shortest distance between these two words in the list.

### Solution
Use HashMap

```
public class WordDistance {
    HashMap<String, List<Integer>> map;
    
    public WordDistance(String[] words) {
        map = new HashMap();
        for(int i = 0; i < words.length; i ++) {
            if(!map.containsKey(words[i]))
                map.put(words[i], new ArrayList());
            map.get(words[i]).add(i);
        }
    }

    public int shortest(String word1, String word2) {
        int min = Integer.MAX_VALUE;
        for(int i: map.get(word1)) {
            for(int j: map.get(word2)) 
                min = Math.min(Math.abs(i - j), min);
        }
        return min;
    }
}
```

## Follow up 2
Now word1 could be the same as word2 and they represent two individual words in the list.

### Solution

If we remove the boolean same variable, it can solve the basic problem.

```
    public int shortestWordDistance(String[] words, String word1, String word2) {
        int index = -1;
        int min = words.length;
        boolean same = word1.equals(word2);
        for(int i = 0; i < words.length; i ++) {
            if(words[i].equals(word1) || words[i].equals(word2)) {
                if(index != -1 && (same || !words[index].equals(words[i]))){
                    min = Math.min(i - index, min);
                }
                index = i;
            }
        }
        return min;
    }

```

# Zigzag iterator

Given two 1d vectors, implement an iterator to return their elements alternately.

For example, given two 1d vectors:

```
v1 = [1, 2]
v2 = [3, 4, 5, 6]
By calling next repeatedly until hasNext returns false, the order of elements returned by next should be:
[1, 3, 2, 4, 5, 6]
```

Follow up: What if you are given k 1d vectors? How well can your code be extended to such cases?

```
[1,2,3]
[4,5,6,7]
[8,9]
It should return [1,4,8,2,5,9,3,6,7].
```

## Solution
Uses a linkedlist to store the iterators in different vectors. Every time we call next(), we pop an element from the list, and re-add it to the end to cycle through the lists.

```

public class ZigzagIterator {
    LinkedList<Iterator> list;
    
    public ZigzagIterator(List<Integer> v1, List<Integer> v2) {
        list = new LinkedList();
        if(!v1.isEmpty())
            list.add(v1.iterator());
        if(!v2.isEmpty())
            list.add(v2.iterator());
    }

    public int next() {
        Iterator cur = list.remove();
        int result = (int)cur.next();
        if(cur.hasNext()) list.add(cur);
        return result;
    }

    public boolean hasNext() {
        return !list.isEmpty();
    }
}

```

Many things need to be notice in code:

- For the member list, we need to specify its type as LinkedList, or Queue. If we use List<Iterator> list, we cannot use remove() or poll() method because they are not specified by interface List, but by Queue or Deque.

- After we got the Iterator cur, we need to convert cur.next() to (int), otherwise it will raise Object cannot convert to int exception.

# Flip Game

You are playing the following Flip Game with your friend: Given a string that contains only these two characters: + and -, you and your friend take turns to flip two consecutive "++" into "--". The game ends when a person can no longer make a move and therefore the other person will be the winner.

Write a function to determine if the starting player can guarantee a win.

For example, given s = "++++", return true. The starting player can guarantee a win by flipping the middle "++" to become "+--+".

## Solution

```
    Map<String, Boolean> winMap = new HashMap();
    
    public boolean canWin(String s) {
        if(winMap.containsKey(s)) return winMap.get(s);
        for(int i = -1; (i = s.indexOf("++", i + 1)) >= 0; ) {
            String t = s.substring(0, i) + "--" + s.substring(i+2);
            if(!canWin(t)) {
                winMap.put(s, true);
                return true;
            }
        }
        winMap.put(s, false);
        return false;
    }

``` 

# Generalized Abbreviation

Write a function to generate the generalized abbreviations of a word.

## Example

Given word = "word", return the following list (order does not matter):

```
["word", "1ord", "w1rd", "wo1d", "wor1", "2rd", "w2d", "wo2", "1o1d", "1or1", "w1r1", "1o2", "2r1", "3d", "w3", "4"]

```

## Solution

For each character, we can choose to put or not put it in the abbreviation. So we can use backtracking

```

    public List<String> generateAbbreviations(String word) {
        List<String> result = new ArrayList();
        dfs(result, new StringBuilder(), word, 0, 0);
        return result;
    }
    
    public void dfs(List<String> result, StringBuilder sb, String word, int pos, int count) {
        int len = sb.length();
        if(pos == word.length()) {
            if(count > 0) sb.append(count);
            result.add(sb.toString());
        } else {
            dfs(result, sb, word, pos+1, count+1);
            if(count > 0)
                sb.append(count);
            dfs(result, sb.append(word.charAt(pos)), word, pos+1, 0);
        }
        sb.setLength(len);
    }

```

# Maximum Size Subarray Sum Equals k

Given an array nums and a target value k, find the maximum length of a subarray that sums to k. If there isn't one, return 0 instead.

```

Example 1:
Given nums = [1, -1, 5, -2, 3], k = 3,
return 4. (because the subarray [1, -1, 5, -2] sums to 3 and is the longest)

Example 2:
Given nums = [-2, -1, 2, 1], k = 1,
return 2. (because the subarray [-1, 2] sums to 1 and is the longest

```
## Solution

I didn't come up with the idea of using map.

```
    public int maxSubArrayLen(int[] nums, int k) {
        int max = 0, sum = 0;
        HashMap<Integer, Integer> map = new HashMap();
        for(int i = 0; i < nums.length; i ++) {
            sum += nums[i];
            if(sum == k) 
                max = i + 1;
            else if(map.containsKey(sum - k))
                max = Math.max(max, i - map.get(sum - k));
            if(!map.containsKey(sum))
                map.put(sum, i);
        }
        return max;
    }

```

# Factor Combinations

Numbers can be regarded as product of its factors. Write a function that takes an integer n and return all possible combinations of its factors,

## Example

```
input: 1
output: []

input: 37
output: []

input: 12
output:
[
  [2, 6],
  [2, 2, 3],
  [3, 4]
]

input: 32
output:
[
  [2, 16],
  [2, 2, 8],
  [2, 2, 2, 4],
  [2, 2, 2, 2, 2],
  [2, 4, 4],
  [4, 8]
]

```

## Solution

Backtracking, similar to combination sum.

```
    public List<List<Integer>> getFactors(int n) {
        List<List<Integer>> result = new ArrayList();
        helper(result, new ArrayList(), n, 2);
        return result;
    }
    
    public void helper(List<List<Integer>> result, List<Integer> cur, int n, int start) {
        for(int i = start; i <= n / i; i ++) {
            if(n % i == 0) {
                cur.add(i);
                cur.add(n/i);
                result.add(new ArrayList(cur));
                cur.remove(cur.size()-1);
                helper(result, cur, n/i, i);
                cur.remove(cur.size()-1);
            }
        }
    }

```

# Bomb Enemy

Given a 2D grid, each cell is either a wall 'W', an enemy 'E' or empty '0' (the number zero), return the maximum enemies you can kill using one bomb.

The bomb kills all the enemies in the same row and column from the planted point until it hits the wall since the wall is too strong to be destroyed.

Note that you can only put the bomb at an empty cell.

## Example

```
For the given grid

0 E 0 0
E 0 W E
0 E 0 0

return 3. (Placing a bomb at (1,1) kills 3 enemies)
```

## Solution

Using dp, only need to record number of enemies in current row and every column. If encounter a wall, update the values.

```
    public int maxKilledEnemies(char[][] grid) {
        if(grid == null || grid.length == 0 || grid[0].length == 0)
            return 0;
        int m = grid.length, n = grid[0].length;
        int row = 0;
        int[] col = new int[n];
        int max = 0;
        for(int i = 0; i < m; i ++) {
            for(int j = 0; j < n; j ++) {
                if(grid[i][j] == 'W') continue;
                if(j == 0 || grid[i][j-1] == 'W') {
                    row = 0;
                    for(int k = j; k < n && grid[i][k] != 'W'; k ++)
                        row += grid[i][k] == 'E' ? 1 : 0;
                }
                if(i == 0 || grid[i-1][j] == 'W') {
                    col[j] = 0;
                    for(int k = i; k < m && grid[k][j] != 'W'; k ++)
                        col[j] += grid[k][j] == 'E' ? 1 : 0;
                }
                if(grid[i][j] == '0')
                    max = Math.max(row + col[j], max);
            }
        }
        return max;
    }

```

# Inorder Successor in BST

Given a binary search tree and a node in it, find the in-order successor of that node in the BST.

Note: If the given node has no in-order successor in the tree, return null.

## Solution

```
	//Iterative
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        TreeNode cur = root;
        TreeNode result = null;
        while(cur != null) {
            if(cur.val > p.val) {
                result = cur;
                cur = cur.left;
            } else {
                cur = cur.right;
            }
        }
        return result;
    }

	// Recursive
	public TreeNode successor(TreeNode root, TreeNode p) {
  		if (root == null)
    	return null;

  		if (root.val <= p.val) {
    		return successor(root.right, p);
  		} else {
    		TreeNode left = successor(root.left, p);
    		return (left != null) ? left : root;
  		}
	}	
```

# Find the Celebrity

Suppose you are at a party with n people (labeled from 0 to n - 1) and among them, there may exist one celebrity. The definition of a celebrity is that all the other n - 1 people know him/her but he/she does not know any of them.

Now you want to find out who the celebrity is or verify that there is not one. The only thing you are allowed to do is to ask questions like: "Hi, A. Do you know B?" to get information of whether A knows B. You need to find out the celebrity (or verify there is not one) by asking as few questions as possible (in the asymptotic sense).

You are given a helper function bool knows(a, b) which tells you whether A knows B. Implement a function int findCelebrity(n), your function should minimize the number of calls to knows.

Note: There will be exactly one celebrity if he/she is in the party. Return the celebrity's label if there is a celebrity in the party. If there is no celebrity, return -1.

## Solution

```
    public int findCelebrity(int n) {
        if(n <= 1) return n-1;
        int cur = 0;
        for(int i = 1; i < n; i ++) {
            if(knows(cur, i)) {
                    cur = i;
            }
        }
        for(int i = 0; i < n; i ++) {
            if(i != cur && (knows(cur, i) || !knows(i, cur)))
                return -1;
        }
        return cur;
    }

```


