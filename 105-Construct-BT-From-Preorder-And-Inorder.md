# 105. Construct Binary Tree from Preorder and Inorder Traversal

给定一个二叉树的先序后中序遍历数组，重建这个二叉树。

这题我们可以用递归的方式来解决。先序遍历的特征就是根节点肯定在最开始，而中序遍历的特征则是根节点被左右子树夹着。那么我们用`pStart`来标记子树的根节点在先序遍历中的位置，`iLeft`和`iRight`分别标记子树再中序遍历下的范围。

很显然，`preorder[pStart]`就是子树的根节点值。我们根据这个值到`inorder[iLeft...iRight]`里面去找到根节点的位置`iRoot`。为了快速查找，我们可以实现建立一个中序遍历下每个节点值对应在哪个下标的Map。

`inorder[iLeft, iRoot - 1]`就是左子树的中序遍历结果，`inorder[iRoot + 1, iRight]`就是右子树的中序遍历结果。同时，我们也就知道左子树到底有多少个节点了。那么在先序遍历里，左子树的根是`preorder[pStart + 1]`，而右子树根的位置则可以通过之前得到的左子树尺寸来推测出来，递归的向下去建立子树。

base case分两种情况，一个是中序缩减到空，那么返回null。另一个则是中序缩减到只有一个元素，那么直接建好节点并返回。

Time complexity: O(N).

Space complexity: O(N). HashMap.

```java
class Solution {
  public TreeNode buildTree(int[] preorder, int[] inorder) {
    Map<Integer, Integer> inorderIdxMap = new HashMap<>();
    for (int i = 0; i < inorder.length; i++) {
      inorderIdxMap.put(inorder[i], i);
    }
    return buildTree(preorder, inorder, 0, 0, inorder.length - 1, inorderIdxMap);
  }

  private TreeNode buildTree(
      int[] preorder,
      int[] inorder,
      int pStart,
      int iLeft,
      int iRight,
      Map<Integer, Integer> inorderIdxMap) {
    if (iLeft > iRight) {
      return null;
    }
    if (iLeft == iRight) {
      return new TreeNode(inorder[iLeft]);
    }
    int rootVal = preorder[pStart];
    TreeNode root = new TreeNode(rootVal);
    int iRoot = inorderIdxMap.get(rootVal);
    root.left = buildTree(preorder, inorder, pStart + 1, iLeft, iRoot - 1, inorderIdxMap);
    root.right =
        buildTree(preorder, inorder, pStart + 1 + iRoot - iLeft, iRoot + 1, iRight, inorderIdxMap);
    return root;
  }
}
```

2025 code:

```java
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        Map<Integer, Integer> inorderIdxMap = new HashMap<>();
        for (int i = 0; i < inorder.length; i++) {
            inorderIdxMap.put(inorder[i], i);
        }
        return helper(preorder, 0, inorder, 0, inorder.length - 1, inorderIdxMap);
    }

    private TreeNode helper(int[] preorder, int pl, int[] inorder, int il, int ir,
            Map<Integer, Integer> inorderIdxMap) {
        if (il > ir) {
            return null;
        }
        if (il == ir) {
            return new TreeNode(inorder[il]);
        }
        TreeNode node = new TreeNode(preorder[pl]);
        int inorderIdx = inorderIdxMap.get(node.val);
        node.left = helper(preorder, pl + 1, inorder, il, inorderIdx - 1, inorderIdxMap);
        node.right = helper(preorder, inorderIdx - il + 1 + pl, inorder, inorderIdx + 1, ir, inorderIdxMap);
        return node;
    }
}
```