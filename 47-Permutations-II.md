# 47. Permutations II

和[46 Permutations](46-Permutation.md)差不多，但是这次给的数组里面会有重复元素，要求给出的全排列是无重复的。

有重复元素的话，比如题目的例子里`[1, 1, 2]`就只有三种无重复的全排列。那么其实也是和46差不多的思路，依然是dfs深搜。当前位置和后面每个位置交换，得到一种唯一的排列。但是这次要注意的是，我们用一个`Set`来记录当前位置已经放过哪些数字了。如果已经放过一个数字，再次遇见同样的数字时就跳过，这样就可以避免重复的全排列。

Time complexity: O(n! * n)，最多有`n!`个全排列，每记录一个排列需要`n`的复杂度。

Space complexity: (n^2)，n层递归树，虽然每一层的Set元素个数会逐渐变小，但基本遵循等差数列求和。故O(n^2)。

```java
class Solution {
  public List<List<Integer>> permuteUnique(int[] nums) {
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
      for (int n : nums) permutation.add(n);
      res.add(new ArrayList<>(permutation));
      return;
    }
    Set<Integer> used = new HashSet<>();
    for (int i = level; i < nums.length; i++) {
      if (!used.contains(nums[i])) {
        used.add(nums[i]);
        swap(nums, level, i);
        dfs(nums, level + 1, res);
        swap(nums, level, i);
      }
    }
  }

  private void swap(int[] nums, int i, int j) {
    int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
  }
}
```