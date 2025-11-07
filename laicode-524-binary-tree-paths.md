# Laicode 524. Binary Tree Paths

参照[leetcode 257](257-binary-tree-paths.md)。

```java
public class Solution {
  public String[] binaryTreePaths(TreeNode root) {
    List<List<Integer>> pathList = new ArrayList<>();
    helper(root, new ArrayList<>(), pathList);
    String[] res = new String[pathList.size()];
    for(int i = 0; i < res.length; i++) {
      res[i] = pathToString(pathList.get(i));
    }
    return res;
  }

  private void helper(TreeNode node, List<Integer> path, List<List<Integer>> pathList) {
    if(node == null) {
      return;
    }
    path.add(node.key);
    if(isLeaf(node)) {
      pathList.add(new ArrayList<>(path));
      path.remove(path.size() - 1);
      return;
    }
    helper(node.left, path, pathList);
    helper(node.right, path, pathList);
    path.remove(path.size() - 1);
  }

  private boolean isLeaf(TreeNode node) {
    return node.left == null && node.right == null;
  }

  private String pathToString(List<Integer> path) {
    StringBuilder sb = new StringBuilder();
    for(int i = 0; i < path.size(); i++) {
      sb.append(path.get(i));
      if(i != path.size() - 1) {
        sb.append("->");
      }
    }
    return sb.toString();
  }
}
```