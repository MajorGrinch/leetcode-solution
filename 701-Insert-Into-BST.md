# 701. Insert into a Binary Search Tree

## Recursive Approach

往BST里插入新的节点，插入后还得保持树还是BST。

要想插入新节点，那么就得先确定要插入那儿。按照BST的性质，
+ 如果当前节点值小于目标值，那么目标值肯定要插到右子树里
+ 如果当前节点值大于目标值，那么目标值肯定要插到左子树里
+ 当前节点值不可能等于目标值，题干声明了

base case是子树为空，那么val肯定就插到这里，并返回这个单节点的子树。如果如果当前节点值小于目标值，那么目标值插入左子树。这里就对左子树进行同样的方法调用，继续去找适合的位置。另一种情况，亦如此。由于递归调用，所以会产生和二叉树高度同等量级的调用栈，空间复杂度和树高一个量级。

Time complexity: O(logN)

Space complexity: O(H), where H is the height.

```java
class Solution {
  public TreeNode insertIntoBST(TreeNode root, int val) {
    if (root == null) {
      root = new TreeNode(val);
    } else if (root.val < val) {
      root.right = insertIntoBST(root.right, val);
    } else {
      root.left = insertIntoBST(root.left, val);
    }
    return root;
  }
}
```

## Iterative Approach

以上递归方法会产生与树高同量级的调用栈，这里我们还可以用迭代法。运用同样的思路，找到新节点应该放置的位置的父节点，然后最后通过父节点插入新节点。

Time complexity: O(logN)

Space complexity: O(1)

```java
class Solution {
  public TreeNode insertIntoBST(TreeNode root, int val) {
    TreeNode newNode = new TreeNode(val);
    if(root == null){
      return newNode;
    }
    TreeNode prev = null, curr = root;
    while (curr != null) {
      prev = curr;
      if (curr.val < val) {
        curr = curr.right;
      } else {
        curr = curr.left;
      }
    }
    if (prev.val > val) {
      prev.left = newNode;
    } else {
      prev.right = newNode;
    }
    return root;
  }
}
```
