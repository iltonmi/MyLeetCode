#  81. Search in Rotated Sorted Array II 

原理同 33. Search in Rotated Sorted Array

```java
public boolean search(int[] nums, int target) {
    if(nums == null) {
        return false;
    }
    if(nums.length == 1) {
        return nums[0] == target ? true : false;
    }
    int i = nums.length - 2;
    for(; i >= 0 && nums[i] <= nums[i+1]; i--) {}
    int start = i + 1;
    return pivotBinarySearch(nums, target, i < 0 ? 0 : i + 1) != -1;
}

private int pivotBinarySearch(int[] nums, int target, int start) {
    int offset = start;
    int left = 0;
    int right = nums.length - 1;
    while(left <= right) {
        int mid = (left + right) / 2;
        int realMid = (mid + offset) % nums.length;
        if(nums[realMid] == target) {
            return realMid;
        }
        if(nums[realMid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return -1;
}
```

