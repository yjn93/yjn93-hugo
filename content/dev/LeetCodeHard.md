+++
Categories = ["Algorithm"]
Description = ""
Tags = ["LeetCode"]
date = "2017-01-13T10:44:08-05:00"
menu = "main"
title = "LeetCodeHard"

+++

# Best Meeting Point

A group of two or more people wants to meet and minimize the total travel distance. You are given a 2D grid of values 0 or 1, where each 1 marks the home of someone in the group. The distance is calculated using Manhattan Distance, where distance(p1, p2) = |p2.x - p1.x| + |p2.y - p1.y|.

For example, given three people living at (0,0), (0,4), and (2,2)

```
1 - 0 - 0 - 0 - 1
|   |   |   |   |
0 - 0 - 0 - 0 - 0
|   |   |   |   |
0 - 0 - 1 - 0 - 0

```
The point (0,2) is an ideal meeting point, as the total travel distance of 2+2+2=6 is minimal. So return 6.

## Solution

Try to solve it in one dimension first. Similar to the problem *Minimum Moves to Equal Array Elements*

```
    public int minTotalDistance(int[][] grid) {
        if(grid == null || grid.length == 0 || grid[0].length == 0)
            return 0;
        int m = grid.length, n = grid[0].length;
        List<Integer> rows = new ArrayList();
        List<Integer> cols = new ArrayList();
        for(int i = 0; i < m; i ++) {
            for(int j = 0; j < n; j ++) {
                if(grid[i][j] == 1)
                    rows.add(i);
            }
        }
        for(int j = 0; j < n; j ++) {
            for(int i = 0; i < m; i ++) {
                if(grid[i][j] == 1)
                    cols.add(j);
            }
        }
        
        return getMin(rows) + getMin(cols);
    }
    
    public int getMin(List<Integer> list) {
        int left = 0, right = list.size() - 1;
        int sum = 0;
        while(left < right)
            sum += list.get(right --) - list.get(left ++);
        return sum;
    }

```

# Smallest Rectangle Enclosing Black Pixels

An image is represented by a binary matrix with 0 as a white pixel and 1 as a black pixel. The black pixels are connected, i.e., there is only one black region. Pixels are connected horizontally and vertically. Given the location (x, y) of one of the black pixels, return the area of the smallest (axis-aligned) rectangle that encloses all black pixels.

## Example

```
[
  "0010",
  "0110",
  "0100"
]
and x = 0, y = 2,
Return 6.

```
## Solution

I used dfs to find the boundary, but we don't need to search every black cell. All the black cells are connected, in a projected 1D array all the black pixels are also connected.

We can use binary search to search boundaries.

```
    public int minArea(char[][] image, int x, int y) {
        if(image == null || image.length == 0 || image[0].length == 0)
            return 0;
        int y_min = searchCols(image, 0, y, true);
        int y_max = searchCols(image, y + 1, image[0].length, false);
        int x_min = searchRows(image, 0, x, true);
        int x_max = searchRows(image, x + 1, image.length, false);
        
        return (x_max - x_min) * (y_max - y_min);
    }
    
    public int searchCols(char[][] image, int left, int right, boolean leftPart) {
        int m = image.length;
        while(left < right) {
            int mid = (right - left) /2 + left;
            int k = 0;
            while(k < m && image[k][mid] == '0') k ++;
            if((k < m) == leftPart)
                right = mid;
            else
                left = mid + 1;
        }
        return left;
    }
    
    public int searchRows(char[][] image, int top, int bottom, boolean topPart) {
        int n = image[0].length;
        while(top < bottom) {
            int mid = (bottom - top) /2 + top;
            int k = 0;
            while(k < n && image[mid][k] == '0') k ++;
            if((k < n) == topPart)
                bottom = mid;
            else
                top = mid + 1;
        }
        return top;
    }

```

# Valid Word Square (easy)

Given a sequence of words, check whether it forms a valid word square.

