# Laicode 404. Factor Combinations

思路参照[leetcode 254](254-Factor-Combination.md).

```java
public class Solution {
  public List<List<Integer>> combinations(int target) {
    List<List<Integer>> res = new ArrayList<>();
    if(target <= 2) {
      return res;
    }
    List<Integer> factors = getAllFactors(target);
    dfs(target, factors, 0, new ArrayList<>(), res);
    return res;
  }

  private void dfs(int target, List<Integer> factors, int index, List<Integer> comb, List<List<Integer>> res) {
    if(target == 1) {
      res.add(new ArrayList<>(comb));
      return;
    }
    if(index == factors.size()) {
      return;
    }

    dfs(target, factors, index + 1, comb, res);

    int factor = factors.get(index);
    int count = 0;
    while(target > 1 && target % factor == 0) {
      comb.add(factor);
      target /= factor;
      dfs(target, factors, index + 1, comb, res);
      count++;
    }

    for(int i = 0; i < count; i++) {
      comb.remove(comb.size() - 1);
    }
  }

  private List<Integer> getAllFactors(int target) {
    List<Integer> factorList = new ArrayList<>();
    for(int i = 2; i <= target / 2; i++) {
      if(target % i == 0) {
        factorList.add(i);
      }
    }
    return factorList;
  }
}
```