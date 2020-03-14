#  35. Search Insert Position

重点：对二分查找范围收缩范围的微调。

因为我们要找的是一个正好大于等于target的数。

因此我们可以找到刚好小于等于target的数，

```java
public int searchInsert(int[] nums, int target) {
    if(nums == null || nums.length == 0) {
        return 0;
    }
    int l = 0;
    int r = nums.length - 1;
    while(l < r) {
        int mid = r + (l - r) / 2;
        //1. 当l = 0, r = 2时，会变成l = 1, r = 2（符合情况2）或者l = 0, r = 0;
        //2. 当l = 0, r = 1时，会变成l = 0, r = 0或者l = 1, r = 1;
        //所以说，结束条件总是l == r
        //那么，如果结束时arr[r] == target, 返回
        //否则，由于arr[r] < target, 返回arr[r+1] > target
        if(nums[mid] <= target) {
            l = mid;
        } else {
            r = mid - 1;
        }
    }
    return nums[l] < target ? l + 1 : l;
    //return nums[r] < target ? r + 1 : r;
}
```

