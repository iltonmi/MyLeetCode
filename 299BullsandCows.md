#  299. Bulls and Cows

题目描述不清晰。。。空间O(1)，时间(N)

```java
public String getHint(String secret, String guess) {
    int[] map = new int[10];
    for(int i = 0; i < secret.length(); i++) {
        map[secret.charAt(i) - '0']++;
    }
    int bull = 0;
    int cow = 0;
    for(int i = 0; i < secret.length(); i++) {
        char s = secret.charAt(i);
        char g = guess.charAt(i);
        if(s == g && map[s - '0'] > 0) {
            bull++;
            map[s - '0']--;
        }
    }
    for(int i = 0; i < secret.length(); i++) {
        char s = secret.charAt(i);
        char g = guess.charAt(i);
        if(s != g && map[g - '0'] > 0) {
            cow++;
            map[g - '0']--;
        }
    }
    return bull + "A" + cow + "B";
}
```

