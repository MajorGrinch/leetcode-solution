# Laicode 55. Get Keys In Binary Search Tree In Given Range

给定一个BST，返回给定范围内所有的节点，按照升序排列。

对BST进行中序遍历，就可以升序访问，途中遇到满足条件的就放进列表。这里有个优化的地方，就是当前节点值如果小于等于min的话，就没必要继续遍历左孩子了，因为左子树里面所有的值肯定都小于这个值，也就小于min。同理，如果当前节点值大于等于max的话，也就没必要访问右子树。

为什么把res定义为局部变量 local variable 并作为参数传入方法，而不是作为类的成员变量，instance field——因为方便GC回收。这也算是个小trick吧。不过一味地增加参数也不是最好，代码的可维护性同样也很重要。

之前二叉树遍历的几个题目，我用的是instance field，大意了。不过，无伤大雅。

Time complexity: O(N)

Space complexity: O(H), H is height.

```java
public class Solution {
    public List<Integer> getRange(TreeNode root, int min, int max) {
        List<Integer> res = new ArrayList<>();
        if(root == null){
            return res;
        }
        addWithinRange(root, min, max, res);
        return res;
    }

    private void addWithinRange(TreeNode curr, int min, int max, List<Integer> res){
        if(curr == null){
            return;
        }
        if(curr.key > min){
            addWithinRange(curr.left, min, max, res);
        }
        if(min <= curr.key && curr.key <= max){
            res.add(curr.key);
        }
        if(curr.key < max){
            addWithinRange(curr.right, min, max, res);
        }
    }
}
```