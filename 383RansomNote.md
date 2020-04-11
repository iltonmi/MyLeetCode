#  383. Ransom Note

集邮。空间O(1)，时间O(N)

```java
public boolean canConstruct(String ransomNote, String magazine) {
    int[] map = new int[26];

    for(char ch : magazine.toCharArray()){
        map[ch-'a']++;
    }

    for(char ch : ransomNote.toCharArray()){
        if(--map[ch -'a'] == -1){
            return false;
        }
    }

    return true;
}
```

