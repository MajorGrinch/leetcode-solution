# 1650. Lowest Common Ancestor of a Binary Tree III

这题也是找LCA，但是二叉树每个节点都知道自己的父节点。所以我们可以先找到p和q各自的深度，然后从较深的那个开始出发到和较浅的那个齐平。齐平之后，两个点同时出发向上走直至相遇，就是他们最近的公共祖先。

Time complexity: O(H). H is height.

Space complexity: O(1).

```java
class Solution {
  public Node lowestCommonAncestor(Node p, Node q) {
    int pDepth = getDepth(p);
    int qDepth = getDepth(q);
    if (pDepth < qDepth) {
      return findLCA(p, q, qDepth - pDepth);
    } else {
      return findLCA(q, p, pDepth - qDepth);
    }
  }

  private Node findLCA(Node shallow, Node deep, int diff) {
    for (int i = 0; i < diff; i++) deep = deep.parent;
    while (shallow != deep) {
      shallow = shallow.parent;
      deep = deep.parent;
    }
    return shallow;
  }

  private int getDepth(Node p) {
    if (p == null) return 0;
    int depth = 0;
    while (p != null) {
      p = p.parent;
      depth++;
    }
    return depth;
  }
}
```