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
}EEEEEEE
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

# Minimum Unique Word Abbreviation (Hard)

Given a target string and a set of strings in a dictionary, find an abbreviation of this target string with the smallest possible length such that it does not conflict with abbreviations of the strings in the dictionary.

Each number or letter in the abbreviation is considered length = 1. For example, the abbreviation "a32bc" has length = 4.

## Example

```
"apple", ["blade"] -> "a4" (because "5" or "4e" conflicts with "blade")

"apple", ["plain", "amber", "blade"] -> "1p3" (other valid answers include "ap3", "a3e", "2p2", "3le", "3l1").

```

## Solution

Bit manipulation + DFS

The key idea of my solution is to preprocess the dictionary to transfer all the words to bit sequences (int):

Pick the words with same length as target string from the dictionary and compare the characters with target. If the characters are different, set the corresponding bit to 1, otherwise, set to 0.

Ex: "abcde", ["abxdx", "xbcdx"] => [00101, 10001]

The problem is now converted to find a bit mask that can represent the shortest abbreviation, so that for all the bit sequences in dictionary, mask & bit sequence > 0.

Ex: for [00101, 10001], the mask should be [00001]. if we mask the target string with it, we get "4e", which is the abbreviation we are looking for.

To find the bit mask, we need to perform DFS with some optimizations. But which bits should be checked? We can perform "or" operation for all the bit sequences in the dictionary and do DFS for the "1" bits in the result.

Ex: 00101 | 10001 = 10101, so we only need to take care of the 1st, 3rd, and 5th bit.

```
    int cand;  // candidate bits in mask that can be set to 1
    int n, bn;  
    int minLen, minAbbr;
    List<Integer> dict = new ArrayList(); //store reversed bit representation of words
    
    public String minAbbreviation(String target, String[] dictionary) {
        n = target.length();
        bn = 1 << n;
        minLen = Integer.MAX_VALUE;
        buildBitDict(target, dictionary);
        
        dfs(1, 0);
        
        //Reconstruct from minAbbr (minAbbr is a reversed representation of abbr) 
        int count = 0;
        StringBuilder res = new StringBuilder();
        for(int i = 0; i < n; i ++) {
            if(((1 << i) & minAbbr) == 0) 
                count ++;
            else {
                if(count > 0)
                    res.append(count);
                res.append(target.charAt(i));
                count = 0;
            }
        }
        if(count > 0) res.append(count);
        return res.toString();
        
    }
    
    public void dfs(int bit, int mask) {
        int len = abbrLen(mask);
        if(len >= minLen) return;
        boolean match = true;
        for(int word: dict) {
            if((word & mask) == 0){
                match = false;
                break;
            }
        }
        if(match) {
            minLen = len;
            minAbbr = mask;
        } else {
            for(int b = bit; b < bn; b <<= 1) {
                if((b & cand) != 0)
                    dfs(b << 1, mask + b);
            }
        }
    }
    
    public int abbrLen(int mask) {
        int count = n;
        for(int b = 3; b < bn; b <<= 1) {
            if((b & mask) == 0)
                count --;
        }
        return count;
    }
    
    public void buildBitDict(String target, String[] dictionary) {
        cand = 0;
        for(String s: dictionary) {
            if(s.length() != n) continue;
            int word = 0;
            for(int i = 0; i < n; i ++) {
                if(s.charAt(i) != target.charAt(i))
                    word |= 1 << i;
            }
            dict.add(word);
            cand |= word;
        }        
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

# Palindrome Permutation

Given a string, determine if a permutation of the string could form a palindrome.

For example,
"code" -> False, "aab" -> True, "carerac" -> True.

## Solution

Use a set.

```
    public boolean canPermutePalindrome(String s) {
        Set<Character> set = new HashSet();
        for(int i = 0; i < s.length(); i ++) {
            if(!set.add(s.charAt(i)))
                set.remove(s.charAt(i));
        }
        return set.size() == 0 || set.size() == 1;
    }

```

# Palindrome Permutation II

Given a string s, return all the palindromic permutations (without duplicates) of it. Return an empty list if no palindromic permutation could be form.

For example:

Given s = "aabb", return ["abba", "baab"].

Given s = "abc", return [].


## Solution

```
    public List<String> generatePalindromes(String s) {
        List<String> result = new ArrayList();
        if(s == null || s.length() == 0)
            return result;
        Set<Character> set = new HashSet();
        List<Character> sb = new ArrayList();
        for(int i = 0; i < s.length(); i ++) {
            if(!set.add(s.charAt(i))) {
                set.remove(s.charAt(i));
                sb.add(s.charAt(i));
            }
        }
        System.out.println(sb.toString());
        if(set.size() != 0 && set.size() != 1)
            return result;
        String mid = set.size() == 1 ? set.iterator().next().toString() : "";
        Collections.sort(sb);
        helper(result, new StringBuilder(), mid, sb, new boolean[sb.size()]);
        return result;        
    }
    
    public void helper(List<String> result, StringBuilder cur, String mid, List<Character> sb, boolean[] used) {
        if(cur.length() == sb.size()) {
            result.add(cur.toString() + mid + cur.reverse().toString());
            // reverse back is very important!!!
            cur.reverse();
            return;
        }
        for(int i = 0; i < sb.size(); i ++) {
            if(used[i]) continue;
            if(i > 0 && sb.get(i) == sb.get(i-1) && !used[i-1]) continue;
            cur.append(sb.get(i));
            used[i] = true;
            helper(result, cur, mid, sb, used);
            cur.deleteCharAt(cur.length()-1);
            used[i] = false;
        }
    }

