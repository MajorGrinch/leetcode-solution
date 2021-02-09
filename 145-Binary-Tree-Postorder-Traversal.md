# 145. Binary Tree Postorder Traversal

## Recursive Approach

二叉树后序遍历，递归实现版本。后序遍历，处理顺序就是先左子树，再右子树，最后再处理根节点。

Time complexity: O(N)

Space complexity: O(H), where H is the height.

```java
class Solution {

    private List<Integer> res;
    public List<Integer> postorderTraversal(TreeNode root) {
        res = new ArrayList<>();
        postorder(root);
        return res;
    }

    private void postorder(TreeNode root){
        if(root == null){
            return;
        }
        postorder(root.left);
        postorder(root.right);
        res.add(root.val);
    }
}
```