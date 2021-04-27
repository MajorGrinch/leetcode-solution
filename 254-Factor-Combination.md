# 254. Factor Combinations

给定一个数n，返回所有可能的因数分解结果。

经典的DFS题目。我们先得到`n`所有的因数，然后每个因数为一层进行dfs。base case是所有因数均遍历完了。如果正好分解结束，就把当前的因数分解组合加入答案。不管是否分解完，因数遍历完了都需要返回。

对于每一层的因数来说，我们先针对不用该因数的情况进行深搜，再对用这个因数的情况进行深搜。用这个因数又分用几次，遍历每一种情况进行深搜直到不可分为止。返回前，记得恢复状态。

Time complexity: hard to say.

Space complexity: depends on how many factors does n have.


```java
class Solution {
  public List<List<Integer>> getFactors(int n) {
    List<List<Integer>> res = new ArrayList<>();
    if (n <= 2) {
      return res;
    }
    List<Integer> factorList = getAllFactors(n);
    dfs(n, factorList, 0, new ArrayList<>(), res);
    return res;
  }

  private void dfs(
      int n, List<Integer> factorList, int factorIdx, List<Integer> comb, List<List<Integer>> res) {
    if (factorIdx == factorList.size()) {
      if (n == 1) res.add(new ArrayList<>(comb));
      return;
    }

    dfs(n, factorList, factorIdx + 1, comb, res);

    int factor = factorList.get(factorIdx);
    int count = 0;
    while (n % factor == 0) {
      count++;
      comb.add(factor);
      n /= factor;
      dfs(n, factorList, factorIdx + 1, comb, res);
    }
    for (int i = 0; i < count; i++) comb.remove(comb.size() - 1);
  }

  private List<Integer> getAllFactors(int n) {
    List<Integer> factorList = new ArrayList<>();
    for (int i = 2; i <= n / 2; i++) {
      if (n % i == 0) factorList.add(i);
    }
    return factorList;
  }
}
```