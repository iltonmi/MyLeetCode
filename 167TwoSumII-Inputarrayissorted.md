#  167. Two Sum II - Input array is sorted 

双指针。

```java
public int[] twoSum(int[] numbers, int target) {
    int l = 0;
    int r = numbers.length - 1;
    while(l < r) {
        int cur = numbers[l] + numbers[r];
        if(cur > target) {
            r--;
        } else if(cur < target) {
            l++;
        } else {
            return new int[]{l + 1, r + 1};
        }
    }
    return new int[]{-1, -1};
}
```

