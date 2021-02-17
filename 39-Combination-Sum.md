# 39. Combination Sum

给出一组无重复的数字，求有多少种组合方法可以相加到目标值。经典DFS题。

DFS遍历每一个位置的数字时，可以选择不加入这个数字，或者加入这个数字。至于加几个，完全取决于目前累计和和目标值的差距。
+ 当累计和已经超过目标值的时候，停止搜索。当然，这种情况是不可能出现的，因为我们向组合内添加元素时，会保持累计和不大于目标值的。
+ 当累计和等于目标值的时候，我们就可以把这个组合给放入答案了。
+ 如果不等于累计和，但是当前已经遍历完所有候选元素了，也该返回。
+ 否则算出累计和与目标值的差能由几个当前元素构成，分别进行搜索。假如当前元素是5，差值是13。那么当前元素放0，1，2次都是可以的。

Time complexity: O((T/M)^(N+1)), N is the number of candidates, T is target value and M is the min value among the candidates. 分析一下，递归树最多只有N层，因为我们是根据候选数字的个数来决定递归栈有多少层的。每一层最多会有T/M个分支，所以答案个数是(T/M)^N量级的。每添加一个答案，会消耗T/M量级的复杂度，所以复杂度是(T/M)^(N+1)。

Space complexity: O(N + T/M), N层的调用栈，组合内元素个数是T/M量级的。

```java
class Solution {
  public List<List<Integer>> combinationSum(int[] candidates, int target) {
    List<List<Integer>> res = new ArrayList<>();
    if(candidates == null || candidates.length == 0){
      return res;
    }
    dfs(candidates, target, 0, new ArrayList<>(), 0, res);
    return res;
  }

  private void dfs(int[] candidates, int target, int level, List<Integer> comb, int accu, List<List<Integer>> res) {
    if (accu > target) return;
    if (accu == target) {
      res.add(new ArrayList<>(comb));
      return;
    }
    if (level >= candidates.length) return;

    int currNum = candidates[level];
    int n = (target - accu) / currNum;
    dfs(candidates, target, level + 1, comb, accu, res);
    for (int i = 0; i < n; i++) {
      comb.add(currNum);
      accu += currNum;
      dfs(candidates, target, level + 1, comb, accu, res);
    }
    for (int i = 0; i < n; i++) {
      comb.remove(comb.size() - 1);
    }
  }
}
```