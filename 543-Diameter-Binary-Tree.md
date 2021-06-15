# 543. Diameter of Binary Tree

给定一个二叉树，测定它里面的最长路径并输出路径里边的个数。路径可以不经过root节点。

~~我来鹅城只干三件事。~~递归的思路，还是思考三个事情：
+ 我希望左右孩子返回什么？他们各自到叶子节点的最长路径节点数
+ 我当前层需要做什么？计算经过我这个节点的最长路径是否是全局最长
+ 我需要向上层返回什么？我这个节点到底下叶子节点的最长路径节点数

这样我们就可以计算出全局最长的路径里的节点个数，减去1就是边的数量。

Time complexity: O(N)

Space complexity: O(H)

```java
class Solution {
  public int diameterOfBinaryTree(TreeNode root) {
    int[] res = {0};
    diameterOfBinaryTree(root, res);
    return res[0] - 1;
  }

  private int diameterOfBinaryTree(TreeNode root, int[] res) {
    if (root == null) {
      return 0;
    }
    int leftLen = diameterOfBinaryTree(root.left, res);
    int rightLen = diameterOfBinaryTree(root.right, res);
    res[0] = Math.max(res[0], leftLen + rightLen + 1);
    return Math.max(leftLen, rightLen) + 1;
  }
}
```