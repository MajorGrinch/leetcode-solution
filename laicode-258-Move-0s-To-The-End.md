# Laicode 258. Move 0s To The End I

题目要求把所有的0都挪到最后，其他元素相对位置不用维护。

利用快排中的循环不变量，这里也是一左一右两个挡板`l`和`r`。`l`左边，即[0, l)都是不等于0的，`r`右边，即(r, len - 1]都是等于0的。

和快排类似的维护循环不变量的方法，最后循环退出时候循环不变量依然成立，所以我们可以确定答案的正确性。

Time complexity: O(N)

Space complexity: O(1)

```java
public class Solution {
    public int[] moveZero(int[] array) {
        int l = 0, r = array.length - 1;
        while(l <= r){
            if(array[l] != 0){
                l++;
            }else if(array[r] == 0){
                r--;
            }else{
                swap(array, l++, r--);
            }
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