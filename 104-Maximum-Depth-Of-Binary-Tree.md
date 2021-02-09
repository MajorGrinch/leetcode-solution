# 104. Maximum Depth of Binary Tree

计算二叉树的最深子树的深度。

用递归访问的方法，每一层解决如下问题，
+ 我希望左右子树返回什么？我希望他们各自返回以他们为`root`的子树的深度
+ 我当前层希望计算什么？我希望计算以我为根节点的子树的深度
+ 我希望向上返回什么？以我为根节点的子树深度

递归的计算左右子树深度，`root`为`null`时深度就是0，也是递归的base case。base case即能够让递归开始返回答案的情况。当前层得到左右子树各自的深度之后取最大的那个，加上1就是以当前节点为`root`的子树深度，返回给上层。这样，最后以真正`root`为根节点的深度就可以得到了。

Time complexity: O(N), because we have to access every nodes in the tree.

Space complexity: O(H), where H is the depth of the tree.

```java
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null){
            return 0;
        }
        int leftDepth = maxDepth(root.left);
        int rightDepth = maxDepth(root.right);
        return Math.max(leftDepth, rightDepth) + 1;
    }
}
```