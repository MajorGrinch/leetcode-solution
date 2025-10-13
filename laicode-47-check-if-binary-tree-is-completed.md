# Laicode 47. Check If Binary Tree Is Completed

检查一下二叉树是否complete。

怎么定义complete？除了最后一行，其他所有行都是满节点，并且最后一行的节点都是从左开始连续的。

其实这题还是逐行扫描，和[102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/description/)核心思想是一样的。但是在扫描的时候，我们记录一个全局变量`metNull`，表示是否在二叉树里面遇到`null`节点了。根据题目的定义，我们可以推测出一旦遇到过`null`节点了，该二叉树后面所有的节点都必须为`null`。否则，就不符合要求。

```java
public class Solution {
  public boolean isCompleted(TreeNode root) {
    if(root == null) {
      return true;
    }
    Deque<TreeNode> levelQ = new ArrayDeque<>();
    levelQ.offer(root);
    boolean metNull = false;
    while(!levelQ.isEmpty()) {
      int size = levelQ.size();
      for(int i = 0; i < size; i++) {
        TreeNode curr = levelQ.poll();
        if(curr.left == null) {
          metNull = true;
        } else {
          if(metNull) {
            return false;
          }
          levelQ.offer(curr.left);
        }

        if(curr.right == null) {
          metNull = true;
        } else {
          if(metNull) {
            return false;
          }
          levelQ.offer(curr.right);
        }
      }
    }
    return true;
  }
}
```