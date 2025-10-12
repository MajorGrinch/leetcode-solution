# Laicode 11. Rainbow Sort

用 i, j, k三个挡板。
+ `[0, i)` -> -1，i的左边放置-1
+ `[i, j)` -> 0，i到j之前放置0
+ `(k, len)` -> 1，k右边放置1


```java
public class Solution {
  public int[] rainbowSort(int[] array) {
    if(array.length == 1) return array;

    int i = 0; // all items before i are -1
    int j = 0;  // all items [i, j) are 0
    int k = array.length - 1;  // all items [k, end) are 1

    while(j <= k) {
      if(array[j] == -1) {
        swap(array, i++, j++);
      } else if(array[j] == 0) {
        j++;
      } else {
        // array[j] == 1, j cannot move because we do not know what value swapped to j
        swap(array, j, k--);
      }
    }

    return array;
  }

  private void swap(int[] array, int i, int j) {
    int temp = array[i];
    array[i] = array[j];
    array[j] = temp;
  }
}
```