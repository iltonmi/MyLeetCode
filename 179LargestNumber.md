# 179. Largest Number

 https://leetcode.com/articles/largest-number/ 

>  sorting the problem example in descending order would produce the number 95343039534303, while the correct answer can be achieved by transposing the 33 and the 3030. 

转换为字符串，直接排序是不行的。

>  10a+b>10b+a, 10b+c>10c+b (a_b > b_a, b_c > c_b)
> so 10b+c+a>10c+b+a and 10a+b+c>10b+a+c 
> then 10a+b+c>10c+b+a
> so 10a+c>10c+a (a_c > c_a)

a_b > b_a则a 在 b之前，而且具有传递性。

```java
private static class LargerNumberComparator implements Comparator<String> {
    @Override
    public int compare(String a, String b) {
        String order1 = a + b;
        String order2 = b + a;
       return order2.compareTo(order1);
    }
}

public String largestNumber(int[] nums) {
    // Get input integers as strings.
    String[] asStrs = new String[nums.length];
    for (int i = 0; i < nums.length; i++) {
        asStrs[i] = String.valueOf(nums[i]);
    }

    // Sort strings according to custom comparator.
    Arrays.sort(asStrs, new LargerNumberComparator());

    // If, after being sorted, the largest number is `0`, the entire number
    // is zero.
    if (asStrs[0].equals("0")) {
        return "0";
    }

    // Build largest number from sorted array.
    StringBuilder res = new StringBuilder();
    for (String numAsStr : asStrs) {
        res.append(numAsStr);
    }

    return res.toString();
}
```

 