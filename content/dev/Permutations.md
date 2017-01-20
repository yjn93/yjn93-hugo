+++
Categories = ["Algorithm"]
Tags = ["LeetCode", "BackTracking"]
Description = ""
date = "2016-12-29T16:03:10-05:00"
title = "Permutations"
menu = "main"

+++

# Permutation
- [LeetCode 46](https://leetcode.com/problems/permutations/)

Given a collection of distinct numbers, return all possible permutations.

## BackTracking by checking whether current list contains an element
```
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        helper(result, new ArrayList(), nums);
        return result;
    }

    public void helper(List<List<Integer>> result, List<Integer> cur, int[] nums) {
        if(cur.size() == nums.length) {
            result.add(new ArrayList(cur));
            return;
        }
        for(int i = 0; i < nums.length; i ++) {
            if(cur.contains(nums[i])) continue;
            cur.add(nums[i]);
            helper(result, cur, nums);
            cur.remove(cur.size()-1);
        }
    }
```

## BackTracking with swap
Try to put every element in current position

```
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        helper(result, nums, 0);
        return result;
    }

    public void helper(List<List<Integer>> result, int[] nums, int start) {
        if(start == nums.length) {
            List<Integer> cur = new ArrayList();
            for(int num: nums)
                cur.add(num);
            result.add(cur);
            return;
        }
        for(int i = start; i < nums.length; i ++) {
            swap(nums, i, start);
            helper(result, nums, start+1);
            swap(nums, i, start);
        }
    }
```
# Permutation 2
- [LeetCode 47](https://leetcode.com/problems/permutations-ii/)

Given a collection of numbers that might contain duplicates, return all possible unique permutations.

## Example
[1,1,2] have the following unique permutations:
```
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```

## BackTracking to fulfill current list
Use an extra boolean array " boolean[] used" to indicate whether the value is added to list.

Sort the array "int[] nums" to make sure we can skip the same value.

when a number has the same value with its previous, we can use this number only if his previous is used

```
    public List<List<Integer>> permuteUnique(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> result = new ArrayList();
        helper(result, new ArrayList(), nums, new boolean[nums.length]);
        return result;
    }

    public void helper(List<List<Integer>> result, List<Integer> cur, int[] nums, boolean[] used) {
        if(cur.size() == nums.length) {
            result.add(new ArrayList(cur));
            return;
        }
        for(int i = 0; i < nums.length; i ++) {
            if(used[i]) continue;
            if(i > 0 && nums[i] == nums[i-1] && !used[i-1]) continue;
            used[i] = true;
            cur.add(nums[i]);
            helper(result, cur, nums, used);
            cur.remove(cur.size()-1);
            used[i] = false;
        }
    }
```

# Next Permutation
- [LeetCode 31](https://leetcode.com/problems/next-permutation/)

Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

## Solution
```
    public void nextPermutation(int[] nums) {
        int len = nums.length;
        if(len < 2) return;
        int index = len - 1;
        while(index > 0 && nums[index] <= nums[index-1]) {
                index --;
        }
        if(index == 0) {
            reverse(nums, 0, len-1);
            return;
        }
        index --;
        int i = len-1;
        for(; i > 0; i --){
            if(nums[i] > nums[index])
                break;
        }
        swap(nums, index, i);
        reverse(nums, index+1, len-1);
    }

    public void reverse(int[] nums, int start, int end) {
        if(start >= end)
            return;
        for(int i = start; i <= (end+start)/2; i ++) {
            swap(nums, i, start+end-i);
        }
    }
```
# Permutation Sequence
- [LeetCode 60](https://leetcode.com/problems/permutation-sequence/)

The set [1,2,3,…,n] contains a total of n! unique permutations.

By listing and labeling all of the permutations in order,
We get the following sequence (ie, for n = 3):

```
1. "123"
2. "132"
3. "213"
4. "231"
5. "312"
6. "321"
```
Given n and k, return the kth permutation sequence.

## Solution
```
    public String getPermutation(int n, int k) {
        int[] factorial = new int[n+1];
        factorial[0] = 1;
        int sum = 1;
        List<Integer> nums = new ArrayList();
        for(int i = 1; i <= n; i ++) {
            sum *= i;
            factorial[i] = sum;
            nums.add(i);
        }
        
        k --;
        StringBuilder sb = new StringBuilder();
        for(int i = 1; i <= n; i ++) {
            int index = k/factorial[n-i];
            sb.append(nums.get(index));
            nums.remove(index);
            k -= index*factorial[n-i];
        }
        return sb.toString();
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

