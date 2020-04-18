# 60. Permutation Sequence（第 k 个排列）

 [https://leetcode.com/problems/permutation-sequence/discuss/22507/%22Explain-like-I'm-five%22-Java-Solution-in-O(n)](https://leetcode.com/problems/permutation-sequence/discuss/22507/"Explain-like-I'm-five"-Java-Solution-in-O(n)) 

数学规律题，空间O(N)，时间O(N ^ 2)(时间复杂度考虑了remove操作)

```java
 public String getPermutation(int n, int k) {
    StringBuilder sb = new StringBuilder();
    List<Integer> num = new ArrayList<Integer>(n);
    int fact = 1;
    for (int i = 1; i <= n; i++) {
        // generate factorial 0!, 1!, ..., (n - 1)!
        fact *= i;
        // generate nums 1, 2, ..., n
        num.add(i);
    }
    k--;
    for (int i = 0; i < n; i++) {
        fact /= (n - i);
        int index = (k / fact);
        sb.append(num.remove(index));
        k -= index * fact;
    }
    return sb.toString();
}
```

 