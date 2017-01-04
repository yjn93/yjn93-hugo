+++
Categories = ["Algorithm"]
Description = "Three variations of Contains Duplicate Problem"
Tags = ["LeetCode", "TreeMap", "Bucket"]
date = "2017-01-02T11:02:39-05:00"
menu = "main"
title = "Contains Duplicate"

+++

# Contains Duplicate

Given an array of integers, find if the array contains any duplicates.

This is the basic problem. The solution is to use a set.

```
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> set = new HashSet();
        for(int num: nums) {
            if(!set.add(num))
                return true;
        }
        return false;
    }
```

# Contains Duplicate within a window

- [LeetCode 219] (https://leetcode.com/problems/contains-duplicate-ii/)

Given an array of integers and an integer k, find out whether there are two distinct indices i and j in the array such that nums[i] = nums[j] and the difference between i and j is at most k.

## Solution

We can use a set or a hashmap. We only need to maintain the window size in for loop.

```
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        Set<Integer> set = new HashSet();
        for(int i = 0; i < nums.length; i ++) {
            if(!set.add(nums[i]))
                return true;
            if(i >= k)
                set.remove(nums[i-k]);
        }
        return false;
    }
```

# Contains Duplicate in a range within a window

- [LeetCode 220] (https://leetcode.com/problems/contains-duplicate-iii/)

Given an array of integers, find out whether there are two distinct indices i and j in the array such that the difference between nums[i] and nums[j] is at most t and the difference between i and j is at most k.

## TreeMap solution
Instead of strict duplicate, there is now a constraint on the range of the values of the elements to be considered duplicates, it reminds us of doing a range check which is implemented in tree data structure and would take O(LogN) if a balanced tree structure is used.

We can use Java TreeSet class. The floor and ceiling method can return the greatest and smallest element within a range in the set.

```
    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
        TreeSet<Integer> set = new TreeSet();
        for(int i = 0; i < nums.length; i ++) {
            Integer floor = set.floor(nums[i] + t);
            Integer ceiling = set.ceiling(nums[i] - t);
            if((floor != null && floor >= nums[i]) || (ceiling != null && ceiling <= nums[i]))
                return true;
            set.add(nums[i]);
            if(i >= k)
                set.remove(nums[i-k]);
        }
        return false;
    }
```

## Bucket solution

This solution is doing constant time complexity.

Notice: to avoid overflow, we need to use Long type.

Another complication is that negative ints are allowed. A simple num / t just shrinks everything towards 0. Therefore, we can just reposition every element to start from Integer.MIN_VALUE.

```
    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
        if(k < 1 || t < 0) return false;
        Map<Long, Long> map = new HashMap();
        for(int i = 0; i < nums.length; i ++) {
            long newNum = (long)nums[i] - Integer.MIN_VALUE;
            long bucket = newNum/((long)t + 1);
            
            if(map.containsKey(bucket) || 
                map.containsKey(bucket-1) && newNum - map.get(bucket-1) <= t ||
                map.containsKey(bucket+1) && map.get(bucket+1) - newNum <= t)
                return true;
            map.put(bucket, newNum);
            if(i >= k) {
                long lastBucket = ((long)nums[i-k] - Integer.MIN_VALUE)/((long)t + 1);
                map.remove(lastBucket);
            }
        }
        return false;
    }
```
