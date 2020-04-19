#  77. Combinations 

回溯，剪枝，空间O(C(n, k))，时间O(k * C(n, k))

```java
class Solution {
    private List<List<Integer>> res = new LinkedList<>();
    private int n;
    public List<List<Integer>> combine(int n, int k) {
        this.n = n;
        help(new LinkedList<>(), 1, k);
        return res;
    }
    
    //k代表剩余被需要数字的数量
    private void help(LinkedList<Integer> temp, int start, int k) {
        if(0 == k) {
            res.add(new ArrayList(temp));
            return;
        }
        //n - (k - 1） = i 是后面有足够被选数字的极限条件，大于这个数之后，调用回溯必定失败
        for(int i = start; i <= n - k + 1; i++) {
            temp.add(i);
            help(temp, i + 1, k - 1);
            temp.removeLast();
        }
    }
}
```

