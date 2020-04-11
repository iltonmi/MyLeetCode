#  345. Reverse Vowels of a String

双指针，空间O(1)，时间O(N)

```java
Set<Character> set = new HashSet<>(10) {
    {
        add('a');
        add('e');
        add('i');
        add('o');
        add('u');
        add('A');
        add('E');
        add('I');
        add('O');
        add('U');
    }
};

public String reverseVowels(String s) {
    char[] arr = s.toCharArray();
    int l = 0;
    int r = arr.length - 1;
    while(l < r) {
        while(l < r && !set.contains(arr[l])) {
            l++;
        }
        while(l < r && !set.contains(arr[r])) {
             r--;
         }
        if(l < r) {
            swap(arr, l++, r--);
        }
    }
    return new String(arr);
}

private void swap(char[] arr, int i, int j) {
    char temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}
```

