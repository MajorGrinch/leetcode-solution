# 170. Two Sum III - Data structure design

用一个 HashMap 来记录出现过的数字以及对应的次数。find 的时候，我们就直接遍历这个 Map 的所有数字找 two sum。

要注意一个 corner case，就是万一 target - v1 == v2，比如 3 出现了 2 次，然后我们 find(6)，想想看这个该怎么处理？

Time complexity: O(N)

Space complexity: O(N)

```java
class TwoSum {

    private Map<Integer, Integer> freqMap;

    public TwoSum() {
        freqMap = new HashMap<>();
    }
    
    public void add(int number) {
        freqMap.put(number, freqMap.getOrDefault(number, 0) + 1);
    }
    
    public boolean find(int value) {
        for(int v1: freqMap.keySet()) {
            int v2 = value - v1;
            if(freqMap.containsKey(v2)) {
                if(v1 != v2) {
                    return true;
                } else if(freqMap.get(v2) > 1){
                    return true;
                }
            }
        }
        return false;
    }
}
```