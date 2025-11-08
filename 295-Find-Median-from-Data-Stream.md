# 295. Find Median from Data Stream

给定流输入，要求实现读入数据并随时可以得到已有数据的中位数。

我们可以用两个堆来实现该需求，一个大顶堆，一个小顶堆。我们时刻保证大顶堆存的数字都比小顶堆要小，这样我们就成功的把已有输入按照大小一分为二了。

读入数据的时候，先放入小顶堆，然后把小顶堆里去取一个最小的放入大顶堆。当大顶堆元素个数比小顶堆要多不止1个的时候，我们再安排大顶堆最大的元素回小顶堆，从而保证大顶堆不会比小顶堆多超过1个元素。

读取中位数的时候，如果两个堆数量一致，那么就取各自堆顶元素的平均数。如果不一致，那么大顶堆数量肯定是比小顶堆多1的，那么大顶堆的堆顶元素就是中位数。

```java
class MedianFinder {
    private PriorityQueue<Integer> minHeap;
    private PriorityQueue<Integer> maxHeap;

    public MedianFinder() {
        minHeap = new PriorityQueue<>();
        maxHeap = new PriorityQueue<>(Collections.reverseOrder());
    }
    
    public void addNum(int num) {
        minHeap.offer(num);
        maxHeap.offer(minHeap.poll());
        if(maxHeap.size() - minHeap.size() > 1) {
            minHeap.offer(maxHeap.poll());
        }
    }
    
    public double findMedian() {
        if(minHeap.size() == maxHeap.size()) {
            return (minHeap.peek() + maxHeap.peek()) / 2.0;
        } else {
            return maxHeap.peek() * 1.0;
        }
    }
}
```