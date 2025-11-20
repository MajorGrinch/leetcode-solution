# Laicode 213. Reconstruct Binary Tree With Preorder And Inorder

参照[leetcode 105](105-Construct-BT-From-Preorder-And-Inorder.md)。

```java
public class Solution {
  public TreeNode reconstruct(int[] inOrder, int[] preOrder) {
    int len = inOrder.length;
    Map<Integer, Integer> inOrderIdxMap = new HashMap<>();
    for(int i = 0; i < len; i++) {
      inOrderIdxMap.put(inOrder[i], i);
    }
    return helper(inOrder, 0, len - 1, preOrder, 0, inOrderIdxMap);
  }

  private TreeNode helper(int[] inOrder, int il, int ir, int[] preOrder, int pl, Map<Integer, Integer> inOrderIdxMap) {
    if(il > ir) {
      return null;
    }
    if(il == ir) {
      return new TreeNode(inOrder[il]);
    }
    TreeNode node = new TreeNode(preOrder[pl]);
    int inorderIdx = inOrderIdxMap.get(node.key);
    node.left = helper(inOrder, il, inorderIdx - 1, preOrder, pl + 1, inOrderIdxMap);
    node.right = helper(inOrder, inorderIdx + 1, ir, preOrder, pl + inorderIdx - il + 1, inOrderIdxMap);
    return node;
  }
}
```