A sequence of words forms a valid word square if the kth row and column read the exact same string, where 0 ≤ k < max(numRows, numColumns).

Note:
1. The number of words given is at least 1 and does not exceed 500.
2. Word length will be at least 1 and does not exceed 500.
3. Each word contains only lowercase English alphabet a-z.

## Example

```
Input:
[
  "abcd",
  "bnrt",
  "crmy",
  "dtye"
]

Output:
true

Input:
[
  "abcd",
  "bnrt",
  "crm",
  "dt"
]

Output:
true

Input:
[
  "ball",
  "area",
  "read",
  "lady"
]

Output:
false

```

## Solution

Simple compare every symmetric char.

I failed on this case: ["ball","asee","let","lep"]

```
    public boolean validWordSquare(List<String> words) {
        if(words == null || words.size() == 0) return true;
        int len = words.size();
        for(int i = 0; i < len; i ++) {
            if(words.get(i).length() > len) return false;
            for(int j = 0; j < words.get(i).length(); j ++) {
                if(words.get(j).length() <= i || words.get(j).charAt(i) != words.get(i).charAt(j))
                    return false;
            }
        }
        return true;
    }

```

# Word Squares

Given a set of words (without duplicates), find all word squares you can build from them.

## Example

```
Input:
["area","lead","wall","lady","ball"]

Output:
[
  [ "wall",
    "area",
    "lead",
    "lady"
  ],
  [ "ball",
    "area",
    "lead",
    "lady"
  ]
]

Input:
["abat","baba","atan","atal"]

Output:
[
  [ "baba",
    "abat",
    "baba",
    "atan"
  ],
  [ "baba",
    "abat",
    "baba",
    "atal"
  ]
]

```

## Solution

We know that the sequence contains 4 words because the length of each word is 4.

Every word can be the first word of the sequence, let's take "wall" for example.

Which word could be the second word? Must be a word start with "a" (therefore "area"), because it has to match the second letter of word "wall".

Which word could be the third word? Must be a word start with "le" (therefore "lead"), because it has to match the third letter of word "wall" and the third letter of word "area".

What about the last word? Must be a word start with "lad" (therefore "lady"). For the same reason above.




```
    class TrieNode {
        List<String> startWith;
        TrieNode[] children;
        
        public TrieNode() {
            startWith = new ArrayList();
            children = new TrieNode[26];
        }
    }
    
    class Trie {
        TrieNode root;
        
        public Trie(String[] words) {
            root = new TrieNode();
            for(String word: words) {
                TrieNode cur = root;
                for(char ch: word.toCharArray()) {
                    int idx = ch - 'a';
                    if(cur.children[idx] == null)
                        cur.children[idx] = new TrieNode();
                    cur.children[idx].startWith.add(word);
                    cur = cur.children[idx];
                } 
            }
        }
        
        public List<String> findPrefix(String prefix) {
            List<String> result = new ArrayList();
            TrieNode cur = root;
            for(char ch: prefix.toCharArray()) {
                int idx = ch - 'a';
                if(cur.children[idx] == null)
                    return result;
                cur = cur.children[idx];
            }
            return cur.startWith;
        }
    }
    
    public List<List<String>> wordSquares(String[] words) {
        List<List<String>> result = new ArrayList();
        if(words == null || words.length == 0) return result;
        int len = words[0].length();
        Trie trie = new Trie(words);
        List<String> cur = new ArrayList();
        for(String word: words) {
            cur.add(word);
            dfs(result, cur, trie, len);
            cur.remove(cur.size()-1);
        }
        return result;
    }
    
    public void dfs(List<List<String>> result, List<String> cur, Trie trie, int len) {
        if(cur.size() == len) {
            result.add(new ArrayList(cur));
            return;
        }
        int index = cur.size();
        StringBuilder prefix = new StringBuilder();
        for(String w: cur)
            prefix.append(w.charAt(index));
        List<String> startWith = trie.findPrefix(prefix.toString());
        for(String s: startWith) {
            cur.add(s);
            dfs(result, cur, trie, len);
            cur.remove(cur.size()-1);
        }
    }
```