```

# One Edit Distance

Given two strings S and T, determine if they are both one edit distance apart.

## Solution

```

//  There're 3 possibilities to satisfy one edit distance apart: 
  
//  1) Replace 1 char:
// 	  s: a B c
// 	  t: a D c
//  2) Delete 1 char from s: 
//	  s: a D  b c
//	  t: a    b c
//  3) Delete 1 char from t
//	  s: a   b c
//	  t: a D b c
 

    public boolean isOneEditDistance(String s, String t) {
        if(Math.abs(s.length() - t.length()) > 1)
            return false;
        for(int i = 0; i < Math.min(s.length(), t.length()); i ++) {
            if(s.charAt(i) != t.charAt(i)) {
                if(s.length() == t.length()) 
                    return s.substring(i+1).equals(t.substring(i+1));
                else if(s.length() > t.length())
                    return s.substring(i+1).equals(t.substring(i));
                else 
                    return s.substring(i).equals(t.substring(i+1));
            }
        }
		//All previous chars are the same, the only possibility is deleting the end char in the longer one of s and t 
        return Math.abs(s.length() - t.length()) == 1;
    }
```

# Line Reflection

Given n points on a 2D plane, find if there is such a line parallel to y-axis that reflect the given points.

Example 1:
Given points = [[1,1],[-1,1]], return true.

Example 2:
Given points = [[1,1],[-1,-1]], return false.

## Solution

1. Find the smallest and largest x-value for all points.
2. If there is a line then it should be at y = (minX + maxX) / 2.
3. For each point, make sure that it has a reflected point in the opposite side.

I used HashMap because I didn't consider the case that several points can share the same x-index.

For example: [[1,2],[2,2],[1,4],[2,4]]

```
    public boolean isReflected(int[][] points) {
        HashSet<Integer> set = new HashSet();
        int min = Integer.MAX_VALUE, max = Integer.MIN_VALUE;
        for(int[] point: points) {
            set.add(Arrays.hashCode(point));
            min = Math.min(min, point[0]);
            max = Math.max(max, point[0]);
        }
        int sum = max + min;
        for(int[] point: points) {
            if(!set.contains(Arrays.hashCode(new int[]{sum - point[0], point[1]})))
                return false;
        }
        return true;
    }

```

# Largest BST Subtree

Given a binary tree, find the largest subtree which is a Binary Search Tree (BST), where largest means subtree with largest number of nodes in it.

## Solution

Similar to LeetCode 98. Validate Binary Search Tree 

```
    int max = 0;
    
    class TreeNodeBound {
        int size;
        int left;
        int right;
        
        public TreeNodeBound(int size, int left, int right) {
            this.size = size;
            this.left = left;
            this.right = right;
        }
    }
    
    public int largestBSTSubtree(TreeNode root) {
        dfs(root);
        return max;
    }
    
    public TreeNodeBound dfs(TreeNode root) {
        if(root == null) return new TreeNodeBound(0, Integer.MAX_VALUE, Integer.MIN_VALUE);
        TreeNodeBound left = dfs(root.left);
        TreeNodeBound right = dfs(root.right);
        if(left.size == -1 || right.size == -1 || left.right >= root.val || right.left <= root.val)
            return new TreeNodeBound(-1, 0, 0);
        int size = left.size + right.size + 1;
        max = Math.max(max, size);
        return new TreeNodeBound(size, Math.min(left.left, root.val), Math.max(right.right, root.val));
    }

