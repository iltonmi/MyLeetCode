#  33. Search in Rotated Sorted Array

**题目中有这样一句话：**Your algorithm's runtime complexity must be in the order of *O*(log *n*).

显然，明示我们用二分查找了。

**回顾经典二分查找：**对比查找范围中间值和目标值。

**和经典二分查找的不同之处：**旋转后，单调递增序列跨越了数组边界。

**存在的问题：**左右边界low和high，以及中间值索引mid在缩小查找范围的时候需要跨越数组边界。

**解决思路：**

	1. 问题核心是寻找中间值索引，因为算法的核心是比较中间值和目标值。
 	2. 从此问题和经典二分查找中间值索引的联系中入手。
 	3. 若能将问题数组中的索引映射到原数组中的索引，则能够套用经典二分查找，解决单调递增序列跨越数组边界的问题。
 	4. 还原旋转的数组，应用经典二分查找，假设原数组中间值的索引是mid。在寻转数组中，中间值索引为realMid。
 	5. 假设旋转的偏移值为offset，对于旋转后没有跨越数组边界的部分，索引映射为index -> index + offset。对于旋转后跨越数组边界的部分，索引映射为index -> (index + offset) % length。
 	6. 所以realMid = (mid + offset) % length。
 	7. 没错，我们只需要利用经典二分查找模型，计算出mid，再将mid引映射到realMid，找到真正需要比较的中间值。再考虑中间值和目标值的比较结果，更新经典二分查找模型中的low和high。

**释疑：**

1. 因为只用到了中间值，索引不需要考虑原数组和旋转数组的映射。

代码如下：时间O(logn)，空间O(1)。

```java
class Solution {
    public int search(int[] nums, int target) {
        if(nums == null) {
            return -1;
        }
        if(nums.length == 1) {
            return nums[0] == target ? 0 : -1;
        }
        int i = nums.length - 2;
        for(; i >= 0 && nums[i] <= nums[i+1]; i--) {}
        int start = i + 1;
        return pivotBinarySearch(nums, target, i < 0 ? 0 : i + 1);
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
    
    // private int binarySearch(int[] nums, int target) {
    //     int left = 0;
    //     int right = nums.length - 1;
    //     while(left < right) {
    //         int mid = (left + right) / 2;
    //         if(nums[mid] == target) {
    //             return mid;
    //         }
    //         if(nums[mid] < target) {
    //             left = mid + 1;
    //         } else {
    //             right = mid - 1;
    //         }
    //     }
    //     return -1;
    // }
}
```

