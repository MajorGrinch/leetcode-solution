# 362. Design Hit Counter

开两个长度为 300 的数组，timestampBucket 存放 timstamp，hit存放对应 timestamp 的 hit 次数。为什么要单独的一个数组存放对应时间戳的 hit 次数？因为同一个时间戳可能被 hit 多次。

题目说了，所有的call 都是在时间上有序的。也就是后面的 call 的 timestamp 参数永远比前面的要大。因此，对于 hit 操作，我们可以把 timestamp 模除 300，作为它在数组里的下标。因为一旦 call 了某个 timestamp，它 300 秒之前的那个 timestamp 就可以永远的被覆盖了。比如我 hit(600)，那么后续操作不管是 hit 还是 getHits，他们的参数都必须大于等于 600。就算是 `getHits(600)`，我也只用统计 `[301, 600]` 这个范围的点击数就好。用模除得到下标 `idx` 之后，一旦我们发现 timestampBucket 里面对应下标的 timestamp 和当前传入的 timestamp 不一样，那么我们就直接覆盖，并且重置 hit 数组对应的点击数为 1。如果 `timestampBucket[idx]` 和我们传入的参数一样的话，那就不用覆盖，hit 数组对应位置加 1 就好。

对于 getHits 操作，我们直接遍历 timestampBucket 数组，找到离传入的 timestamp 300 秒内的所有坐标去加他们对应的点击数就好了。

Time complexity: hit: O(1), getHits: O(time) depends on 5 mins or longer.

Space complexity: O(time) depends on 5 mins or longer.


```java
class HitCounter {

    private int[] hit;
    private int[] timestampBucket;

    public HitCounter() {
        hit = new int[300];
        timestampBucket = new int[300];
    }

    public void hit(int timestamp) {
        int idx = timestamp % 300;
        if (timestampBucket[idx] != timestamp) {
            timestampBucket[idx] = timestamp;
            hit[idx] = 1;
        } else {
            hit[idx]++;
        }
    }

    public int getHits(int timestamp) {
        int count = 0;
        for (int i = 0; i < hit.length; i++) {
            if (timestamp - timestampBucket[i] < 300) {
                count += hit[i];
            }
        }
        return count;
    }
}
```