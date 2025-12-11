# 222. Count Complete Tree Nodes

## Solution

### Approach: Binary Search with Height Calculation

**Key Insight:** In a complete binary tree, we can leverage its properties:
- If left height equals right height, it's a perfect binary tree with exactly 2^h - 1 nodes
- Otherwise, we recursively count: 1 + countNodes(left) + countNodes(right)

By computing heights in each recursive call, we avoid visiting all nodes.

**Time Complexity:** O(logn * logn)
- We make at most log(n) recursive calls (tree height)
- Each call computes heights in O(log n) time
- Total: O(logn * logn)

**Space Complexity:** O(log n) for recursion stack

### Java Implementation

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public int countNodes(TreeNode root) {
        if (root == null) {
            return 0;
        }

        // Get height by going all the way left
        int leftHeight = getLeftHeight(root);
        // Get height by going all the way right
        int rightHeight = getRightHeight(root);

        // If heights are equal, it's a perfect binary tree
        if (leftHeight == rightHeight) {
            // 2^h - 1 nodes (using bit shift: 2^h = 1 << h)
            return (1 << leftHeight) - 1;
        }

        // Otherwise, recursively count left and right subtrees
        return 1 + countNodes(root.left) + countNodes(root.right);
    }

    private int getLeftHeight(TreeNode node) {
        int height = 0;
        while (node != null) {
            height++;
            node = node.left;
        }
        return height;
    }

    private int getRightHeight(TreeNode node) {
        int height = 0;
        while (node != null) {
            height++;
            node = node.right;
        }
        return height;
    }
}
```

## Explanation

1. **Base Case:** If root is null, return 0

2. **Height Calculation:**
   - `getLeftHeight()`: Traverse left edges to find the leftmost path length
   - `getRightHeight()`: Traverse right edges to find the rightmost path length

3. **Perfect Binary Tree Check:**
   - If left height equals right height, all levels are completely filled
   - For a perfect tree of height h, total nodes = 2^h - 1
   - We use bit shifting `(1 << h)` which is equivalent to 2^h but faster

4. **Recursive Case:**
   - If heights differ, the tree is not perfect
   - Count current node (1) + nodes in left subtree + nodes in right subtree
   - Each recursive call again checks if subtrees are perfect

## Example Walkthrough

For tree [1,2,3,4,5,6]:
```
       1
      / \
     2   3
    / \  /
   4  5 6
```

1. Root (1): leftHeight=3, rightHeight=2 � Not perfect, recurse
2. Node (2): leftHeight=2, rightHeight=2 � Perfect! Return 2^2 - 1 = 3 nodes
3. Node (3): leftHeight=2, rightHeight=1 � Not perfect, recurse
4. Node (6): leftHeight=1, rightHeight=1 � Perfect! Return 2^1 - 1 = 1 node
5. Final count: 1 + 3 + (1 + 1 + 0) = 6

## Why This is Faster Than O(n)

- Regular traversal: O(n) - visits every node once
- This approach: O(log� n) - uses binary search-like recursion
- At each level, we only recurse into one subtree (either left or right ends up being perfect)
- Height calculations are O(log n), done at most O(log n) times
