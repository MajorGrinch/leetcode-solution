# Laicode 127. Lowest Common Ancestor II

参照[leetcode 1650](1650-LCA-of-Binary-Tree-III.md)。

```java
public class Solution {
  public TreeNodeP lowestCommonAncestor(TreeNodeP one, TreeNodeP two) {
    int depth1 = getDepth(one);
    int depth2 = getDepth(two);
    if(depth1 > depth2) {
      return findLCA(one, two, depth1 - depth2);
    } else {
      return findLCA(two, one, depth2 - depth1);
    }
  }

  private TreeNodeP findLCA(TreeNodeP one, TreeNodeP two, int diff) {
    while(diff-- > 0) {
      one = one.parent;
    }
    while(one != two) {
      one = one.parent;
      two = two.parent;
    }
    return one;
  }

  private int getDepth(TreeNodeP node) {
    if(node == null) {
      return 0;
    }
    int depth = 0;
    while(node != null) {
      node = node.parent;
      depth++;
    }
    return depth;
  }
}
```

这样写也行。
```java
public class Solution {
  public TreeNodeP lowestCommonAncestor(TreeNodeP one, TreeNodeP two) {
    int depth1 = getDepth(one);
    int depth2 = getDepth(two);
    int diff = depth1 - depth2;
    return diff > 0 ? findLCA(one, two, diff) : findLCA(two, one, -diff);
  }

  /**
    Assume one is deeper
  */
  private TreeNodeP findLCA(TreeNodeP one, TreeNodeP two, int diff) {
    while(diff > 0) {
      one = one.parent;
      diff--;
    }
    while(one != two) {
      one = one.parent;
      two = two.parent;
    }
    return one;
  }

  private int getDepth(TreeNodeP node) {
    int depth = 0;
    while(node != null) {
      node = node.parent;
      depth++;
    }
    return depth;
  }
}
```