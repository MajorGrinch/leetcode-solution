这题要求找有序数组里最靠近target值的坐标。

assumption:
1. 数组为空怎么办？-1
2. 数组是否含有重复元素？有。如果答案有多个，随意返回哪个都可以。
3. 数组是否可能没有target？当然可能。

这题可以看做leetcode 35的变种，本质上可以理解为找到第一个大于等于target的坐标。

找到之后那个数如果正好是target，那就直接返回。如果不是，就看它和前一个元素谁和target更近。



```java
public class Solution {
    public int closest(int[] array, int target) {
      if(array == null || array.length == 0) return -1;

      int l = 0;
      int r = array.length - 1;
      while(r - l > 1) {
        int mid = l + (r-l) / 2;
        int val = array[mid];
        if(val == target) {
          return mid;
        } else if (val < target) {
          l = mid;
        } else {
          // val > targer
          r = mid;
        }
      }

      if(Math.abs(target - array[l]) < Math.abs(target - array[r])) {
        return l;
      } else {
        return r;
      }
    }
}

public class Solution {
    public int closest(int[] array, int target) {
      if(array == null || array.length == 0) return -1;
      if( target < array[0]) return 0;
      if(array[array.length - 1] < target) return array.length - 1;

      int l = 0;
      int r = array.length - 1;
      while(l < r) {
        int mid = l + (r-l) / 2;
        int val = array[mid];
        if(target < val) {
          r = mid;
        } else {
          // val <= target
          l = mid + 1;
        }
      }

      if(array[l] == target) {
        return l;
      }
      // l == r. compare l - 1 with l
      if(Math.abs(target - array[l]) < Math.abs(target - array[l - 1])) {
        return l;
      } else {
        return l - 1;
      }
    }
}
```