#  278. First Bad Version 

二分查找的变形，空间O(1)，时间O(logN)

```java
public int firstBadVersion(int n) {
    int l = 1;
    int r = n;
    while(l < r) {
        int mid = l + (r - l) / 2;
        if(isBadVersion(mid)) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return r;
}
```

