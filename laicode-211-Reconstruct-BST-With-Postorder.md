# Laicode 211. Reconstruct Binary Search Tree With Postorder Traversal

给定一个BST的后序遍历数组，重建这个树。

后序遍历的话，那么子树的根节点永远是在最后的。所以我们可以从后往前遍历，然后优先建立右子树，再去建立左子树。全局用一个`rootIndex`来表明当前子树的root所在位置，每次建立之后就把它向前推进一个位置。但是我们如何界定左右子树他们的边界在哪儿呢？我们用一个min来标明。`min`记录了当前子树里允许的最小值，也就是说如果`rootIndex`往前推进的途中遇到了比传进来的`min`还小的节点，那么这时候这个子树就已经结束了，我们需要返回null。

递归调用建立右子树的时候，传入的`min`就是这个子树根的值加1。递归调用建立左子树是，把上层传进来的`min`原封不动的传进去。

Time complexity: O(N).

Space complexity: O(H).

```java
public class Solution {
  public TreeNode reconstruct(int[] post) {
    return reconstruct(post, new int[] {post.length - 1}, Integer.MIN_VALUE);
  }

  private TreeNode reconstruct(int[] post, int[] rootIndex, int min) {
    if (rootIndex[0] < 0 || post[rootIndex[0]] < min) {
      return null;
    }
    TreeNode root = new TreeNode(post[rootIndex[0]--]);
    root.right = reconstruct(post, rootIndex, root.key + 1);
    root.left = reconstruct(post, rootIndex, min);
    return root;
  }
}
```