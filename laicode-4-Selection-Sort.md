# Laicode 4. Selection Sort

选择排序，算法很简单，i从0到n-2，每次找到[i+1, n)里最小的那个数并换到i的位置来。

这样结束之后，整个数组就有序了。

Time complexity: O(n^2)

Space complexity: O(1)

```java
public class Solution {
    public int[] solve(int[] array) {
        for(int i = 0; i < array.length - 1; i++){
            int minIdx = i;
            for(int j = i+1; j < array.length; j++){
                if(array[j] < array[minIdx]){
                    minIdx = j;
                }
            }
            swap(array, minIdx, i);
        }
        return array;
    }

    private void swap(int[] array, int i, int j){
        int temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }
}
```