# 215. Kth Largest Element in an Array

找到数组中第k个大的数。

用小顶堆来实现，数组所有数字依次入堆。当堆的长度超过K之后，每加入一个新的，就弹出一个旧的。最后这个堆里面剩的就是这个数组里前K个大的元素，堆顶就是第K大的元素。

如果想亲自实现小顶堆的话，可以参考[Laicode 25](laicode-25-K-Smallest-In-Unsorted-Array.md)里面的大顶堆来自己实现个小顶堆。

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        for(int num: nums) {
            minHeap.offer(num);
            if(minHeap.size() > k) {
                minHeap.poll();
            }
        }
        return minHeap.peek();
    }
}
```