# 280. Sort With 2 Stacks

给定一个stack，要求把它排序。

要把它排序的话，用selection sort的思想，借助另一个栈来存放元素。每次都从input里面弹出元素压入buffer，在这个过程中记录目前的最小值`localMin`。

找到目前最小值之后，把buffer里面所有大于等于`localMin`的给弹出来，只把大于`localMin`的压回`input`，等于`localMin`的压回buffer。这样这轮结束之后，input里面只剩大于`localMin`的元素了。

重复到input没有元素为止，和selection sort过程差不多。

Time complexity: O(N^2)

Space complexity: O(N). We need an extra stack.


```java
public class Solution {
    public void sort(LinkedList<Integer> s1) {
        LinkedList<Integer> s2 = new LinkedList<Integer>();
        // s2 is used as a buffer
        sort(s1, s2);
    }

    private void sort(Deque<Integer> input, Deque<Integer> buffer){
        while(!input.isEmpty()){
            int localMin = Integer.MAX_VALUE;
            int count = 0;
            while(!input.isEmpty()){
                int val = input.pop();
                if(val < localMin){
                    localMin = val;
                    count = 1;
                }else if(val == localMin){
                    count++;
                }
                buffer.push(val);
            }
            while(!buffer.isEmpty() && buffer.peek() >= localMin){
                int val = buffer.pop();
                if(val != localMin){
                    input.push(val);
                }
            }
            while(count-- > 0){
                buffer.push(localMin);
            }
        }
        while(!buffer.isEmpty()){
            input.push(buffer.pop());
        }
    }
}
```