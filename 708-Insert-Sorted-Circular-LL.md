# 708. Insert into a Sorted Circular Linked List

给定一个环状升序链表，把给定的数插入进这个链表，插入之后保持升序。

这题不难，主要是要考虑的情况太多了，因为题目除了限定链表长度和数值范围，其他均没有限定。要考虑的情况有如下：
+ 空链表
+ 只有一个节点
+ 重复元素

把这些情况都考虑到，双指针解决。

Time complexity: O(N)

Space complexity: O(1)

```java
class Solution {
    public Node insert(Node head, int insertVal) {
        Node newNode = new Node(insertVal);
        if(head == null){
            newNode.next = newNode;
            return newNode;
        }
        Node prev = null, curr = head;
        do{
            if(shouldPlaceAfter(newNode, curr)){
                newNode.next = curr.next;
                curr.next = newNode;
                break;
            }
            prev = curr;
            curr = curr.next;
        }while(curr != head);

        if(newNode.next == null){
            prev.next = newNode;
            newNode.next = curr;
        }
        return head;
    }

    /**
     * Check if node should be placed after a
     */
    private boolean shouldPlaceAfter(Node node, Node a){
        if(a.val <= a.next.val){
            if(a.val <= node.val && node.val <= a.next.val){
                return true;
            }
        }else{
            if(node.val <= a.next.val || node.val >= a.val){
                return true;
            }
        }
        return false;
    }
}
```