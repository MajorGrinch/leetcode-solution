# Laicode 401. Search In Bitonic Array

给一个先递增再递减的数组，找到指定的数。

assumption:
+ 找不到就返回-1
+ 数组不为null

思路：用三次二分法，第一次找peak，第二次在递增区间找，第三次在递减区间找。

```java
public class Solution {
  public int search(int[] array, int target) {
    // Write your solution here
    if(array == null || array.length == 0) return -1;
    int l = 0;
    int r = array.length - 1;
    while(l < r) {
      int mid = l + (r - l) / 2;
      if(array[mid] < array[mid + 1]) {
        l = mid + 1;
      } else {
        // array[mid] > array[mid + 1]
        r = mid;
      }
    }
    int peak = l;

    l = 0;
    r = peak;
    while(l <= r) {
      int mid = l + (r - l) / 2;
      if(array[mid] == target) {
        return mid;
      } else if (array[mid] < target){
        l = mid + 1;
      } else {
        r = mid - 1;
      }
    }

    l = peak + 1;
    r = array.length - 1;

    while(l <= r) {
      int mid = l + (r - l) / 2;
      if(array[mid] == target) {
        return mid;
      } else if(array[mid] < target) {
        r = mid - 1;
      } else {
        l = mid + 1;
      }
    }

    return -1;
  }
}
```