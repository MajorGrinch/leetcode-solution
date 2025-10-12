199. Binary Tree Right Side View

给一个二叉树，返回右视图。

## BFS approach

BFS的话，就按层来搜索，和[102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/description/)差不多。唯一的区别是，我们只要每一层最后一个节点作为right-side view。

```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if(root == null) {
            return res;
        }
        Deque<TreeNode> levelQ = new ArrayDeque<>();
        levelQ.offer(root);
        while(!levelQ.isEmpty()) {
            int size = levelQ.size();
            for(int i = 0; i < size; i++) {
                TreeNode node = levelQ.poll();
                if(i == size - 1) {
                    res.add(node.val);
                }
                if(node.left != null) {
                    levelQ.offer(node.left);
                }
                if(node.right != null) {
                    levelQ.offer(node.right);
                }
            }
        }
        return res;
    }
}
```

## DFS approach

DFS的话，就是优先搜右子树。

搜索的同时，从0开始记录当前的层数。当前层数如果等于结果长度，说明是第一次搜这一层，那么该节点就是最右边的节点，把他添加进入结果。如果当前层数不等于结果长度，那么说明当前层已经搜过了，就继续往下搜。

```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        dfs(root, 0, res);
        return res;
    }

    private void dfs(TreeNode root, int depth, List<Integer> res) {
        if(root == null) {
            return;
        }
        if(res.size() == depth) {
            res.add(root.val);
        }
        dfs(root.right, depth + 1, res);
        dfs(root.left, depth + 1, res);
    }
}
```