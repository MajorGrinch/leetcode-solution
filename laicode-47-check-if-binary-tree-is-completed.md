# Laicode 47. Check If Binary Tree Is Completed

检查一下二叉树是否complete。

怎么定义complete？除了最后一行，其他所有行都是满节点，并且最后一行的节点都是从左开始连续的。

其实这题还是逐行扫描，和[102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/description/)核心思想是一样的。但是在扫描的时候，我们记录一个全局变量`metNull`，表示是否在二叉树里面遇到`null`节点了。

根据题目的定义，我们可以推测出一旦当前层的任意一个节点有子节点为`null`，那么这个节点同一层后面所有的节点都不可以有非`null`子节点。而且下一层所有节点必须得是叶子节点。那么我们可以抽象出这么个逻辑，只要遇到`null`了，后面按层遍历的时候就不可以遇到非`null`子节点。按照这个逻辑实现就可以了。

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