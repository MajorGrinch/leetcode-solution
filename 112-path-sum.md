# 112. Path Sum

给定一个二叉树，是否有一条root-to-leaf path，它的和等于 targetSum。

那么针对这个二叉树的递归，我们还是思考三件事情：
+ 我希望左右孩子给我什么？他们各自是否有到叶子节点等于 targetSum 的路径。
+ 我当前层需要做什么？判断当前node 是否是叶子节点，是的话就看当前node 值是否是 targetSum。不是叶子节点的话，就看它有没有到叶子节点的路径和等于 targetSum - node.val 的。
+ 我需要向上返回什么？返回我的左右孩子是否有路径到叶子节点，路径和是 targetSum - node.val。

```java
class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if (root == null) {
            return false;
        }
        if (isLeafNode(root)) {
            return root.val == targetSum;
        }

        return hasPathSum(root.left, targetSum - root.val) || hasPathSum(root.right, targetSum - root.val);
    }

    private boolean isLeafNode(TreeNode node) {
        return node.left == null && node.right == null;
    }
}
```

```java
class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if (root == null) {
            return false;
        }
        if (root.left == null && root.right == null) {
            return root.val == targetSum;
        }
        targetSum -= root.val;
        return hasPathSum(root.left, targetSum) || hasPathSum(root.right, targetSum);
    }
}
```