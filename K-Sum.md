# K-Sum

 https://leetcode.com/problems/4sum/discuss/8609/My-solution-generalized-for-kSums-in-JAVA 

> #### General Idea
>
> If you have already read and implement the 3sum and 4sum by using the sorting approach: reduce them into 2sum at the end, you might already got the feeling that, all ksum problem can be divided into two problems:
>
> 1. 2sum Problem
> 2. Reduce K sum problem to K – 1 sum Problem
>
> Therefore, the ideas is simple and straightforward. We could use recursive to solve this problem. Time complexity is O(N^(K-1)).

```java
class Solution {
    int len = 0;
    public List<List<Integer>> fourSum(int[] nums, int target) {
        len = nums.length;
        Arrays.sort(nums);
        return kSum(nums, target, 4, 0);
    }
   private ArrayList<List<Integer>> kSum(int[] nums, int target, int k, int index) {
        ArrayList<List<Integer>> res = new ArrayList<List<Integer>>();
        if(index >= len) {
            return res;
        }
        if(k == 2) {
            int i = index, j = len - 1;
            while(i < j) {
                //find a pair
                if(target - nums[i] == nums[j]) {
                    List<Integer> temp = new ArrayList<>();
                    temp.add(nums[i]);
                    temp.add(target-nums[i]);
                    res.add(temp);
                    //skip duplication
                    while(i<j && nums[i]==nums[i+1]) {
                        i++;
                    }
                    while(i<j && nums[j-1]==nums[j]) {
                        j--;
                    }
                    i++;
                    j--;
                //move left bound
                } else if (target - nums[i] > nums[j]) {
                    i++;
                //move right bound
                } else {
                    j--;
                }
            }
        } else{
            for (int i = index; i < len - k + 1; i++) {
                //use current number to reduce ksum into k-1sum
                ArrayList<List<Integer>> temp = kSum(nums, target - nums[i], k-1, i+1);
                if(temp != null){
                    //add previous results
                    for (List<Integer> t : temp) {
                        t.add(0, nums[i]);
                    }
                    res.addAll(temp);
                }
                while (i < len-1 && nums[i] == nums[i+1]) {
                    //skip duplicated numbers
                    i++;
                }
            }
        }
        return res;
    }
}
```

