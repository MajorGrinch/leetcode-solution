# 107. Binary Tree Level Order Traversal II

参照[leetcode 102](102-Binary-Tree-Level-Order-Traversal.md)，其实是一样的，只不过最后把结果反过来就行。唉，就当我没做过这题。。。


```java
class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        Queue<TreeNode> levelNodeQ = new LinkedList<>();
        levelNodeQ.offer(root);
        while (!levelNodeQ.isEmpty()) {
            int levelSize = levelNodeQ.size();
            List<Integer> levelList = new ArrayList<>();
            for (int i = 0; i < levelSize; i++) {
                TreeNode curr = levelNodeQ.poll();
                levelList.add(curr.val);
                if (curr.left != null)
                    levelNodeQ.offer(curr.left);
                if (curr.right != null)
                    levelNodeQ.offer(curr.right);
            }
            res.add(levelList);
        }
        return res;
    }
}
```