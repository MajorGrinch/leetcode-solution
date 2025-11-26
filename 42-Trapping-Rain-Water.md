# 42. Trapping Rain Water

给一个柱状图，求出能盛放多少水。

最简单的思路，对每个位置都向左向右寻找最高点，然后左右最高点里较矮的那个就决定了这个位置能盛放多少水。按照这个方法，时间复杂度O(n^2)。有没有更好的方法来节省时间复杂度呢？

## DP Solution

最左最右的两个点是肯定没法盛水的，针对中间的每个点我们记录左右包括自身在内的最高点高度。leftPeak[i]是`[0, i]`里最高的高度，rightPeak[i]是`[i, n - 1]`里的最高高度。很显然，leftPeak[i]来自于leftPeak[i - 1]和height[i]里大的那个，rightPeak[i]来自于rightPeak[i + 1]和height[i]里大的那个。这样，两次遍历就可以求出每个点对应的左右最高高度。

最后我们再遍历一次，针对每个点拿到左右最高高度，取小的那个减去当前点的高度就是这个点能放水的容量。

Time complexity: O(n)

Space complexity: O(n)

```java
class Solution {
  public int trap(int[] height) {
    if (height == null || height.length <= 2) {
      return 0;
    }
    int n = height.length;
    int[] leftPeak = new int[n], rightPeak = new int[n];
    leftPeak[0] = height[0];
    rightPeak[n - 1] = height[n - 1];
    for (int i = 1; i < n - 1; i++) {
      leftPeak[i] = Math.max(leftPeak[i - 1], height[i]);
    }
    for (int i = n - 2; i >= 1; i--) {
      rightPeak[i] = Math.max(rightPeak[i + 1], height[i]);
    }
    int vol = 0;
    for (int i = 1; i < n - 1; i++) {
      vol += Math.min(leftPeak[i], rightPeak[i]) - height[i];
    }
    return vol;
  }
}
```

## Two Pointers Solution

动态规划方法需要额外的两个数组记录每个点左右的最高点，造成了O(n)的空间复杂度。有没有办法能够在空间上进行节省呢？

有。我们仔细分析一下动态规划的思路，每个点求出左右最高高度，取短板和自己的高度相减就是这个点能盛放的体积。也就是对于一个点`i`来说，它能盛放水的体积取决于`[0, i]`和`[i, n - 1]`这两侧最高高度的短板和自己高度的差值。

我们采用双指针`i`和`j`分别从两边向中间扫的思想，两边相向而行的过程中不断记录各自目前已经遇到的最高高度。故leftPeak记录的还是`[0, i]`最高高度，rightPeak记录的也还是`[j, len - 1]`的最高高度。

如果目前右边的最高高度比较高的话，那么左边就是短板，所以当前`i`能存放的水就取决于左边短板和`i`的高度差。如果左边目前的最高高度比较高的话，那么右边就是短板，当前`j`能存放水的体积就取决于右边目前最高高度和`j`的高度差。双指针相向而行，哪边是短板就让哪边往中间挪一步。

那么我们可能会有疑问，为什么哪边是短板就移动对应的指针呢？那另一边的指针对应点能存储水的体积该怎么办？我们举个例子来解释，因为左边是短板，所以我们只能确定`i`这个点目前能放多少水。因为右边最高高度高啊，`i`这个点能放的水肯定受制于左边的最高高度。那此时`j`点能放多少水呢？不确定。可能`j`左边会遇到一个更高的地方让`j`放`rightPeak - height[j]`的水，也有可能遇到一个更矮的地方让 j 存不了多少水。所以此时，我们只能确定`i`能放多少水并且我们移动`i`来试图更新左边最高点。同理，当右边成为短板时，我们也只能确定`j`能盛多少水。

Time complexity: O(n)

Space complexity: O(1)

```java
class Solution {
  public int trap(int[] height) {
    if (height == null || height.length <= 2) {
      return 0;
    }
    int leftPeak = 0, rightPeak = 0;
    int i = 0, j = height.length - 1;
    int vol = 0;
    while (i <= j) {
      leftPeak = Math.max(leftPeak, height[i]);
      rightPeak = Math.max(rightPeak, height[j]);
      if (leftPeak < rightPeak) {
        vol += leftPeak - height[i];
        i++;
      } else {
        vol += rightPeak - height[j];
        j--;
      }
    }
    return vol;
  }
}
```