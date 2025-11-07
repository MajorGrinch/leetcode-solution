# 257. Binary Tree Paths

给定一个二叉树，打印所有根节点到叶子的路径。很简单，自顶向下 DFS。遇到叶子节点就加入答案并返回，否则就继续搜。

这题真正需要注意的是复杂度，时间复杂度是O(NlogN)，因为每个叶子节点都需要复制整个路径。空间复杂度是O(N)，因为要记录路径。

```java
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<List<Integer>> pathList = new ArrayList<>();
        helper(root, new ArrayList<>(), pathList);
        return pathList.stream().map(path -> pathToString(path)).toList();
    }

    private void helper(TreeNode node, List<Integer> path, List<List<Integer>> pathList) {
        if(node == null) {
            return;
        }
        path.add(node.val);
        if(node.left == null && node.right == null) {
            // leaf node
            pathList.add(new ArrayList<>(path));
            path.remove(path.size() - 1);
            return;
        }
        helper(node.left, path, pathList);
        helper(node.right, path, pathList);
        path.remove(path.size() - 1);
        return;
    }

    private String pathToString(List<Integer> path) {
        StringBuilder sb = new StringBuilder();
        for(int i = 0; i < path.size(); i++) {
            sb.append(path.get(i));
            if(i != path.size() - 1){
                sb.append("->");
            }
        }
        return sb.toString();
    }
}
```