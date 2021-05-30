# 491. Increasing Subsequences

给定一个数组，返回所有有两个及以上元素的升序序列。

用DFS的思想，找出所有的升序序列，怎么找就不赘述了。不过这里我们要注意去重，因为题目要求输出不同的升序序列。所以同一层搜索的时候，我们用`Set`记录下已经用过的数字，之后的搜索中需要跳过。

如何记录所有两个及以上元素的升序序列呢？我们在搜索的途中记录。举个例子`[4,6,7]`和`[4,6,8]`这两个序列肯定都是由`[4,6]`这个序列衍生来的，所以我们在搜索到当前序列为`[4,6]`的时候直接把它记录下来，然后再向下搜索并衍生出之后的两个序列。

Time complexity: O(n * 2^n), like find subsets.

Space complexity: O(n).

```java
class Solution {
  public List<List<Integer>> findSubsequences(int[] nums) {
    List<List<Integer>> res = new ArrayList<>();
    if (nums.length <= 1) return res;
    findSubsequences(nums, 0, new ArrayList<>(), res);
    return res;
  }

  private void findSubsequences(
      int[] nums, int index, List<Integer> subseq, List<List<Integer>> res) {
    if (subseq.size() > 1) {
      res.add(new ArrayList<>(subseq));
    }
    if (index == nums.length) return;
    Set<Integer> used = new HashSet<>();
    for (int i = index; i < nums.length; i++) {
      if (used.contains(nums[i])) continue;
      if (subseq.size() > 0 && nums[i] < subseq.get(subseq.size() - 1)) continue;

      used.add(nums[i]);
      subseq.add(nums[i]);
      findSubsequences(nums, i + 1, subseq, res);
      subseq.remove(subseq.size() - 1);
    }
  }
}
```