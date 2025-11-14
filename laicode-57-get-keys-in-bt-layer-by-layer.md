# Laicode 57. Get Keys In Binary Tree Layer By Layer

参照[leetcode 102](102-Binary-Tree-Level-Order-Traversal.md)。

```java
public class Solution {
  public List<List<Integer>> layerByLayer(TreeNode root) {
    List<List<Integer>> res = new ArrayList<>();
    if(root == null) {
      return res;
    }
    Queue<TreeNode> level = new ArrayDeque<>();
    level.offer(root);
    while(!level.isEmpty()) {
      int size = level.size();
      List<Integer> levelList = new ArrayList<>();
      for(int i = 0; i < size; i++) {
        TreeNode curr = level.poll();
        levelList.add(curr.key);
        if(curr.left != null) {
          level.offer(curr.left);
        }
        if(curr.right != null) {
          level.offer(curr.right);
        }
      }
      res.add(levelList);
    }
    return res;
  }
}
```