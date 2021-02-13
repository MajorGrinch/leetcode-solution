# Laicode 52. Search In Binary Search Tree

## Recursive Approach

根据BST的性质，当前节点值比目标值小就去右子树搜索，当前节点值比目标值大就去左子树搜索。BST搜索，其实就是二分搜索，时间复杂度就是logN。因为递归会造成调用栈，所以空间复杂度是和树的高度一个量级。

Time complexity: O(logN)

Space complexity: O(H), where H is the height.

```java
public class Solution {
  public TreeNode search(TreeNode root, int key) {
    if (root == null) {
      return null;
    }
    if (root.key == key) {
      return root;
    }
    return root.key < key ? search(root.right, key) : search(root.left, key);
  }
}
```

## Iterative Approach

以上是递归方法，这里介绍迭代法，中心思想是不变的。迭代法只用定义常数级的额外变量，所以空间复杂度降下来了。

Time complexity: O(logN)

Space complexity: O(1)


```java
public class Solution {
  public TreeNode search(TreeNode root, int key) {
    TreeNode curr = root;
    while (curr != null && curr.key != key) {
      if (curr.key < key) {
        curr = curr.right;
      } else {
        curr = curr.left;
      }
    }
    return curr;
  }
}
```