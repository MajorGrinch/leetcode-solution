# 250. Count Univalue Subtree

二叉树里有多少个具有相同元素的子树？

按照题目要求，所有叶子节点都是满足该条件的子树。还有什么？如果一个子树的非空左右子树都满足该条件，且非空左右子树和根的值都相等，那么这个根所在的子树也满足。接下来就是二叉树的标准三件套：
+ 我希望左右子树返回给我什么？他们各自是否是相同元素子树。
+ 我当前层需要计算什么？按照之前的条件，当前节点的子树是否满足。满足的话，全局答案+1。
+ 我需要向上返回什么？我这个节点的子树是否满足该条件。

```java
class Solution {
    public int countUnivalSubtrees(TreeNode root) {
        int[] res = { 0 };
        helper(root, res);
        return res[0];
    }

    private boolean helper(TreeNode node, int[] res) {
        if (node == null) {
            return true;
        }
        boolean left = helper(node.left, res);
        boolean right = helper(node.right, res);
        boolean isUni = left && right;
        if (node.left != null) {
            isUni &= node.val == node.left.val;
        }
        if (node.right != null) {
            isUni &= node.val == node.right.val;
        }
        if (isUni) {
            res[0]++;
        }
        return isUni;
    }
}
```