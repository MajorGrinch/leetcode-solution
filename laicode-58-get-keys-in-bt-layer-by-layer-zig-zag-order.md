# Laicode 58. Get Keys In Binary Tree Layer By Layer Zig-Zag Order

参照[leetcode 103](103-Binary-Tree-Zigzag-Level-Order-Traversal.md)。

```java
public class Solution {
  public List<Integer> zigZag(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    if(root == null) {
      return res;
    }
    boolean towardsRight = false;
    Deque<TreeNode> levelQ = new ArrayDeque<>();
    levelQ.offer(root);
    while(!levelQ.isEmpty()) {
      int size = levelQ.size();
      for(int i = 0; i < size; i++) {
        if(towardsRight) {
          TreeNode curr = levelQ.pollFirst();
          res.add(curr.key);
          if(curr.left != null) {
            levelQ.offerLast(curr.left);
          }
          if(curr.right != null) {
            levelQ.offerLast(curr.right);
          }
        } else {
          TreeNode curr = levelQ.pollLast();
          res.add(curr.key);
          if(curr.right != null) {
            levelQ.offerFirst(curr.right);
          }
          if(curr.left != null) {
            levelQ.offerFirst(curr.left);
          }
        }
      }
      towardsRight = !towardsRight;
    }
    return res;
  }
}
```