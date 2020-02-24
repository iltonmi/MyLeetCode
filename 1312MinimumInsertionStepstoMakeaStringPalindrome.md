# 1312. Minimum Insertion Steps to Make a String Palindrome

 https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/discuss/470706/JavaC%2B%2BPython-Longest-Common-Sequence 

## **Intuition**

Split the string `s` into to two parts, and we try to make them symmetrical by adding letters.

The more common symmetrical subsequence they have, the less letters we need to add.

Now we change the problem to find the length of longest common sequence.
This is a typical dynamic problem.

## **Explanation**

**Step1.**
Initialize `dp[n+1][n+1]`,
where`dp[i][j]` means the length of longest common sequence between
`i` first letters in `s1` and `j` first letters in `s2`.

**Step2.**
Find the the longest common sequence between `s1` and `s2`,
where `s1 = s` and `s2 = reversed(s)`

**Step3.**
`return n - dp[n][n]`

## **Complexity**

Time `O(N^2)`
Space `O(N^2)`

等价于最长公共子序列。

```java
public int minInsertions(String s) {
    int n = s.length();
    int[][] dp = new int[n+1][n+1];
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            dp[i + 1][j + 1] = s.charAt(i) == s.charAt(n - 1 - j) 
                ? dp[i][j] + 1 : Math.max(dp[i][j + 1], dp[i + 1][j]);
        }
    }
    return n - dp[n][n];
}
```

