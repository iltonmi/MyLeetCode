# 252. Meeting Room

集邮。

```java
public boolean canAttendMeetings(int[][] intervals) {
    Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
    int preEnd = -1;
    for(int[] meeting : intervals) {
        if(preEnd > meeting[0]) {
            return false;
        } else {
            preEnd = meeting[1];
        }
    }
    return true;
}
```

