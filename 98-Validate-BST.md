# 98. Validate Binary Search Tree

二叉搜索树的特点是，对于这个树里的任意节点，该节点左子树里所有节点的值都比它小，右子树里所有节点的值都比它大。这是个充要条件，即满足这一性质的二叉树，就是二叉搜索树。二叉搜索树一定可以推出这个性质。

那么我们这里就可以充分的利用这个充要条件，对二叉树进行搜索。搜索的同时，对节点的值设定一个上下界，以保证二叉搜索树的性质。搜索的base case是空节点，以空节点为根的子树，当然算BST。思考三件事情，
+ 我希望左右孩子给我返回什么？他们各自是否为BST。
+ 我当前层想要干什么？计算以我为根的子树是否是BST。
+ 我想给上层返回什么？返回以我为根的子树是否是BST。

由于该性质，对于左子树搜索时，所有节点的上限值就必定是当前节点值减一。同理，对于右子树搜索时，所有节点下限值必定是当前节点值加一。如果当前节点越过了上下界，说明当前节点破坏了BST的性质，这也导致整棵树不再是BST。

对于整颗树来说，节点的上下限肯定是int范围。但是，为了防止overflow，我们用long来定义上下界。最后我们就可以计算出整个树是否是BST。

Time complexity: O(N)

Space complexity: O(H), where H is the height.

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    private boolean isValidBST(TreeNode curr, long minValue, long maxValue){
        if(curr == null){
            return true;
        }
        long val = curr.val;
        if(val < minValue || val > maxValue){
            return false;
        }
        return isValidBST(curr.left, minValue, val - 1) && isValidBST(curr.right, val + 1, maxValue);
    }
}
```