# Longest Substring with At Most Two Distinct Characters

Given a string, find the length of the longest substring T that contains at most 2 distinct characters.

For example, Given s = “eceba”,

T is "ece" which its length is 3.

## Solution

```
    public int lengthOfLongestSubstringTwoDistinct(String s) {
        int start = 0, next = -1;
        int result = 0;
        for(int end = 1; end < s.length(); end ++) {
            if(s.charAt(end) == s.charAt(end-1)) continue;
            if(next > -1 && s.charAt(end) != s.charAt(next-1))
            {
                result = Math.max(result, end - start);
                start = next;
            }
            next = end;
        }
        return result > s.length() - start ? result : s.length() - start;
    }

```

# Longest Substring with At Most K Distinct Characters

Given a string, find the length of the longest substring T that contains at most k distinct characters.

For example, Given s = “eceba” and k = 2,

T is "ece" which its length is 3.

## Solution using HashMap

```
    public int lengthOfLongestSubstringKDistinct(String s, int k) {
        HashMap<Character, Integer> map = new HashMap();
        int start = 0;
        int result = 0;
        for(int end = 0; end < s.length(); end ++) {
            map.put(s.charAt(end), end);
            if(map.size() > k) {
                int leftMost = end;
                for(int value: map.values()) {
                    leftMost = Math.min(leftMost, value);
                }
                map.remove(s.charAt(leftMost));
                start = leftMost + 1;
            }
            result = Math.max(result, end - start + 1);
        }
        return result;
    }

```

## Solution using Sliding Window

```
    public int lengthOfLongestSubstringKDistinct(String s, int k) {
        int[] count = new int[256];
        int num = 0, start = 0, result = 0;
        for(int i = 0; i < s.length(); i ++) {
            if(count[s.charAt(i)] ++ == 0) num ++;
            if(num > k) {
                while(-- count[s.charAt(start++)] > 0);
                num --;
            }
            result = Math.max(result, i - start + 1);
        }
        return result;
    }

```


# Word Pattern

Given a pattern and a string str, find if str follows the same pattern.

Here follow means a full match, such that there is a bijection between a letter in pattern and a non-empty substring in str.

## Examples:

```
pattern = "abab", str = "redblueredblue" should return true.
pattern = "aaaa", str = "asdasdasdasd" should return true.
pattern = "aabb", str = "xyzabcxzyabc" should return false.
```

## Solution

Backtracking

```
    public boolean wordPatternMatch(String pattern, String str) {
        HashMap<Character, String> map = new HashMap();
        HashSet<String> used = new HashSet();
        return helper(map, used, pattern, 0, str, 0);
    }
    
    public boolean helper(HashMap<Character, String> map, HashSet<String> used, String pattern, int idx, String str, int pos) {
        if(pos == str.length() && idx == pattern.length())
            return true;
        if(pos == str.length() || idx == pattern.length())
            return false;
        char ch = pattern.charAt(idx);
        if(map.containsKey(ch)) {
            String temp = map.get(ch);
            if(!str.startsWith(temp, pos))
                return false;
            return helper(map, used, pattern, idx+1, str, pos + temp.length());
        }
        for(int i = pos; i < str.length(); i ++) {
            String s = str.substring(pos, i+1);
            if(used.contains(s)) continue;
            map.put(ch, s);
            used.add(s);
            if(helper(map, used, pattern, idx+1, str, i+1))
                return true;
            map.remove(ch);
            used.remove(s);
        }
        return false;
    }

```

#  Closest Binary Search Tree Value II

Given a non-empty binary search tree and a target value, find k values in the BST that are closest to the target.

Note:
- Given target value is a floating point.
- You may assume k is always valid, that is: k ≤ total nodes.
- You are guaranteed to have only one unique set of k values in the BST that are closest to the target.
- Follow up: assume that the BST is balanced, could you solve it in less than O(n) runtime (where n = total nodes)?

