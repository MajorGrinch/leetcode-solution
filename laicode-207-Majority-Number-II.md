## Laicode 207. Majority Number II

和[169. Majority Element](169-Majority-Element.md)类似，只不过这要求次数超过1/3的。

首先我们可以得知，这个数组里最多只有两个这样的数。那么我们就用两个变量`candidate1`和`candidate2`来记录两个候选数字，以及`count1`和`count2`来记录他们各自出现的次数。还和之前那题差不多的思路，如果目前`count1`或者`count2`为0，就重置对应的候选数字为当前数字。否则，如果当前数字是任意候选数字的话，把它对应的count自增。如果不是候选数字的话，两个候选数字的count一起自减。为什么要两个count都自减呢？因为这样子我们相当于从数组中去除了一个三元组，我们知道这三元组组成的数组没有Majority Number。所以从数组里去掉这个三元组，对最后的答案没有影响。

但是这题不保证输入的数组里有Majority Number，所以我们对两个候选数字还要检查一下。

Time complexity: O(N)

Space complexity: O(1)

```java
public class Solution {
  public List<Integer> majority(int[] array) {
    if (array.length == 1) return Arrays.asList(array[0]);
    int candidate1 = 0, candidate2 = 0;
    int count1 = 0, count2 = 0;
    for (int i = 0; i < array.length; i++) {
      if (count1 == 0) {
        candidate1 = array[i];
        count1 = 1;
      } else if (count2 == 0) {
        candidate2 = array[i];
        count2 = 1;
      } else if (candidate1 == array[i]) {
        count1++;
      } else if (candidate2 == array[i]) {
        count2++;
      } else {
        count1--;
        count2--;
      }
    }
    count1 = 0;
    count2 = 0;
    for (int num : array) {
      if (num == candidate1) count1++;
      else if (num == candidate2) count2++;
    }
    List<Integer> res = new ArrayList<>();
    if (count1 > array.length / 3) res.add(candidate1);
    if (count2 > array.length / 3) res.add(candidate2);
    return res;
  }
}
```