# 114. Flatten Binary Tree to Linked List

给定一个二叉树，以先序遍历的顺序把它in-place铺平为链表。

那这题就是把原来的二叉树完全变成只有右孩子的一个二叉树，让它看起来像是链表一样。我们如果按照先序遍历的顺序来实现的话，左孩子变成右孩子之后，那轮到处理右孩子的时候怎么办？右孩子已经被左孩子覆盖了。所以我们不能以先序的顺序来处理，我们需要用反先序的顺序即`右左根`。用一个变量来记录上一次处理的节点，先右后左递归调用。按照这顺序，上一次处理的一定是当前节点先序遍历的下一个节点，所以直接让它作为当前节点的右孩子，当前节点的左孩子直接置为空。因为如果有左孩子肯定已经被放去右边了，就不用担心。最后更新上一次处理的节点就行。

Time complexity: O(N).

Space complexity: O(H).

```java
class Solution {
  public void flatten(TreeNode root) {
    flatten(root, new TreeNode[1]);
  }

  private void flatten(TreeNode root, TreeNode[] prev) {
    if (root == null) return;
    flatten(root.right, prev);
    flatten(root.left, prev);
    root.right = prev[0];
    root.left = null;
    prev[0] = root;
  }
}
```