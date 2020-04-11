#  290. Word Pattern 

题目都说双射了，那就至少2个散列表啦，空间时间O(N)。

```java
public boolean wordPattern(String pattern, String str) {
    String[] words = str.split(" ");
    if(words.length != pattern.length()) {
        return false;
    }
    //letter -> word
    HashMap<Character, String> ltow = new HashMap<>(words.length);
    //word -> letter
    HashMap<String, Character> wtol = new HashMap<>(words.length);

    for(int i = 0; i < words.length; i++) {
        String word = words[i];
        char letter = pattern.charAt(i);
        String wordForLetter = ltow.getOrDefault(letter, null);
        if(wordForLetter == null) {
            if(wtol.containsKey(word)) {
                return false;
            } else {
                ltow.put(letter, word);
                wtol.put(word, letter);
            }
        } else if(!wordForLetter.equals(word)) {
            return false;
        }
    }
    return true;
}
```

