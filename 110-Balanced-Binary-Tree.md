# 110. Balanced Binary Tree

判断一个数是否是平衡二叉树，平衡二叉树判断条件——所有节点的左右子树高度相差不大于1。

这里用递归的方法来判断，递归的时候三个问题，
+ 我希望左右孩子给我什么？以左右孩子为根节点的子树如果是平衡树的话，我希望返回平衡树的高度。如果不是平衡树的话，返回`-1`给我。
+ 当前层想要干什么？判断以当前节点为根节点的子树是否是平衡树。如果左右子树有一个不是平衡树，那我自身也不是平衡树。只有当左右子树都是平衡树，且高度差不大于1，我自己也才是平衡树。
+ 我想要向上返回什么？如果以当前节点为根节点的子树是平衡树的话，返回高度。不是的话，返回`-1`。

这样的话，最后看`root`节点返回的是高度还是`-1`。返回`-1`的话，说明这个数不是平衡树。返回高度的话，说明这个数是平衡树。

Time complexity: O(N)

Space complexity: O(H), where H is the height.

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        return getHeight(root) == -1 ? false : true;
    }

    private int getHeight(TreeNode root){
        if(root == null){
            return 0;
        }
        int leftHeight = getHeight(root.left);
        if(leftHeight == -1){
            return -1;
        }
        int rightHeight = getHeight(root.right);
        if(rightHeight == -1){
            return -1;
        }
        if(Math.abs(leftHeight - rightHeight) > 1){
            return -1;
        }
        return Math.max(leftHeight, rightHeight) + 1;
    }
}
```