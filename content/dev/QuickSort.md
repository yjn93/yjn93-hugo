+++
Categories = ["Algorithm"]
Description = "Quick sort idea for finding kth largest element in array"
Tags = ["LeetCode", "Basic algorithm"]
date = "2016-12-21T09:59:54-05:00"
menu = "main"
title = "QuickSort"

+++

# Kth Largest Element in an Array
- [LeetCode 215](https://leetcode.com/problems/kth-largest-element-in-an-array/)

Find the kth largest element in an unsorted array. 

## Example

Given [3,2,1,5,6,4] and k = 2, return 5.

## Solution
Quickselect, using quicksort idea. Two implementation of quicksort:

### First implementation of partition:
```
    public int partition(int[] nums, int start, int end) {
        int pivot = nums[start];
        int left = start, right = end+1;
        while(true) {
            while(left < end && nums[++ left] < pivot);
            while(start < right && nums[-- right] > pivot );
            if(left < right) swap(nums, left, right);
            else break;
        }
        swap(nums, start, right);
        return right;
    }
```

### Second implementation of partition:
```
    public int partition(int[] nums, int lo, int hi) {
    	int left = lo, right = hi, pivot = a[hi];
    	while (left < right) {
	      if (a[left++] > pivot) swap(a, --`left, -- right);
    	}
    	swap(a, left, hi);
		return left;
	}
```

### Complete Solution
```
    public int findKthLargest(int[] nums, int k) {
        k = nums.length - k;
        int lo = 0, hi = nums.length-1;
        while(lo < hi) {
            int split = partition(nums, lo, hi);
            if(split < k) lo = split+1;
            else if(split > k) hi = split - 1;
            else break;
        }
        return nums[k];
    }
    
    public int partition(int[] nums, int start, int end) {
        int pivot = nums[start];
        int left = start, right = end+1;
        while(true) {
            while(left < end && nums[++ left] < pivot);
            while(start < right && nums[-- right] > pivot );
            if(left < right) swap(nums, left, right);
            else break;
        }
        swap(nums, start, right);
        return right;
    }
    
    public void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
```

