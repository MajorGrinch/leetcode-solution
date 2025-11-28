## Laicode 207. Majority Number II

参照[leetcode 229](229-majority-element-2.md)。

```java
public class Solution {
  public List<Integer> majority(int[] array) {
    List<Integer> res = new ArrayList<>();
    if(array.length == 0) {
      return res;
    }
    if(array.length == 1) {
      res.add(array[0]);
      return res;
    }
    int candidate1 = array[0];
    int count1 = 1;
    int candidate2 = 0;
    int count2 = 0;
    int i = 1;
    while(i < array.length) {
      if(array[i] == candidate1) {
        count1++;
        i++;
      } else {
        candidate2 = array[i++];
        count2++;
        break;
      }
    }
    while(i < array.length) {
      if(array[i] == candidate1) {
        count1++;
      } else if(array[i] == candidate2) {
        count2++;
      } else if(count1 == 0) {
        candidate1 = array[i];
        count1++;
      } else if(count2 == 0) {
        candidate2 = array[i];
        count2++;
      } else {
        count1--;
        count2--;
      }
      i++;
    }
    count1 = 0;
    count2 = 0;
    for(int num: array) {
      if(num == candidate1) {
        count1++;
      } else if(num == candidate2) {
        count2++;
      }
    }
    if(count1 > array.length / 3) {
      res.add(candidate1);
    }
    if(count2 > array.length / 3) {
      res.add(candidate2);
    }
    return res;
  }
}
```