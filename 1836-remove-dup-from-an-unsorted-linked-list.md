# 1836. Remove Duplicates From an Unsorted Linked List

给定一个无序链表，清除所有重复元素。

很简单，先过一遍链表并且用 HashMap 记录每个元素出现的次数。第二次遍历的时候再根据对应元素出现的次数来决定是否保留。

Time complexity: O(N)

Space complexity: O(N)


```java
class Solution {
    public ListNode deleteDuplicatesUnsorted(ListNode head) {
        ListNode p = head;
        Map<Integer, Integer> freqMap = new HashMap<>();
        while(p != null) {
            freqMap.put(p.val, freqMap.getOrDefault(p.val, 0) + 1);
            p = p.next;
        }
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        p = dummy;
        while(p.next != null) {
            if(freqMap.get(p.next.val) > 1) {
                p.next = p.next.next;
            } else {
                p = p.next;
            }
        }
        return dummy.next;
    }
}
```