```

# Sentence Screen Fitting

Given a rows x cols screen and a sentence represented by a list of non-empty words, find how many times the given sentence can be fitted on the screen.

Note:

1. A word cannot be split into two lines.
2. The order of words in the sentence must remain unchanged.
3. Two consecutive words in a line must be separated by a single space.
4. Total words in the sentence won't exceed 100.
5. Length of each word is greater than 0 and won't exceed 10.
6. 1 ≤ rows, cols ≤ 20,000.

## Solution

I used brute force which exceeds time limit. We can use a dp like solution which keep track of the row  starting with every index of the sentence.


```
    public int wordsTyping(String[] sentence, int rows, int cols) {
        int[] nextIdx = new int[sentence.length];
        int[] times = new int[sentence.length];
        for(int i = 0; i < sentence.length; i ++) {
            int cur = i;
            int j = 0;
            while(j + sentence[cur].length() <= cols) {
                j += sentence[cur].length() + 1;
                cur ++;
                if(cur == sentence.length){
                    cur = 0;
                    times[i] ++;
                }
            }
            nextIdx[i] = cur;
        }
        int count = 0;
        int index = 0;
        for(int i = 0; i < rows; i ++) {
            count += times[index];
            index = nextIdx[index];
        }

        return count;
    }

```
# Encode and Decode Strings

Design an algorithm to encode a list of strings to a string. The encoded string is then sent over the network and is decoded back to the original list of strings.

## Solution

```
    // Encodes a list of strings to a single string.
    public String encode(List<String> strs) {
        StringBuilder sb = new StringBuilder();
        for(String str: strs) {
            sb.append(str.length() + "/" + str);
        }
        return sb.toString();
    }

    // Decodes a single string to a list of strings.
    public List<String> decode(String s) {
        List<String> result = new ArrayList();
        int index = 0;
        while(index < s.length()) {
            int slash = s.indexOf("/", index);
            int len = Integer.valueOf(s.substring(index, slash));
            result.add(s.substring(slash + 1, slash + len + 1));
            index = slash + len + 1;
        }
        return result;
    }

```

# Sequence Reconstruction

Check whether the original sequence org can be uniquely reconstructed from the sequences in seqs. The org sequence is a permutation of the integers from 1 to n, with 1 ≤ n ≤ 104. Reconstruction means building a shortest common supersequence of the sequences in seqs (i.e., a shortest sequence so that all sequences in seqs are subsequences of it). Determine whether there is only one sequence that can be reconstructed from seqs and it is the org sequence.

## Example

```
Input:
org: [1,2,3], seqs: [[1,2],[1,3]]

Output:
false

Explanation:
[1,2,3] is not the only one sequence that can be reconstructed, because [1,3,2] is also a valid sequence that can be reconstructed.

Input:
org: [1,2,3], seqs: [[1,2]]

Output:
false

Explanation:
The reconstructed sequence can only be [1,2].

Input:
org: [1,2,3], seqs: [[1,2],[1,3],[2,3]]

Output:
true

Explanation:
The sequences [1,2], [1,3], and [2,3] can uniquely reconstruct the original sequence [1,2,3].

Input:
org: [4,1,5,2,6,3], seqs: [[5,2,6,3],[4,1,5,2]]

Output:
true

```

## Solution

BFS topological sort

```
    public boolean sequenceReconstruction(int[] org, List<List<Integer>> seqs) {
        Map<Integer, Set<Integer>> adjs = new HashMap();
        Map<Integer, Integer> inDegree = new HashMap();
        
        for(List<Integer> seq: seqs) {
            if(seq.size() == 1) {
                if(!adjs.containsKey(seq.get(0))) {
                    adjs.put(seq.get(0), new HashSet());
                    inDegree.put(seq.get(0), 0);
                }
            } 
            for(int i = 0; i < seq.size()-1; i ++) {
                if(!adjs.containsKey(seq.get(i))) {
                    adjs.put(seq.get(i), new HashSet());
                    inDegree.put(seq.get(i), 0);
                }
                if(!adjs.containsKey(seq.get(i+1))) {
                    adjs.put(seq.get(i+1), new HashSet());
                    inDegree.put(seq.get(i+1), 0);
                }
                if(adjs.get(seq.get(i)).add(seq.get(i+1)))
                    inDegree.put(seq.get(i+1), inDegree.get(seq.get(i+1))+1);
            }
        }
        
        Queue<Integer> queue = new LinkedList();
        for(Map.Entry<Integer, Integer> entry: inDegree.entrySet()) {
            if(entry.getValue() == 0)
                queue.offer(entry.getKey());
        }
        
        int index = 0;
        
        while(!queue.isEmpty()) {
            if(queue.size() > 1) return false;
            int cur = queue.poll();
            if(index == org.length || org[index ++] != cur)
                return false;
            for(int next: adjs.get(cur)) {
                inDegree.put(next, inDegree.get(next)-1);
                if(inDegree.get(next) == 0)
                    queue.offer(next);
            }
        }
        return org.length == index && index == adjs.size();
    }

```

# Strobogrammatic Number

A strobogrammatic number is a number that looks the same when rotated 180 degrees (looked at upside down).

Write a function to determine if a number is strobogrammatic. The number is represented as a string.

For example, the numbers "69", "88", and "818" are all strobogrammatic.

## Solution

```
    public boolean isStrobogrammatic(String num) {
        for(int i = 0, j = num.length()-1; i <= j; i++, j--) {
            if(!"00 11 88 696".contains(num.charAt(i) + "" + num.charAt(j)))
                return false;
        }
        return true;
    }

