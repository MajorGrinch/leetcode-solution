# 4. Median of Two Sorted Arrays

可以利用[laicode 202. Kth Smallest In Two Sorted Arrays](laicode-202-Kth-Smallest-In-Two-Sorted-Arrays.md)这题的方法。当有奇数个元素的时候，中位数就是第`N/2 + 1`小。当偶数个的时候，就找到`N/2`和`N/2 + 1`小，然后取平均值。

Time complexity: O(logk)

Space complexity: O(logk)

```java
class Solution {
  public double findMedianSortedArrays(int[] nums1, int[] nums2) {
    int N = nums1.length + nums2.length;
    if (N % 2 == 1) {
      return kth(nums1, nums2, N / 2 + 1, 0, 0);
    } else {
      int val1 = kth(nums1, nums2, N / 2, 0, 0);
      int val2 = kth(nums1, nums2, N / 2 + 1, 0, 0);
      return (val1 + val2) / 2.0;
    }
  }

  private int kth(int[] nums1, int[] nums2, int k, int start1, int start2) {
    if (start1 >= nums1.length) {
      return nums2[start2 + k - 1];
    }
    if (start2 >= nums2.length) {
      return nums1[start1 + k - 1];
    }
    if (k == 1) {
      return Math.min(nums1[start1], nums2[start2]);
    }
    int midIndex1 = start1 + k / 2 - 1;
    int midIndex2 = start2 + k / 2 - 1;
    int midVal1 = midIndex1 >= nums1.length ? Integer.MAX_VALUE : nums1[midIndex1];
    int midVal2 = midIndex2 >= nums2.length ? Integer.MAX_VALUE : nums2[midIndex2];
    if (midVal1 < midVal2) {
      return kth(nums1, nums2, k - k / 2, midIndex1 + 1, start2);
    } else {
      return kth(nums1, nums2, k - k / 2, start1, midIndex2 + 1);
    }
  }
}
```