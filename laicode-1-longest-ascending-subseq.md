# Laicode 1. Longest Ascending Subsequence

参照[leetcode 300](300-Longest-Increasing-Subsequence.md)。

```java
public class Solution {
  public int longest(int[] array) {
    int n = array.length;
    if(n <= 1) {
      return n;
    }
    int[] lis = new int[n];
    lis[0] = 1;
    int res = 1;
    for(int i = 1; i < n; i++) {
      int local = 0;
      for(int j = 0; j < i; j++) {
        if(array[j] < array[i]) {
          local = Math.max(local, lis[j]);
        }
      }
      lis[i] = local + 1;
      res = Math.max(res, lis[i]);
    }
    return res;
  }
}
```

```java
public class Solution {
  public int longest(int[] array) {
    int n = array.length;
    if(n <= 1) {
      return n;
    }
    int[] lasLenEnd = new int[n + 1];
    lasLenEnd[1] = array[0];
    int resLen = 1;
    for(int i = 1; i < n; i++) {
      int lasLenEndWithI = largestSmaller(lasLenEnd, 1, resLen, array[i]) + 1;
      lasLenEnd[lasLenEndWithI] = array[i];
      resLen = Math.max(resLen, lasLenEndWithI);
    }
    return resLen;
  }

  private int largestSmaller(int[] array, int l, int r, int target) {
    while(r - l > 1) {
      int mid = l + (r - l) / 2;
      if(array[mid] >= target) {
        r = mid - 1;
      } else {
        l = mid;
      }
    }
    if(array[r] < target) {
      return r;
    } else if(array[l] < target) {
      return l;
    } else {
      return l - 1;
    }
  }
}
```