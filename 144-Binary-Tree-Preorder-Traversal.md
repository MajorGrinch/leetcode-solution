# 144. Binary Tree Preorder Traversal

## Recursive Approach

二叉树先序遍历，递归实现。

Time complexity: O(N)

Space complexity: O(H), where H is the height of the binary tree.

```java
class Solution {

    private List<Integer> res = new ArrayList<>();

    public List<Integer> preorderTraversal(TreeNode root) {
        preorder(root);
        return res;
    }

    private void preorder(TreeNode root){
        if(root == null){
            return;
        }
        res.add(root.val);
        preorder(root.left);
        preorder(root.right);
    }
}
```

## Iterative Approach

二叉树先序遍历，迭代实现。

由于先序遍历的顺序是根左右这一性质，即为先访问根节点，再访问左子树最后再访问右子树。所以我们需要栈来存储下一个需要访问的节点，每当访问一个节点时先把右子树的根，即右孩子给先压入栈，这样程序就会后处理它。左子树的根，即左孩子后入栈，这样我们会优先处理左孩子。等左子树全都处理完了，栈会弹出右孩子来处理右子树。

Time complexity: O(N)

Space complexity: O(H), where H is the height of binary tree.

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if(root == null){
            return res;
        }
        Deque<TreeNode> stack = new ArrayDeque<>();
        stack.push(root);
        while(!stack.isEmpty()){
            TreeNode curr = stack.pop();
            res.add(curr.val);
            if(curr.right != null){
                stack.push(curr.right);
            }
            if(curr.left != null){
                stack.push(curr.left);
            }
        }
        return res;
    }
}
```