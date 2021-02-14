# 145. Binary Tree Postorder Traversal

## Recursive Approach

二叉树后序遍历，递归实现版本。后序遍历，处理顺序就是先左子树，再右子树，最后再处理根节点。

Time complexity: O(N)

Space complexity: O(H), where H is the height.

```java
class Solution {

    private List<Integer> res;
    public List<Integer> postorderTraversal(TreeNode root) {
        res = new ArrayList<>();
        postorder(root);
        return res;
    }

    private void postorder(TreeNode root){
        if(root == null){
            return;
        }
        postorder(root.left);
        postorder(root.right);
        res.add(root.val);
    }
}
```

## Iterative Approach

递归方法会产生与树高同量级的调用栈，迭代法可以避免这额外的空间复杂度。迭代也有两种方法，一种比较常见的是反后序遍历，即`根右左`这样遍历一下，最后得到的答案reverse一下。这种方法没啥好说的，不过这里还有另一种方法。

我们用一个栈来保存遍历的路径，`prev`变量来存遍历过程中上一次访问的节点，以此来判断后序的操作。curr来记录当前访问的节点。总的来说，判断分三大类
+ prev为空，或者curr是prev的子树，说明要继续向下访问，因为curr的左右子树都还没处理。
+ prev是curr的左孩子，说明curr的左子树已经被处理完了。我们应该考虑去处理curr的右子树。
+ prev是curr的右孩子，说明curr的右子树已经被处理完了。我们要考虑处理curr。

先说第一种情况，prev为空，说明是才开始访问二叉树。curr是prev的子树时，说明左右子树都还没处理。这两种情况，我们都需要先把curr的左右子树先处理掉。处理的话，那自然是优先处理左子树，然后再考虑右子树。这里也继续分类，左子树不空的话，就左孩子进栈。否则，如果右子树不空，就右孩子进栈。否则，这是个叶子节点，直接处理它。

第二种情况即上一个访问的节点是curr的左孩子，既然上一个访问的是左孩子，说明curr的左子树已经被处理过了。那么下面我们就要去尝试处理右孩子。如果右孩子不为空，右孩子进栈。右孩子为空的话，那就直接处理当前节点就好了。

第三种情况，上一个访问的节点是curr的右孩子，说明右子树也被处理过了，那么就直接处理当前的节点。

最后，不要忘记更新prev，让它时刻保存的是我们上一次访问的节点。

```java
class Solution {
  public List<Integer> postorderTraversal(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    if (root == null) {
      return res;
    }
    Deque<TreeNode> stack = new ArrayDeque<>();
    TreeNode prev = null;
    stack.push(root);
    while (!stack.isEmpty()) {
      TreeNode curr = stack.peek();
      if (prev == null || prev.left == curr || prev.right == curr) {
        // prev is null or curr is prev's child
        if (curr.left != null) {
          stack.push(curr.left);
        } else if (curr.right != null) {
          stack.push(curr.right);
        } else {
          // curr is leaf
          res.add(curr.val);
          stack.pop();
        }
      } else if (prev == curr.left) {
        // left has been processed
        if (curr.right != null) {
          stack.push(curr.right);
        } else {
          res.add(curr.val);
          stack.pop();
        }
      } else if (prev == curr.right) {
        // right has been processed
        res.add(curr.val);
        stack.pop();
      }
      prev = curr;
    }
    return res;
  }
}
```