```

# Strobogrammatic Number II

Find all strobogrammatic numbers that are of length = n.

For example,

Given n = 2, return ["11","69","88","96"].


## Solution

```
    int[][] map = {{0,0},{1,1},{8,8},{6,9},{9,6}};
    
    public List<String> findStrobogrammatic(int n) {
        List<String> result = new ArrayList();
        if(n == 0) return result;
        if(n == 1) {
            result.add("0");
            result.add("1");
            result.add("8");
            return result;
        }
        if(n % 2 == 0)
            helper(result, "", n);
        else {
            helper(result, "0", n - 1);
            helper(result, "1", n - 1);
            helper(result, "8", n - 1);
        }
        return result;
    }
    
    public void helper(List<String> result, String cur, int n) {
        if(n == 0) {
            if(cur.charAt(0) != '0')
                result.add(cur);
            return;
        }
        int len = cur.length();
        for(int[] num: map) {
            cur = num[0] + cur + num[1];
            helper(result, cur, n - 2);
            cur = cur.substring(1, len+1);
        }
    }

```

# Strobogrammatic Number III

Write a function to count the total strobogrammatic numbers that exist in the range of low <= num <= high.

For example,

Given low = "50", high = "100", return 3. Because 69, 88, and 96 are three strobogrammatic numbers.

## Solution

```
    int[][] map = {{0,0}, {1,1}, {8,8}, {6,9}, {9,6}};
    int count = 0;
    
    public int strobogrammaticInRange(String low, String high) {
        int min = low.length(), max = high.length();
        for(int n = min; n <= max; n ++) {
            if(n % 2 == 0)
                helper("", low, high, n);
            else {
                helper("0", low, high, n-1);
                helper("1", low, high, n-1);
                helper("8", low, high, n-1);
            }
        }
        return count;
    }
    
    public void helper(String cur, String low, String high, int n) {
        int len = cur.length();
        if(n == 0) {
            if(len > 1 && cur.charAt(0) == '0') return;
            if((len == low.length() && cur.compareTo(low) < 0) ||
                (len == high.length() && cur.compareTo(high) > 0))
                return;
            count ++;
            return;
        }
        for(int[] num: map) {
            cur = num[0] + cur + num[1];
            helper(cur, low, high, n-2);
            cur = cur.substring(1, len+1);
        }
    }

```

# Read N Characters Given Read4

The API: int read4(char *buf) reads 4 characters at a time from a file.

The return value is the actual number of characters read. For example, it returns 3 if there is only 3 characters left in the file.

By using the read4 API, implement the function int read(char *buf, int n) that reads n characters from the file.

## Solution

```
    public int read(char[] buf, int n) {
        char[] temp = new char[4];
        int count = 0;
        boolean eof = false;
        while(count < n && !eof) {
            int cur = read4(temp);
            eof = cur < 4;
            int len = Math.min(cur, n - count);
            for(int i = 0; i < len; i ++) {
                buf[count ++] = temp[i];
            }
        }
        return count;
    }

```

# Read N Characters Given Read4 II - Call multiple times

The read function may be called multiple times.

## Solution

```
     int index = 0;
     int count = 0;
     char[] next = new char[4];
     
    public int read(char[] buf, int n) {
        int p = 0;
        while(p < n) {
            if(index == 0)
                count = read4(next);
            if(count == 0) break;
            while(p < n && index < count)
                buf[p ++] = next[index ++];
            if(index == count) index = 0;
        }
        return p;
    }
```


# Group Shifted Strings

Given a string, we can "shift" each of its letter to its successive letter, for example: "abc" -> "bcd". We can keep "shifting" which forms the sequence:

"abc" -> "bcd" -> ... -> "xyz"

Given a list of strings which contains only lowercase alphabets, group all strings that belong to the same shifting sequence.

## Example 

```
["abc", "bcd", "acef", "xyz", "az", "ba", "a", "z"]

[
  ["abc","bcd","xyz"],
  ["az","ba"],
  ["acef"],
  ["a","z"]
]
Show Company Tags
Show Tags
Show Similar Problems


```

## Solution

```
    public List<List<String>> groupStrings(String[] strings) {
        Map<String, List<String>> map = new HashMap();
        Arrays.sort(strings);
        for(String cur: strings) {
            int offset = cur.charAt(0) - 'a';
            String key = "";
            for(int i = 0; i < cur.length(); i ++) {
                char c = (char)(cur.charAt(i) - offset);
                if(c < 'a')
                    c += 26;
                key += c;
            }
            if(!map.containsKey(key))
                map.put(key, new ArrayList());
            map.get(key).add(cur);
        }
        return new ArrayList(map.values());
    }
```


