# Laicode 141. Binary Tree Path Sum To Target III

给定一个二叉树，是否存在直上直下的路径和的值与target相等。

那么这题其实也没那么复杂，我们依然是自顶向下遍历。遍历途中，我们时刻检查是否存在一条以当前节点为终点的路径和为target。所以我们不仅需要记录当前节点的前缀和，还需要记录当前路径之前各节点对应的前缀和。这样我们就可以用当前层的前缀和减去target的方法，配合Set来快速检索。如果不存在的话，那么就继续对左右孩子进行遍历，并且把当前节点的前缀和给放入Set。如果左右孩子任意传回true，说明他们那里有对应的路径，当前节点也就返回true。如果都没有，就返回false。

需要注意的是，因为Set里可能已经存在当前的前缀和了，比如当前节点为0，所以我们需要记录`add`的返回值。true的话表示Set里没有这个值，添加成功，回头返回的时候也需要移除掉。false的话表示Set里已经有这个值了，添加失败，回头就没必要删掉，上一层会删掉的。

Time complexity: O(N).

Space complexity: O(H).

```java
public class Solution {
  public boolean exist(TreeNode root, int target) {
    Set<Integer> prefixSumSet = new HashSet<>();
    prefixSumSet.add(0);
    return exist(root, target, 0, prefixSumSet);
  }

  private boolean exist(TreeNode root, int target, int prefixSum, Set<Integer> prefixSumSet) {
    if (root == null) {
      return false;
    }
    prefixSum += root.key;
    if (prefixSumSet.contains(prefixSum - target)) {
      return true;
    }
    boolean needRemoval = prefixSumSet.add(prefixSum);
    if (exist(root.left, target, prefixSum, prefixSumSet)
        || exist(root.right, target, prefixSum, prefixSumSet)) {
      return true;
    }
    if (needRemoval) {
      prefixSumSet.remove(prefixSum);
    }
    return false;
  }
}
```