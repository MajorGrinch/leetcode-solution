# 75. Sort Colors

利用快排中的挡板思想。只不过这里有三个挡板，`i`、`j`和`k`。

循环不变量是[0, i)区间都是0，[i, j)都是1，(k, len - 1]都是2。那么一开始要让这个循环不变量成立的话，i和j就要初始化为0，k要初始化为`len - 1`。

如果我们需要让整个区间有序的话，那么最后j必须等于k + 1才能使[i, j)和(k, len - 1]连续。所以，循环条件是 `while(j <= k)`。

当nums[j]为0时，就把i和j交换并且各自自增以维护循环不变量。这里交换之后nums[i]为0是毋庸置疑的，i++不难理解。然而交换过后nums[j]不一定就是1，因为我并不能确定nums[i]原先指向的就是1。如果nums[i]原先就是1，那么交换过后对j进行自增不难理解。但毕竟i和j一开始都是从0开始，如果原先nums[i]为0，意味着`i == j`即[i, j)是0区间，那么其实j就是和自己交换。如此一来，我们还是得对j进行自增以维护[i, j)0区间的性质。

如果nums[j]为1，就只对j进行自增来维护[i, j)全是1。如果nums[j]为2，就把j和k交换，但是只把k自减，j不动。因为我们并不知道换过来的nums[k]到底是什么数，所以下一轮继续检查。如此一来，每轮循环开始前和结束后，我们的循环不变量都是成立的。

当循环退出之后，我们的循环不变量就能证明我们结果的正确性了。

Time complexity: O(N)

Space complexity: O(1)

```java
class Solution {
    public void sortColors(int[] nums) {
        if(nums == null || nums.length == 0){
            return;
        }
        int i = 0, j = 0, k = nums.length - 1;
        while(j <= k){
            if(nums[j] == 0){
                swap(nums, i++, j++);
            }else if(nums[j] == 2){
                swap(nums, j, k--);
            }else{
                // nums[j] == 1
                j++;
            }
        }
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```