# 46. Permutations

给定一个无重复元素的数组，得到它的全排列。这题也是经典的DFS题目，我们针对每个位置进行枚举，既然是全排列，那么按照我们以前高中学全排列的思路，第一个位置可以放n个元素，后面可以放剩下的n-1个，直到最后。

体现在代码里就是，当前位置可以和包括自己在内的后面所有元素进行交换，然后继续枚举下一个位置。当枚举到数组结尾时，我们就把目前的排列给加入答案。那么其实就是，0号位置可以放n个元素，1号位置可以放n-1个元素，直到最后的位置。和全排列的语义是一样的，所以最后我们可以枚举所有的全排列并加入答案。

Time complexity: O(N! * N), `N!`是全排列的答案个数，N是元素个数。因为添加答案的时候，需要逐个添加，所以会耗费额外的N复杂度。

Space complexity: O(N), N层调用栈。

```java
class Solution {
  public List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> res = new ArrayList<>();
    if (nums == null) {
      return res;
    }
    dfs(nums, 0, res);
    return res;
  }

  private void dfs(int[] nums, int level, List<List<Integer>> res) {
    if (level == nums.length) {
      List<Integer> permutation = new ArrayList<>();
      for(int n : nums) permutation.add(n);
      res.add(permutation);
      return;
    }
    for (int i = level; i < nums.length; i++) {
      swap(nums, level, i);
      dfs(nums, level + 1, res);
      swap(nums, level, i);
    }
  }

  private void swap(int[] nums, int i, int j) {
    int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
  }
}
```