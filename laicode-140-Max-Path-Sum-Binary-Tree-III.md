# Laicode 140. Maximum Path Sum Binary Tree III

给定一个二叉树，找出直上直下的路径的最大和。路劲的起点终点不限，只要是直上直下的路径即可。

那么这题和[laicode 639. Max Path Sum From Leaf To Root](laicode-639-Max-Path-Sum-From-Leaf-To-Root.md)很像，不过这题路径起点和终点不限。所以我们在实现的时候就没有必要非要碰到叶子节点才去试图更新答案，而是每下一层就去试图更新答案。然后往下层调用的时候，先把目前为止的路径和与0比较。如果比0小，那只会拖累路径和的计算，就直接斩断，传0作为下层调用的prefixSum。如果比0大，那么就可以给路径和做贡献，就传给下层用。

对左右孩子进行调用，一直到最后。这样，我们就可以得到整棵树最大的直上直下路径和。

Time complexity: O(N).

Space complexity: O(H).

```java
public class Solution {
  public int maxPathSum(TreeNode root) {
    int[] res = new int[] {Integer.MIN_VALUE};
    maxPathSum(root, 0, res);
    return res[0];
  }

  private void maxPathSum(TreeNode root, int prefixSum, int[] res) {
    if (root == null) {
      return;
    }
    res[0] = Math.max(res[0], prefixSum + root.key);
    prefixSum += root.key;
    prefixSum = Math.max(0, prefixSum);
    maxPathSum(root.left, prefixSum, res);
    maxPathSum(root.right, prefixSum, res);
  }
}
```

2025 code:

```java
public class Solution {
  public int maxPathSum(TreeNode root) {
    int[] res = {Integer.MIN_VALUE};
    dfs(root, 0, res);
    return res[0];
  }

  private void dfs(TreeNode node, int currSum, int[] res) {
    currSum += node.key;
    res[0] = Math.max(res[0], currSum);
    currSum = Math.max(currSum, 0);
    if(node.left != null) {
      dfs(node.left, currSum, res);
    }
    if(node.right != null) {
      dfs(node.right, currSum, res);
    }
  }
}
```