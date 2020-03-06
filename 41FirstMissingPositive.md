#  41. First Missing Positive 

 https://leetcode-cn.com/problems/first-missing-positive/solution/que-shi-de-di-yi-ge-zheng-shu-by-leetcode/ 

> 算法
>
> 现在可以开始写算法了。
>
> * 检查 1 是否存在于数组中。如果没有，则已经完成，1 即为答案。
> * 如果 nums = [1]，答案即为 2 。
> * 将负数，零，和大于 n 的数替换为 1 。
> * 遍历数组。当读到数字 a 时，替换第 a 个元素的符号。
> * 注意重复元素：只能改变一次符号。由于没有下标 n ，使用下标 0 的元素保存是否存在数字 n。
> * 再次遍历数组。返回第一个正数元素的下标。
> * 如果 nums[0] > 0，则返回 n 。
> * 如果之前的步骤中没有发现 nums 中有正数元素，则返回n + 1。



```java
public int firstMissingPositive(int[] nums) {
    int n = nums.length;

    // 基本情况
    int contains = 0;
    for (int i = 0; i < n; i++)
      if (nums[i] == 1) {
        contains++;
        break;
      }

    if (contains == 0)
      return 1;

    // nums = [1]
    if (n == 1)
      return 2;

    // 用 1 替换负数，0，
    // 和大于 n 的数
    // 在转换以后，nums 只会包含
    // 正数
    for (int i = 0; i < n; i++)
      if ((nums[i] <= 0) || (nums[i] > n))
        nums[i] = 1;

    // 使用索引和数字符号作为检查器
    // 例如，如果 nums[1] 是负数表示在数组中出现了数字 `1`
    // 如果 nums[2] 是正数 表示数字 2 没有出现
    for (int i = 0; i < n; i++) {
      int a = Math.abs(nums[i]);
      // 如果发现了一个数字 a - 改变第 a 个元素的符号
      // 注意重复元素只需操作一次
      if (a == n)
        nums[0] = - Math.abs(nums[0]);
      else
        nums[a] = - Math.abs(nums[a]);
    }

    // 现在第一个正数的下标
    // 就是第一个缺失的数
    for (int i = 1; i < n; i++) {
      if (nums[i] > 0)
        return i;
    }

    if (nums[0] > 0)
      return n;

    return n + 1;
}
```

