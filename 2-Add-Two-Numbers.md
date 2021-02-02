# 2. Add Two Numbers

给定两个由链表表示的数，返回他们的和的链表。

这题已经说了，数位是倒过来的，也就是说头部是最低位，尾部是最高位，那么就依次相加进位就好。

题目规定了链表不会为空，数字也没有leading 0，所有节点的值都是[0, 9]。如果不限制这些，则需另外考虑。

Time complexity: O(max(M, N)), where M and N are the length respectively

Space complexity: O(max(M, N))

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode res = new ListNode(0);
        ListNode p = res;
        int carry = 0;
        while(l1 != null || l2 != null){
            int d1 = 0, d2 = 0;
            if(l1 != null){
                d1 = l1.val;
                l1 = l1.next;
            }
            if(l2 != null){
                d2 = l2.val;
                l2 = l2.next;
            }
            int sum = d1 + d2 + carry;
            p.next = new ListNode(sum % 10);
            carry = sum / 10;
            p = p.next;
        }
        if(carry != 0){
            p.next = new ListNode(carry);
        }
        return res.next;
    }
}
```