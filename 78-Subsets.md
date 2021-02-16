# 78 Subsets

给定一个无重复数字的数组，输出它所有的子集。

经典深度优先搜索题，DFS。既然是所有子集，那么数组的每一个元素都有两种情况，计入或者不计入，所以总共会有2^n个答案。

dfs到一层的时候，当前层的元素无非就是进入当前子集，或者不进入当前子集。level到达数组末尾的时候，把当前的子集放入答案，然后返回。注意加入答案时要新建ArrayList，因为java传参的传值，传的是这个子集的地址值。如果不新建ArrayList，已经放入答案的子集会被后续的遍历给修改掉。

Time complexity: O(2^n)

Space complexity: O(n), because of n level call stack. Also, the size of result is not taken into consideration.

```java
class Solution {
  public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> res = new ArrayList<>();
    if(nums == null){
      return res;
    }
    dfs(nums, 0, new ArrayList<>(), res);
    return res;
  }

  private void dfs(int[] nums, int level, List<Integer> subset, List<List<Integer>> res){
    if(level == nums.length){
      res.add(new ArrayList<>(subset));
      return;
    }

    subset.add(nums[level]);
    dfs(nums, level + 1, subset, res);
    subset.remove(subset.size() - 1);
    dfs(nums, level + 1, subset, res);
  }
}
```