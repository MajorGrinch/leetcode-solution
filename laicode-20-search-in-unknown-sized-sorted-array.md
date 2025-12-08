# Laicode 20. Search In Unknown Sized Sorted Array



```java
public class Solution {
  public int search(Dictionary dict, int target) {
    int l = 0;
    int r = 1;
    while(dict.get(r) != null && dict.get(r) < target) {
      l = r;
      r *= 2;
    }
    while(l <= r) {
      int mid = l + (r - l) / 2;
      if(dict.get(mid) == null) {
        r = mid - 1;
      } else if(dict.get(mid) == target) {
        return mid;
      } else if(dict.get(mid) < target) {
        l = mid + 1;
      } else {
        r = mid - 1;
      }
    }
    return -1;
  }
}
```