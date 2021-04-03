# Laicode 120. Largest And Second Largest

给定一个无序数组，用最少的比较次数找到最大值和次大值。

直观的来说，我们用2N次比较可以找到。但是既然这题问了，说明肯定有比2N次更少的方法。我们思考一下，俗话说得好——`能打败魔法的只有魔法`。拿到这题里面翻译翻译就是，能打败次大值的，只有最大值。也是就是次大值一定会和最大值比较一下，最后被比下去。也就是说我们找到最大值，并且在找最大值的途中记录它都和那些值比较过，在这些值里找到最大的那个就是次大值。

我们为这题新建个内部类`Element`来包裹int，并且用`compared`来记录当前值都和那些其他值比较过。把输入的int数组变成Element数组，然后从数组两头开始同时往中间走。利用和[laicode 119. Largest And Smallest](laicode-119-Largest-And-Smallest.md)相似的思路，把大一点的值放到前面一半。做完后再对前一半进行相同的操作，直至长度为1为止。

此时，最大值就被我们放到了数组的首位，我们进行了N次的比较。再从最大值的compared列表里找到最大的，这个列表有多长呢？logN。所以最后，我们只用了`N+logN`次比较就找到了最大值和次大值。

```java
public class Solution {
  public int[] largestAndSecond(int[] array) {
    int size = array.length;
    Element[] eArray = intToElement(array);
    while (size > 1) {
      int l = 0, r = size - 1;
      while (l < r) {
        if (eArray[l].val < eArray[r].val) {
          swap(eArray, l, r);
        }
        eArray[l].compared.add(eArray[r].val);
        l++;
        r--;
      }
      size = (size + 1) / 2;
    }
    int[] res = new int[2];
    res[0] = eArray[0].val;
    res[1] = getLargest(eArray[0].compared);
    return res;
  }

  private int getLargest(List<Integer> l) {
    int largest = Integer.MIN_VALUE;
    for (int num : l) {
      if (largest < num) {
        largest = num;
      }
    }
    return largest;
  }

  private Element[] intToElement(int[] array) {
    Element[] eArray = new Element[array.length];
    for (int i = 0; i < array.length; i++) {
      eArray[i] = new Element(array[i]);
    }
    return eArray;
  }

  private void swap(Element[] array, int i, int j) {
    Element temp = array[i];
    array[i] = array[j];
    array[j] = temp;
  }

  class Element {
    int val;
    List<Integer> compared;

    Element(int val) {
      this.val = val;
      compared = new ArrayList<>();
    }
  }
}
```