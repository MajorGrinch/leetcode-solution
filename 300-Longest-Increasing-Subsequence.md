# 300. Longest Increasing Subsequence

给定一个数组，求出最长的升序序列。注意，是序列不是子数组。

## Dynamic Programming Approach

简单的动态规划可以用一个`LIS`数组来记录以每个下标为结尾的最长升序序列，那么`LIS[i] = MAX(LIS[j]) + 1, j < i && nums[j] < nums[i]`。按照这个思路，我们就可以得到以每个下标为结尾的最长升序序列，过程中记录最长的那个长度就好。

Time complexity: O(n^2)

Space complexity: O(n)

```java
class Solution {
  public int lengthOfLIS(int[] nums) {
    if (nums.length <= 1) {
      return nums.length;
    }

    int n = nums.length;
    int[] LIS = new int[n];
    LIS[0] = 1;
    int res = 1;
    for (int i = 1; i < n; i++) {
      int localLongest = 0;
      for (int j = 0; j < i; j++) {
        if (nums[j] < nums[i]) localLongest = Math.max(localLongest, LIS[j]);
      }
      localLongest++;
      res = Math.max(res, localLongest);
      LIS[i] = localLongest;
    }
    return res;
  }
}
```

## Binary Search Approach

此方法较为晦涩难懂，可不做掌握。

用二分法的思想来解决。用`lowestEnding`数组来记录目前各个长度的升序序列对应的最小结尾值，`lowest[len] = val`表示目前长度为`len`的升序序列结尾最小的值是`val`。比方说，目前我总共遇到了2个长度为4的升序序列，一个是`[1,2,5,8]`，另一个是`[1,2,4,5]`。那么`lowest[4] = 5`。

我们首先实现一个`largestSmaller(int[] array, int start, int end, int target)`方法，在给定的`array[start, end]`区间里找到比`target`小的最大值的下标。如果没有比`target`小的，就返回`start - 1`。

输入有n个数，初始化`lowestEnding`长度为`n+1`，因为最长升序序列的长度有可能是`n`。用`res`来记录目前遇到的最长升序序列。一开始，长度为1的升序序列结尾最小值当然就是`nums[0]`。接着我们从`nums`的下标1开始遍历到最后，用二分法从`lowestEnding[1, res]`中找到比`nums[i]`小的最大值len。这时候我就知道`nums[i]`前面有一个长度为`len`的升序序列结尾值是比`nums[i]`要小的，那么以`nums[i]`结尾的最长升序序列长度自然就是`len + 1`。

我们再把`lowestEnding[len + 1]`置为`nums[i]`，因为`lowestEnding[len + 1]`必定是因为要么比`nums[i]`大要么超出`lowestEnding[1, res]`的范围，所以二分法才返回`len`的。两种情况，我们都可以直接把`lowestEnding[len + 1]`置为`nums[i]`。`res`和`len + 1`取大的赋值给`res`，以记录目前遇到的最长的升序序列长度。

Time complexity: O(nlogn)

Space complexity: O(n)

```java
class Solution {
  public int lengthOfLIS(int[] nums) {
    if (nums.length <= 1) {
      return nums.length;
    }

    int n = nums.length;
    int[] lowestEnding = new int[n + 1];
    lowestEnding[1] = nums[0];
    int res = 1;
    for (int i = 1; i < n; i++) {
      int len = largestSmaller(lowestEnding, 1, res, nums[i]);
      lowestEnding[len + 1] = nums[i];
      res = Math.max(res, len + 1);
    }
    return res;
  }

  private int largestSmaller(int[] array, int start, int end, int target) {
    int l = start, r = end;
    while (r - l > 1) {
      int mid = l + (r - l >> 1);
      if (array[mid] < target) {
        l = mid;
      } else {
        r = mid - 1;
      }
    }
    if (array[r] < target) {
      return r;
    } else if (array[l] < target) {
      return l;
    } else {
      return start - 1;
    }
  }
}
```

2025 code:

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        if (n <= 1) {
            return n;
        }
        int[] lisEndValue = new int[n + 1];
        lisEndValue[1] = nums[0];
        int resLen = 1;
        for (int i = 1; i < n; i++) {
            int lisLenEndWithI = largestSmaller(lisEndValue, 1, resLen, nums[i]) + 1;
            lisEndValue[lisLenEndWithI] = nums[i];
            resLen = Math.max(resLen, lisLenEndWithI);
        }
        return resLen;
    }

    private int largestSmaller(int[] nums, int l, int r, int target) {
        while (r - l > 1) {
            int mid = l + (r - l) / 2;
            if (nums[mid] < target) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        if (nums[r] < target) {
            return r;
        } else if (nums[l] < target) {
            return l;
        } else {
            return l - 1;
        }
    }
}
```