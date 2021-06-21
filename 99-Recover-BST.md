# 99. Recover Binary Search Tree

给定一个二叉搜索树，其中有两个节点对调过了，找出他们并且换回来。

既然是有两个节点对调了一下，那么中序遍历下必然会出现一对或者两对前项大于后项的情况。中序遍历这个BST，途中我们用两个数组来记录元素。`prev[0]`记录上一次访问的元素，`swap[0]`记录首个中序下前项大于后项的那个前项，`swap[1]`记录则记录那个后项，`swap[2]`记录第二次该情况的后项。最后`swap[0]`要么和`swap[1]`对调，要么和`swap[2]`对调，取决于`swap[2]`为不为空。

Time complexity: O(N)

Space complexity: O(H)

```java
class Solution {
  public void recoverTree(TreeNode root) {
    TreeNode[] swap = new TreeNode[3];
    findSwapped(root, new TreeNode[1], swap);
    if (swap[2] == null) {
      swapNodeValue(swap[0], swap[1]);
    } else {
      swapNodeValue(swap[0], swap[2]);
    }
  }

  private void findSwapped(TreeNode root, TreeNode[] prev, TreeNode[] swap) {
    if (root == null) {
      return;
    }
    findSwapped(root.left, prev, swap);
    if (prev[0] != null && prev[0].val > root.val) {
      if (swap[0] == null) {
        swap[0] = prev[0];
        swap[1] = root;
      } else {
        swap[2] = root;
      }
    }
    prev[0] = root;
    findSwapped(root.right, prev, swap);
  }

  private void swapNodeValue(TreeNode n1, TreeNode n2) {
    int temp = n1.val;
    n1.val = n2.val;
    n2.val = temp;
  }
}
```