## Solution

1. Consider implement these two helper functions:
 - getPredecessor(N), which returns the next smaller node to N.
 - getSuccessor(N), which returns the next larger node to N.
2. Try to assume that each node has a parent pointer, it makes the problem much easier.
3. Without parent pointer we just need to keep track of the path from the root to the current node using a stack.
4. You would need two stacks to track the path in finding predecessor and successor node separately.

```
    public List<Integer> closestKValues(TreeNode root, double target, int k) {
        Stack<TreeNode> pred = new Stack();
        Stack<TreeNode> succ = new Stack();
        initialStack(root, target, pred, succ);
        
        List<Integer> result = new ArrayList();
        
        while((!pred.isEmpty() || !succ.isEmpty()) && k -- > 0) {
            if(pred.isEmpty())
                result.add(getSucc(succ));
            else if(succ.isEmpty())
                result.add(getPred(pred));
            else if(Math.abs(succ.peek().val-target) < Math.abs(pred.peek().val-target))
                result.add(getSucc(succ));
            else
                result.add(getPred(pred));
        }
        return result;
    }
    
    public void initialStack(TreeNode root, double target, Stack<TreeNode> pred, Stack<TreeNode> succ) {
        TreeNode cur = root;
        while(cur != null) {
            if(cur.val > target) {
                succ.push(cur);
                cur = cur.left;
            } else {
                pred.push(cur);
                cur = cur.right;
            }
        } 
    }
    
    public int getPred(Stack<TreeNode> pred) {
        TreeNode res = pred.pop();
        TreeNode cur = res.left;
        while(cur != null) {
            pred.push(cur);
            cur = cur.right;
        }
        return res.val;
    }
    
    public int getSucc(Stack<TreeNode> succ) {
        TreeNode res = succ.pop();
        TreeNode cur = res.right;
        while(cur != null) {
            succ.push(cur);
            cur = cur.left;
        }
        return res.val;
    } 

```

# Shortest Distance from All Buildings

You want to build a house on an empty land which reaches all buildings in the shortest amount of distance. You can only move up, down, left and right. You are given a 2D grid of values 0, 1 or 2, where:

- Each 0 marks an empty land which you can pass by freely.
- Each 1 marks a building which you cannot pass through.
- Each 2 marks an obstacle which you cannot pass through.

For example, given three buildings at (0,0), (0,4), (2,2), and an obstacle at (0,2):

```
1 - 0 - 2 - 0 - 1
|   |   |   |   |
0 - 0 - 0 - 0 - 0
|   |   |   |   |
0 - 0 - 1 - 0 - 0
```
The point (1,2) is an ideal empty land to build a house, as the total travel distance of 3+3+1=7 is minimal. So return 7.

There will be at least one building. If it is not possible to build such house according to the above rules, return -1.

## Solution

Walk only onto the cells that were reachable from all previous buildings. From the first building I only walk onto cells where grid is 0, and make them -1. From the second building I only walk onto cells where grid is -1, and I make them -2. And so on.

Use a distance matrix to record the total distance from each cell to all the buildings.

```
    int[] delta = {-1, 0, 1, 0, -1};
    int min = Integer.MAX_VALUE;
    
    public int shortestDistance(int[][] grid) {
        if(grid == null || grid.length == 0 || grid[0].length == 0)
            return 0;
        int m = grid.length, n = grid[0].length;
        int[][] dis = new int[m][n];
        int start = 0;
        for(int i = 0; i < m; i ++) {
            for(int j = 0; j < n; j ++) {
                if(grid[i][j] == 1) {
                    bfs(grid, dis, i, j, start --);
                }
            }
        }
        return min == Integer.MAX_VALUE ? -1 : min;
    }

    public void bfs(int[][] grid, int[][] dis, int row, int col, int start) {
        Queue<int[]> queue = new LinkedList();
        queue.offer(new int[]{row, col});
        int level = 0;
        min = Integer.MAX_VALUE;
        while(!queue.isEmpty()) {
            int size = queue.size();
            level ++;
            while(size -- > 0) {
                int[] cur = queue.poll();
                for(int i = 1; i < delta.length; i ++) {
                    int x = cur[0] + delta[i-1];
                    int y = cur[1] + delta[i];
                    if(x < 0 || x == grid.length || y < 0 || y == grid[0].length || grid[x][y] != start)
                        continue;
                    dis[x][y] += level;
                    grid[x][y] --;
                    queue.offer(new int[]{x, y});
                    min = Math.min(min, dis[x][y]);
                }
            }
        }
    }

```

