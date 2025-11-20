# Laicode 214. Reconstruct Binary Tree With Postorder And Inorder

参照[leetcode 106](106-construct-bt-from-inorder-and-postorder-traversal.md)。

```java
public class Solution {
  public TreeNode reconstruct(int[] inOrder, int[] postOrder) {
    Map<Integer, Integer> inOrderIdxMap = new HashMap<>();
    for(int i = 0; i < inOrder.length; i++) {
      inOrderIdxMap.put(inOrder[i], i);
    }
    return helper(inOrder, 0, inOrder.length - 1, postOrder, postOrder.length - 1, inOrderIdxMap);
  }

  private TreeNode helper(int[] inOrder, int il, int ir, int[] postOrder, int pr, Map<Integer, Integer> inOrderIdxMap) {
    if(il > ir) {
      return null;
    }
    if(il == ir) {
      return new TreeNode(inOrder[il]);
    }
    TreeNode node = new TreeNode(postOrder[pr]);
    int inOrderNodeIdx = inOrderIdxMap.get(node.key);
    node.right = helper(inOrder, inOrderNodeIdx + 1, ir, postOrder, pr - 1, inOrderIdxMap);
    node.left = helper(inOrder, il, inOrderNodeIdx - 1, postOrder, pr -1 - ir + inOrderNodeIdx, inOrderIdxMap);
    return node;
  }
}
```