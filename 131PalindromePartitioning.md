# 131. Palindrome Partitioning 

All backtracking problems are composed by these three steps: `choose`, `explore`, `unchoose`.
So for each problem, you need to know:

1. `choose what?` For this problem, we choose each substring.
2. `how to explore?` For this problem, we do the same thing to the remained string.
3. `unchoose` Do the opposite operation of choose.

Let's take this problem as an example:
`1.Define helper()`: Usually we need a helper funcition in backtracking problem, to accept more parameters.
`2.Parameters`: Usually we need the following parameters

```java
    1. The object you are working on:  For this problem is String s.
    2. A start index or an end index which indicate which part you are working on: For this problem, we use substring to indicate the start index.
    3. A step result, to remember current choose and then do unchoose : For this problem, we use List<String> step.
    4. A final result, to remember the final result. Usually when we add, we use 'result.add(new ArrayList<>(step))' instead of 'result.add(step)', since step is reference passed. We will modify step later, so we need to copy it and add the copy to the result;
```

`3.Base case`: The base case defines when to add step into result, and when to return.
`4.Use for-loop`: Usually we need a for loop to iterate though the input String s, so that we can choose all the options.
`5.Choose`: In this problem, if the substring of s is palindrome, we add it into the step, which means we choose this substring.
`6.Explore`: In this problem, we want to do the same thing to the remaining substring. So we recursively call our function.
`7.Un-Choose`: We draw back, remove the chosen substring, in order to try other options.

```java
The above is mainly the template, the code is shown below:
```

```java
public List<List<String>> partition(String s) {
    // Backtracking
    // Edge case
    if(s == null || s.length() == 0) return new ArrayList<>();

    List<List<String>> result = new ArrayList<>();
    helper(s, new ArrayList<>(), result);
    return result;
}
public void helper(String s, List<String> step, List<List<String>> result) {
    // Base case
    if(s == null || s.length() == 0) {
        result.add(new ArrayList<>(step));
        return;
    }
    for(int i = 1; i <= s.length(); i++) {
        String temp = s.substring(0, i);
        if(!isPalindrome(temp)) continue; // only do backtracking when current string is palindrome

        step.add(temp);  // choose
        helper(s.substring(i, s.length()), step, result); // explore
        step.remove(step.size() - 1); // unchoose
    }
    return;
}
public boolean isPalindrome(String s) {
    int left = 0, right = s.length() - 1;
    while(left <= right) {
        if(s.charAt(left) != s.charAt(right))
            return false;
        left ++;
        right --;
    }
    return true;
}
```

空间O(N)，时间O(N * (2 ^ N))，用马拉车算法可以将时间压缩到O(2 ^ N)。