# Rearrange String k Distance Apart


Given a non-empty string str and an integer k, rearrange the string such that the same characters are at least distance k from each other.

All input strings are given in lowercase letters. If it is not possible to rearrange the string, return an empty string "".

## Example


```
str = "aabbcc", k = 3

Result: "abcabc"

The same letters are at least distance 3 from each other.

str = "aaabc", k = 3 

Answer: ""

It is not possible to rearrange the string.

str = "aaadbbcc", k = 2

Answer: "abacabcd"

Another possible answer is: "abcabcda"

The same letters are at least distance 2 from each other.

```

## Solution

Use a hashMap to record the count for every character. Use a max heap to sort characters according to frequency. Use a waitQueue to freeze characters which are currently in the k size window.

```
    public String rearrangeString(String str, int k) {
        if(k < 2 || str.length() < 2) return str;
        HashMap<Character, Integer> map = new HashMap();
        StringBuilder sb = new StringBuilder();
        for(char ch: str.toCharArray()) {
            if(!map.containsKey(ch))
                map.put(ch, 0);
            map.put(ch, map.get(ch)+1);
        }
        
		// Both ways are correct to create a priorityqueue.
        // Queue<Map.Entry<Character, Integer>> maxHeap 
        //     = new PriorityQueue(new Comparator<Map.Entry<Character, Integer>>() {
        //         public int compare(Map.Entry<Character, Integer> e1, Map.Entry<Character, Integer> e2) {
        //             return e2.getValue() - e1.getValue();
        //         }
        //         });
        PriorityQueue<Map.Entry<Character, Integer>> maxHeap 
            = new PriorityQueue<>((a, b) -> (b.getValue() - a.getValue()));
        
        maxHeap.addAll(map.entrySet());
        Queue<Map.Entry<Character, Integer>> waitQueue = new LinkedList<>();
        
        while(!maxHeap.isEmpty()) {
            Map.Entry<Character, Integer> cur = maxHeap.poll();
            sb.append(cur.getKey());
            cur.setValue(cur.getValue()-1);
            waitQueue.offer(cur);
            
            if(waitQueue.size() < k) continue;
            
            Map.Entry<Character, Integer> front = waitQueue.poll();
            if(front.getValue() > 0)
                maxHeap.offer(front);
        }
        
        return sb.length() == str.length() ? sb.toString() : "";
    }

```

# Alien Dictionary

There is a new alien language which uses the latin alphabet. However, the order among letters are unknown to you. You receive a list of words from the dictionary, where words are sorted lexicographically by the rules of this new language. Derive the order of letters in this language.

1. You may assume all letters are in lowercase.
2. If the order is invalid, return an empty string.
3. There may be multiple valid order of letters, return any one of them is fine.

## Example

```
Given the following words in dictionary,
[
  "wrt",
  "wrf",
  "er",
  "ett",
  "rftt"
]
The correct order is: "wertf".

```

## Solution

