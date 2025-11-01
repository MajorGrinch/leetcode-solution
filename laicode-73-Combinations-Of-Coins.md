# Laicode 73. Combinations Of Coins

和[Laicode 404](laicode-404-factor-combinations.md)差不多。

和[leetcode 39](39-Combination-Sum.md)也差不多，只是答案的表现性质稍有区别。我们这里用一个和`coins`等长的List来表明每个面额需要多少个才能组成`target`，所以每个答案的长度都是和coins等长的。

Time complexity: O(N * (T/M)^N). N is the number of denominations, T is target value and M is the min value among the denominations.

Space complexity: O(N), n level call stack and n size combination. Result space is not counted.

```java
public class Solution {
  public List<List<Integer>> combinations(int target, int[] coins) {
    List<List<Integer>> res = new ArrayList<>();
    if (coins == null || coins.length == 0) {
      return res;
    }
    List<Integer> comb = new ArrayList<>();
    for (int i = 0; i < coins.length; i++) {
      comb.add(0);
    }
    dfs(coins, 0, target, comb, res);
    return res;
  }

  private void dfs(int[] coins, int level, int left, List<Integer> comb, List<List<Integer>> res) {
    if (level == coins.length) {
      if (left == 0) {
        res.add(new ArrayList<>(comb));
      }
      return;
    }
    int deno = coins[level];
    int num = left / deno;
    for (int i = 0; i <= num; i++) {
      comb.set(level, i);
      dfs(coins, level + 1, left - i * deno, comb, res);
      comb.set(level, 0);
    }
  }
}
```

2025 code:

```java
public class Solution {
  public List<List<Integer>> combinations(int target, int[] coins) {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> comb = new ArrayList<>();
    for(int i = 0; i < coins.length; i++) {
      comb.add(0);
    }
    dfs(target, coins, 0, comb, res);
    return res;
  }

  private void dfs(int target, int[] coins, int index, List<Integer> comb, List<List<Integer>> res) {
    if(target == 0) {
      res.add(new ArrayList<>(comb));
      return;
    }
    if(index == coins.length) {
      return;
    }

    dfs(target, coins, index + 1, comb, res);
    int count = 0;
    int coin = coins[index];
    while(target >= 0 && coin <= target) {
      count++;
      comb.set(index, comb.get(index) + 1);
      target -= coin;
      dfs(target, coins, index + 1, comb, res);
    }

    while(count > 0) {
      comb.set(index, comb.get(index) - 1);
      target += coin;
      count--;
    }
  }
}
```