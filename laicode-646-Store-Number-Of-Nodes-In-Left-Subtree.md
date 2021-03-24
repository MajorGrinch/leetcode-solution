# Laicode 646. Store Number Of Nodes In Left Subtree

给定一个二叉树，把所有节点左子树大小存在对应节点的`numNodesLeft`里。

又到了我们喜闻乐见的二叉树了，还是那三件事情：
+ 我希望左右子树给我什么？我希望左右子树给我他们的节点个数
+ 我当前节点要做什么？把左子树的个数存入当前节点的`numNodesLeft`里。
+ 我需要向上返回什么？我这个子树的节点个数

搞清楚这三件事情，剩下来的就好办了。写个方法`getNodesNum`，得到左右子树的节点个数之后，把左子树节点个数存好，然后左右子树节点个数之和加上当前节点就是这个子树节点数，返回给上层。

Time complexity: O(N)

Space complexity: O(H), where H is the height.

```java
public class Solution {
  public void numNodesLeft(TreeNodeLeft root) {
    getNodesNum(root);
  }

  private int getNodesNum(TreeNodeLeft root){
    if(root == null){
      return 0;
    }
    int leftNum = getNodesNum(root.left);
    int rightNum = getNodesNum(root.right);
    root.numNodesLeft = leftNum;
    return leftNum + rightNum + 1;
  }
}
```