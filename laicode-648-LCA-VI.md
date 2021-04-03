# Laicode 648. Lowest Common Ancestor VI

给定一个K叉树，和M个节点，找到他们的最近公共祖先。

这题是公共祖先的最通用的一题。思路其实也差不多，检查当前节点，如果为null或者出现在给定节点之中，那么直接返回当前节点给上层作为搜索结果。

如果上一步没有返回，那么我们对当前节点的每一个孩子进行进一步搜索调用。搜索出来的结果不为null，那就记录下来并且count自增。count大于1的时候，说明当前节点是多个节点的公共祖先，就向上返回当前节点作为搜索结果。如果最后count不大于1，那么就返回搜索结果，可能为null也可能是唯一那个搜到的结果。

Time complexity: O(N)

Space complexity: O(max(H, M)). H is height.

```java
public class Solution {
  public KnaryTreeNode lowestCommonAncestor(KnaryTreeNode root, List<KnaryTreeNode> nodes) {
    return lowestCommonAncestor(root, new HashSet<>(nodes));
  }

  private KnaryTreeNode lowestCommonAncestor(KnaryTreeNode root, Set<KnaryTreeNode> nodeSet) {
    if (root == null || nodeSet.contains(root)) {
      return root;
    }
    int count = 0;
    KnaryTreeNode res = null;
    for (KnaryTreeNode child : root.children) {
      KnaryTreeNode searchRes = lowestCommonAncestor(child, nodeSet);
      if (searchRes != null) {
        count++;
        res = searchRes;
      }
      if (count > 1) {
        return root;
      }
    }
    return res;
  }
}
```