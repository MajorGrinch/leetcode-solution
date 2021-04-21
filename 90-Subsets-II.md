# 90. Subsets II

和[78. Subsets](78-Subsets.md)差不多，但是这里的元素有重复，但是题目要求子集不能有重复。

这里我们好好思考一下，假设数组里里有5个`1`，那么按照78题的思路，每个位置分用和不用两种情况，一定会导致有重复子集。比如第一个1选择用，后面的1选择不用这一情况，会和第二个1选择用，其余的1不用，这个情况重复。这两种情况都只会在子集里出现一个`1`。

当前的1一定会分用和不用这两种情况，用的时候，后面的1可以用，也可以不用。当不用的时候，后面的也一定不可以用，否则就会产生重复。所以当前元素不用的时候，后面所有相同元素也要跳过。其余的就和普通求子集一样了，不过要注意的是这题没说相同元素也会响铃，所以我们需要先sort一下。

Time complexity: O(2^n).

Space complexity: O(n), n level call stack for DFS and sort.

```java
class Solution {
  public List<List<Integer>> subsetsWithDup(int[] nums) {
    List<List<Integer>> res = new ArrayList<>();
    if (nums.length == 0) {
      return res;
    }
    Arrays.sort(nums);
    subsets(nums, 0, new ArrayList<>(), res);
    return res;
  }

  private void subsets(int[] nums, int level, List<Integer> subset, List<List<Integer>> res) {
    if (level == nums.length) {
      res.add(new ArrayList<>(subset));
      return;
    }
    subset.add(nums[level]);
    subsets(nums, level + 1, subset, res);
    subset.remove(subset.size() - 1);

    int nextLevel = level + 1;
    while (nextLevel < nums.length && nums[nextLevel] == nums[level]) nextLevel++;

    subsets(nums, nextLevel, subset, res);
  }
}
```