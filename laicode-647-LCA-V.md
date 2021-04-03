# Laicode 647. Lowest Common Ancestor V

给定一个K叉树，和两个节点，找到他们的最近公共祖先。

和找最近公共祖先的其他题目也差不多的思路。只不过是挨个遍历子节点，记录搜寻结果个数。如果搜寻结果大于1，说明当前节点是两个节点的最近公共祖先。否则，搜到什么返回什么。

这样最后我们就可以找到他们的最近公共祖先。

Time complexity: O(N)

Space complexity: O(H). H is height.

```java
public class Solution {
  public KnaryTreeNode lowestCommonAncestor(KnaryTreeNode root, KnaryTreeNode a, KnaryTreeNode b) {
    if (root == null || root == a || root == b) {
      return root;
    }
    KnaryTreeNode res = null;
    int count = 0;
    for (KnaryTreeNode child : root.children) {
      KnaryTreeNode searchRes = lowestCommonAncestor(child, a, b);
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