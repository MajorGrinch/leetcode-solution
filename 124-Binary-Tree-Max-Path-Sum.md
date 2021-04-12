# 124. Binary Tree Maximum Path Sum

给定一个二叉树找到里面最大的路径和。

这题和[laicode 138. Maximum Path Sum Binary Tree I](laicode-138-Maximum-Path-Sum-Binary-Tree-I.md)相似，只不过这题路径起点和终点可以是二叉树内部任意点。那么这题就更简单了！

二叉树上的递归还是三件事情：
+ 我希望左右孩子给我什么？以他们为起点的最大路径和
+ 我当前节点需要做什么？计算经过我的最大路径和，并更新全局答案
+ 我需要向上返回什么？返回以我为起点向下的最大路径和

因为这里的路径起点和终点可以是任意点，所以代码实现上需要注意的就没那么多了。从左右孩子那里各自拿到以他们为起点向下的最大路径和，先和0比，取大的。然后计算经过当前节点的最大路径和，并试图更新全局最大路径和。最后向上返回以当前节点向下的最大路径和。最后我们就可以得到全局最大的路径和。

Time complexity: O(N).

Space complexity: O(H).

```java
class Solution {
  public int maxPathSum(TreeNode root) {
    int[] res = new int[] {Integer.MIN_VALUE};
    maxPathToLeaf(root, res);
    return res[0];
  }

  private int maxPathToLeaf(TreeNode root, int[] res) {
    if (root == null) {
      return 0;
    }
    int leftMaxPath = Math.max(maxPathToLeaf(root.left, res), 0);
    int rightMaxPath = Math.max(maxPathToLeaf(root.right, res), 0);
    int currMaxPath = leftMaxPath + root.val + rightMaxPath;
    if (currMaxPath > res[0]) {
      res[0] = currMaxPath;
    }
    return Math.max(leftMaxPath, rightMaxPath) + root.val;
  }
}
```