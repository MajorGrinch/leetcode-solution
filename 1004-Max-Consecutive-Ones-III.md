# 1004. Max Consecutive Ones III

给定一个只有0和1组成的数组，允许你最多把K个0变成1，找到最长的连续1子数组长度。

允许最多把K个0变成1，那也其实就是说找到最长的连续1的子数组，它里面最多可以有K个0。那么这题其实还是滑动窗口的问题。

`st`记录滑动窗口的起始位置，每轮循环结束时我都要`[st, i]`里面不超过K个0。`count0`用来记录当前窗口里面0的个数，如果`A[i]`是0的话，`count0`就自增。下面开始维护，如果`count0`已经比k大了，而且st就往后一直走，走到count0不大于k为止。不过这里还有个隐藏条件，就是st也不能一直往后走，我们当然不希望st越界，所以限定st最多走到i后面一位。

为什么最多走到i后面一位呢？因为如果k为0的话，那么其实就是子数组不允许有0。那么如果当前`A[i]`为0的话，那么st当然要从`i + 1`开始。维护好性质之后，我们可以知道`[st, i]`最多k个0，拿目前的窗口长度和之前的最大值比较并更新。

Time complexity: O(N), we access each character at most twice.

Space complexity: O(1).

```java
class Solution {
  public int longestOnes(int[] A, int K) {
    int st = 0, maxL = 0;
    int count0 = 0;
    for (int i = 0; i < A.length; i++) {
      if (A[i] == 0) {
        count0++;
      }
      while(count0 > K && st <= i){
        if(A[st] == 0){
          count0--;
        }
        st++;
      }
      maxL = Math.max(maxL, i - st + 1);
    }
    return maxL;
  }
}
```