#  350. Intersection of Two Arrays II

 https://leetcode-cn.com/problems/intersection-of-two-arrays-ii/solution/liang-ge-shu-zu-de-jiao-ji-ii-by-leetcode/ 

1. 散列表

   ```java
   //修改原数组，空间O(MIN(M,N))，时间O(M+N)。
   public int[] intersect(int[] nums1, int[] nums2) {
       if (nums1.length > nums2.length) {
           return intersect(nums2, nums1);
       }
       HashMap<Integer, Integer> map = new HashMap<>();
       for (int n : nums1) {
           map.put(n, map.getOrDefault(n, 0) + 1);
       }
       int k = 0;
       for (int n : nums2) {
           int cnt = map.getOrDefault(n, 0);
           if (cnt > 0) {
               nums1[k++] = n;
               map.put(n, cnt - 1);
           }
       }
       return Arrays.copyOfRange(nums1, 0, k);
   }
   //不修改原数组，空间O(MAX(M,N))，时间O(M+N)。
   public int[] intersect(int[] nums1, int[] nums2) {
       if (nums1.length > nums2.length) {
           return intersect(nums2, nums1);
       }
       HashMap<Integer, Integer> map = new HashMap<>();
       for (int n : nums1) {
           map.put(n, map.getOrDefault(n, 0) + 1);
       }
       //将列表长度初始化为更长的数组长度，防止列表扩容减慢速度
       List<Integer> temp = new ArrayList<>(nums2.length);
       for (int n : nums2) {
           int cnt = map.getOrDefault(n, 0);
           if (cnt > 0) {
               temp.add(n);
               map.put(n, cnt - 1);
           }
       }
       int[] res = new int[temp.size()];
       for(int i = 0; i < res.length; i++) {
           res[i] = temp.get(i);
       }
       return res;
   }
   ```

   

2. 排序

   ```java
   //修改原数组，空间O(1)，时间O(MlogM+NlogN)
   public int[] intersect(int[] nums1, int[] nums2) {
       Arrays.sort(nums1);
       Arrays.sort(nums2);
       int i = 0, j = 0, k = 0;
       while (i < nums1.length && j < nums2.length) {
           if (nums1[i] < nums2[j]) {
               ++i;
           } else if (nums1[i] > nums2[j]) {
               ++j;
           } else {
               nums1[k++] = nums1[i++];
               ++j;
           }
       }
       return Arrays.copyOfRange(nums1, 0, k);
   }
   //不修改元素组和解法1类似，使用列表
   ```

   

