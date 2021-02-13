# 450. Delete Node in a BST

## Recursive Approach

题目要求从BST里删去指定值的节点，**并且最后要返回的是这个树的根节点**，而不是被删除的节点。

根据BST的性质，如果该节点存在的话，我们可以logN时间内定位到该节点。关键在于怎么从BST里删除这个节点，以及具体实现该怎么写。

假设我们在树里找到了该节点，这时候就应该分类讨论一下，
+ 如果它的左子树是空的话，那么直接把它的右子树给父节点当子树
+ 如果它的右子树是空的话，那么直接把它的左子树给父节点当子树
+ 如果它左右子树皆非空，那么我们需要找到这个节点在中序遍历的下一个节点来替代它。为什么是中序遍历的下一个节点？为了维护BST的性质。替代完之后，我们将该新的子树返回给父节点替换掉之前的。

以上三种情况都需要向父节点返回新的子树以替代原来的子树才能达到删除节点的目的。所以我们在向下搜索的过程中，是需要更新子树的。比如，当前节点值比目标值要小，所以我们肯定要去右子树里面找到并删除。右子树删完之后根可能不变也可能改变，因此无论右子树删完之后有没有改变，我们都返回右子树删完之后的新的根节点，用来给当前节点重新做右子树。

所以`TreeNode deleteNode(TreeNode root, int key)`的语义就是，给定一个以`root`为根节点的树，找到值为`key`的节点删除后，返回这个树。在一步步向下搜索中，这个树会越来越小直到我们找到那个节点。删除这个节点，并且返回新的子树给父节点。父节点用这个来替代原来的子树，并且把自己返回给自己的父节点，作为替换掉新的子树，直到我们最初的根节点，返回给外层调用。

Time complexity: O(logN)

Space complexity: O(H), where H is the height.

```java
class Solution {
  public TreeNode deleteNode(TreeNode root, int key) {
    if (root == null) {
      return root;
    }
    if (root.val == key) {
      if (root.left == null) {
        root = root.right;
      } else if (root.right == null) {
        root = root.left;
      } else {
        root.val = getNextInOrder(root).val;
        root.right = deleteNode(root.right, root.val);
      }
    } else if (root.val < key) {
      root.right = deleteNode(root.right, key);
    } else {
      root.left = deleteNode(root.left, key);
    }
    return root;
  }

  /** Given a non-null node of a binary tree, return its next inorder node. */
  private TreeNode getNextInOrder(TreeNode curr) {
    curr = curr.right;
    while (curr.left != null) {
      curr = curr.left;
    }
    return curr;
  }
}
```

## Iterative Approach

递归的方法会产生和树高相同量级的调用栈，从而产生相应的空间复杂度，这是在二叉树上使用递归思路不可避免的问题。不过递归方法，比较直观，方便理解，代价就是空间复杂度。而迭代方法有时候代码比较复杂，但是相应的空间复杂度可以降低到常数级。

这题用迭代的思路，我们可以先找到需要删的那个节点，以及它的父节点。对于被删除的那个节点来说，这相当于把以它为根节点的BST给删去根节点，所以我们实现一个方法——给定一个BST，删除它的根节点，并返回新的根节点。

删掉节点之后，把新的子树给父节点当子树就好了。不过这里注意，父节点有可能为空，因为有可能整棵树的root正好是要删的节点。那么这时候，直接删整棵树的root。

Time complexity: O(logN)

Space complexity: O(1)

```java
class Solution {
  public TreeNode deleteNode(TreeNode root, int key) {
    if (root == null) {
      return root;
    }

    TreeNode prev = null, curr = root;
    while (curr != null && curr.val != key) {
      prev = curr;
      if (curr.val < key) {
        curr = curr.right;
      } else {
        curr = curr.left;
      }
    }
    if (curr == null) {
      // not found
      return root;
    }
    if(prev == null){
      // root is the key node
      return deleteBSTRoot(root);
    }
    if(prev.left == curr){
      prev.left = deleteBSTRoot(curr);
    }else {
      prev.right = deleteBSTRoot(curr);
    }
    return root;
  }

  private TreeNode deleteBSTRoot(TreeNode root){
    if(root == null){
      return root;
    }
    if(root.left == null){
      return root.right;
    }else if(root.right == null){
      return root.left;
    }
    TreeNode nextInOrder = getNextInOrder(root);
    nextInOrder.left = root.left;
    return root.right;
  }

  /** Given a non-leaf node of a binary tree, return its next inorder node. */
  private TreeNode getNextInOrder(TreeNode curr) {
    curr = curr.right;
    while (curr.left != null) {
      curr = curr.left;
    }
    return curr;
  }
}
```