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

 

