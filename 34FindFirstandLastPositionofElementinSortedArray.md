# 34. Find First and Last Position of Element in Sorted Array 

二分查找最坏时间复杂度：O(logN)

中心扩展最坏时间复杂度：O(N)，中心扩展均摊时间复杂度：O(N)

最终时间O(N)，空间O(1)。

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        if(nums == null) {
            return new int[]{-1, -1};
        }
        //二分查找
        int index = binarySearch(nums, target);
        //中心扩展
        return expandAroundCenter(nums, index);
    }
    
    private int[] expandAroundCenter(int[] arr, int center) {
        if(center < 0 || center > arr.length - 1) {
            return new int[]{-1, -1};
        }
        int val = arr[center];
        int left = center - 1;
        int right = center + 1;
        for(;left >= 0 && arr[left] == val; left--) {}
        for(;right < arr.length && arr[right] == val; right++) {}
        return new int[]{left + 1, right - 1};
    }
    
    private int binarySearch(int[] arr, int target) {
        int lo = 0;
        int hi = arr.length - 1;
        while(lo <= hi) {
            int mid = (lo + hi) / 2;
            if(arr[mid] == target) {
                return mid;
            }
            if(arr[mid] < target) {
                lo = mid + 1;
            } else {
                hi = mid - 1;
            }
        }
        return -1;
    }
}
```

