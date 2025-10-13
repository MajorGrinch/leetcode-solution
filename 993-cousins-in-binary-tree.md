# 993. Cousins in Binary Tree

检查给定的两个值在二叉树中是否是表兄弟。所谓表兄弟，就是这两个节点在同一层，但是父节点不一样。

这题我们采用先序遍历，并且沿途记录父节点和层数。如果遇到了任意一个值，那就记录该值所在的层数和父节点，并且停止继续向下层搜寻。为什么不向下层搜寻了？因为没有意义，即使下层有另一个值，那他俩肯定不满足表兄弟的关系了。

Time complexity: O(N)，因为可能需要搜寻所有的节点。

Space complexity: O(H)，H是二叉树层数。二叉树有多少层，我们的调用栈就有可能需要多少层。

```java
class Solution {
    TreeNode parentX;
    TreeNode parentY;
    int depthX = -1;
    int depthY = -1;

    public boolean isCousins(TreeNode root, int x, int y) {
        preOrder(root, x, y, null, 0);
        if(depthX == -1 || depthY == -1) {
            return false;
        }
        return depthX == depthY && parentX != parentY;
    }

    private void preOrder(TreeNode curr, int x, int y, TreeNode parent, int depth) {
        if(curr == null) {
            return;
        }
        if(curr.val == x) {
            this.depthX = depth;
            parentX = parent;
            return;
        }
        if(curr.val == y) {
            this.depthY = depth;
            parentY = parent;
            return;
        }
        preOrder(curr.left, x, y, curr, depth + 1);
        preOrder(curr.right, x, y, curr, depth + 1);
    }
}
```