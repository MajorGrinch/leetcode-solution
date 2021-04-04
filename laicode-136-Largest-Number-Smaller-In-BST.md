# Laicode 136. Largest Number Smaller In Binary Search Tree

给定一个二叉搜索树，找到比target小的最大值。如若没有，就返回-2^31。

那么初始化答案为-2^31，然后沿着BST向下搜索。如果遇到节点值小于target，那么更新答案并且向右子树搜索。如果大于或者等于target，那么就向左子树搜索。

Time complexity: O(H). H is the height.

Space complexity: O(1).

```java
public class Solution {
  public int largestSmaller(TreeNode root, int target) {
    int res = Integer.MIN_VALUE;
    TreeNode curr = root;
    while(curr != null){
      if(curr.key < target){
        res = curr.key;
        curr = curr.right;
      }else{
        curr = curr.left;
      }
    }
    return res;
  }
}
```