```
    public String alienOrder(String[] words) {
        Map<Character, Set<Character>> map = new HashMap();
        Map<Character, Integer> degree = new HashMap();
        for(String w: words) {
            for(char ch: w.toCharArray())
                degree.put(ch, 0);
        }
        
        for(int i = 1; i < words.length; i ++) {
            String w1 = words[i-1];
            String w2 = words[i];
			//if dictionary is ["wtfkj", "wtf"], should return " "
            if (w1.length() > w2.length() && w2.equals(w1.substring(0, w2.length()))) 
                return "";
            int len = Math.min(w1.length(), w2.length());
            for(int j = 0; j < len; j ++) {
                char c1 = w1.charAt(j);
                char c2 = w2.charAt(j);
                if(c1 != c2) {
                    if(!map.containsKey(c1))
                        map.put(c1, new HashSet());
                    if(map.get(c1).add(c2)) {
                        degree.put(c2, degree.get(c2) + 1);
                    }
                    break;
                }
            }
        }
        
        Queue<Character> queue = new LinkedList();
        for(Map.Entry<Character, Integer> entry: degree.entrySet()) {
            if(entry.getValue() == 0) queue.offer(entry.getKey());
        }
        StringBuilder res = new StringBuilder();
        
        while(!queue.isEmpty()) {
            char cur = queue.poll();
            res.append(cur);
            if(map.containsKey(cur)){
                for(char next: map.get(cur)) {
                    degree.put(next, degree.get(next)-1);
                    if(degree.get(next) == 0)
                        queue.offer(next);
                }
            }
        }
        if(res.length() != degree.size()) return "";
        return res.toString();
    }

```

# Optimal Account Balancing

A group of friends went on holiday and sometimes lent each other money. For example, Alice paid for Bill's lunch for $10. Then later Chris gave Alice $5 for a taxi ride. We can model each transaction as a tuple (x, y, z) which means person x gave person y $z. Assuming Alice, Bill, and Chris are person 0, 1, and 2 respectively (0, 1, 2 are the person's ID), the transactions can be represented as [[0, 1, 10], [2, 0, 5]].

Given a list of transactions between a group of people, return the minimum number of transactions required to settle the debt.

Note:

1. A transaction will be given as a tuple (x, y, z). Note that x ≠ y and z > 0.
2. Person's IDs may not be linear, e.g. we could have the persons 0, 1, 2 or we could also have the persons 0, 2, 6.

## Example

```
Input:
[[0,1,10], [1,0,1], [1,2,5], [2,0,5]]

Output:
1

Explanation:
Person #0 gave person #1 $10.
Person #1 gave person #0 $1.
Person #1 gave person #2 $5.
Person #2 gave person #0 $5.

Therefore, person #1 only need to give person #0 $4, and all debt is settled.

```

## Solution

```
    public int minTransfers(int[][] transactions) {
        HashMap<Integer, Integer> map = new HashMap();
        for(int[] trans: transactions) {
            map.put(trans[0], map.getOrDefault(trans[0], 0) - trans[2]);
            map.put(trans[1], map.getOrDefault(trans[1], 0) + trans[2]);
        }
        List<Integer> balance = new ArrayList();
        int index = 0;
        for(int v: map.values()) {
            if(v != 0)
                balance.add(v);
        }
        return cleanBalance(balance) + minMatch(balance, 0);
        
        
    }
    
    public int cleanBalance(List<Integer> balance) {
        Collections.sort(balance);
        int left = 0, right = balance.size()-1;
        int count = 0;
        while(left < right) {
            if(- balance.get(left) == balance.get(right)) {
                count ++;
                balance.remove(right);
                balance.remove(left);
                right -= 2;
            } else if(- balance.get(left) > balance.get(right))
                left ++;
            else
                right --;
        }
        return count;
    }
    
    public int minMatch(List<Integer> balance, int start) {
        int min = Integer.MAX_VALUE;
        while(start < balance.size() && balance.get(start) == 0)
            start ++;
        if(start == balance.size())
            return 0;
        int cur = balance.get(start);
        for(int i = start + 1; i < balance.size(); i ++) {
            if(balance.get(i) * cur < 0) {
                balance.set(i, balance.get(i) + cur);
                min = Math.min(min, 1 + minMatch(balance, start + 1));
                balance.set(i, balance.get(i) - cur);
            }
        }
        return min;
    }

```
