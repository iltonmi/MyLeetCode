# 27. Remove Element

kill the game

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int l = 0;
        int r = 0;
        while(r < nums.length) {
            if(nums[r] != val) {
                swap(nums, l, r);
                l++;
            }
            r++;
        }
        return l;
    }
    
    private void swap(int[] arr, int l, int r) {
        int temp = arr[l];
        arr[l] = arr[r];
        arr[r] = temp;
    }
}
```

 