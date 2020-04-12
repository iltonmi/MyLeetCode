#  16. 3Sum Closest

双指针，空间O(1)，时间O(N ^ 2)

```java
public int threeSumClosest(int[] nums, int target) {
    Arrays.sort(nums);
    int closest = Integer.MAX_VALUE;
    int min;
    int res = Integer.MAX_VALUE;
    for (int i = 0; i < nums.length - 2; i++) {
        if(i != 0 && nums[i] == nums[i-1]) {
            continue;
        }
        int left = i + 1;
        int right = nums.length - 1;
        while (left < right) {
            min = nums[left] + nums[right] + nums[i] - target;
            if (min == 0) {
                return target;
            }
            if (Math.abs(min) < Math.abs(closest)) {
                closest = min;
            }
            if (min > 0) {
                right--;
            } else {
                left++;
            }
        }
    }
    return closest + target;
}
```

