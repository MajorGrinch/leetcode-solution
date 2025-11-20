# 106. Construct Binary Tree from Inorder and Postorder Traversal

给定一个二叉树的中序遍历和后序遍历数组，把它还原出来。

这题其实和[leetcode 105](105-Construct-BT-From-Preorder-And-Inorder.md)类似，那题是先序，这里是后序，都差不多的。

```java
class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        Map<Integer, Integer> inorderIdxMap = new HashMap<>();
        for (int i = 0; i < inorder.length; i++) {
            inorderIdxMap.put(inorder[i], i);
        }
        return helper(inorder, 0, inorder.length - 1, postorder, postorder.length - 1, inorderIdxMap);
    }

    private TreeNode helper(int[] inorder, int il, int ir, int[] postorder, int pr,
            Map<Integer, Integer> inorderIdxMap) {
        if (il > ir) {
            return null;
        }
        if (il == ir) {
            return new TreeNode(inorder[il]);
        }
        TreeNode node = new TreeNode(postorder[pr]);
        int inorderNodeIdx = inorderIdxMap.get(node.val);
        node.right = helper(inorder, inorderNodeIdx + 1, ir, postorder, pr - 1, inorderIdxMap);
        node.left = helper(inorder, il, inorderNodeIdx - 1, postorder, pr - 1 - ir + inorderNodeIdx, inorderIdxMap);
        return node;
    }
}
```