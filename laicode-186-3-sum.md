# Laicode 186. 3 Sum

参照[leetcode 15](15-3-Sum.md)，只不过那题的 target 是 0，这题的 target 是输入。

```java
public class Solution {
  public List<List<Integer>> allTriples(int[] array, int target) {
    Arrays.sort(array);
    List<List<Integer>> res = new ArrayList<>();
    for(int i = 0; i < array.length - 2; i++) {
      findTwoSum(array, target - array[i], i + 1, array.length - 1, array[i], res);
      while(i < array.length - 2 && array[i] == array[i + 1]) {
        i++;
      }
    }
    return res;
  }

  /*
  find two sum in sorted array[st, ed] for target
  */
  private void findTwoSum(int[] array, int target, int st, int ed, int first, List<List<Integer>> res) {
    int l = st;
    int r = ed;
    while(l < r) {
      if(array[l] + array[r] == target) {
        res.add(Arrays.asList(first, array[l], array[r]));
        while(l < r && array[l] == array[l + 1]) {
          // skip same array[l]
          l++;
        }
        l++;
        while(l < r && array[r] == array[r - 1]) {
          // skip same array[r]
          r--;
        }
        r--;
      } else if(array[l] + array[r] < target) {
        l++;
      } else {
        r--;
      }
    }
  }
}
```