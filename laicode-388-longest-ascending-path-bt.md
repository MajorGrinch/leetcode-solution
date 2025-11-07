# Laicode 388. Longest Ascending Path Binary Tree

给定一个二叉树，找到最长的升序路径，要求路径必须是上下方向。

其实就是最基本的二叉树访问，prevKey 记录父节点的数值，prevLen 记录以父节点为终点的最长升序路径，res 记录全局最大值。

当前节点如果比父节点数值大，那么父节点所在的升序路径可以把当前节点也拓展进去。当前节点如果没有父节点大，那么直接就重新开始。

Time complexity: O(N)

Space complexity: O(logN)

```java
public class Solution {
  public int longest(TreeNode root) {
    int[] res = {0};
    helper(root, 0, 0, res);
    return res[0];
  }

  private void helper(TreeNode node, int prevKey, int prevLen, int[] res) {
    if(node == null) {
      return;
    }
    int len = node.key > prevKey ? prevLen + 1 : 1;
    res[0] = Math.max(res[0], len);
    helper(node.left, node.key, len, res);
    helper(node.right, node.key, len, res);
  }
}
```