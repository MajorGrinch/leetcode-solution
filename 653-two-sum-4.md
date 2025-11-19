# 653. Two Sum IV - Input is a BST

直接搜索二叉树就好，途中用一个 HashSet 记录已经访问过的点。二叉树搜索，还是那三个老问题：
+ 我希望左右孩子给我返回什么？他们里面是否含有节点和目前已访问过节点形成 two sum == k 
+ 当前节点需要干什么？对于已访问过的节点，有没有可以和我的 two sum == k 的
+ 我需要向上返回什么？返回有没有可以和我的 two sum == k 的已访问节点

```java
class Solution {
    public boolean findTarget(TreeNode root, int k) {
        return find(root, k, new HashSet<>());
    }

    private boolean find(TreeNode node, int k, Set<Integer> vis) {
        if(node == null) {
            return false;
        }
        if(vis.contains(k - node.val)) {
            return true;
        }
        vis.add(node.val);
        return find(node.left, k, vis) || find(node.right, k, vis);
    }
}
```