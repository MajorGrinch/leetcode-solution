# 155. Min Stack

## Two Stack Approach

题目要求设计一个Min Stack要求能在常数级时间复杂度内获取目前最小的元素。

用两个栈来实现，一个栈`stack`正常存元素，另一个栈`currMin`存的是截止到`stack`里最新的元素遇到的最小的数。

压入新元素`x`时，先正常压入`stack`。当currMin不空而且currMin的栈顶元素比`x`小的话，那就说明目前最小的不是`x`，而是`currMin`的栈顶，所以就再压一次`currMin`的栈顶。否则，就把`x`压入`currMin`。这样`getMin`的时候，我们直接返回`currMin`的栈顶元素就可以表明目前`stack`内最小的元素是谁。

Time complexity: O(1) for all operations.

Space complexity: O(N) because of the extra `currMin` stack.

```java
class MinStack {
    private Deque<Integer> stack, currMin;
    public MinStack() {
        stack = new ArrayDeque<>();
        currMin = new ArrayDeque<>();
    }

    public void push(int x) {
        stack.push(x);
        if(currMin.isEmpty() || currMin.peek() > x){
            currMin.push(x);
        }else{
            currMin.push(currMin.peek());
        }
    }

    public void pop() {
        stack.pop();
        currMin.pop();
    }

    public int top() {
        return stack.peek();
    }

    public int getMin() {
        return currMin.peek();
    }
}
```

## Two Stack Approach Optmization

上面那个方法需要额外的一个栈`currMin`来存放相同数量的元素来存放目前`stack`栈内最小的元素是多少。我们可以额外的压缩一下，不保证可以压到常量级，但是平均情况下可以压缩很多。

比如给定一串数字一次压入站`5, 8, 15, 2, 10`，那么我可以说，
+ size是`[1, 3]`的时候，最小的是`5`
+ size是`[4, 5]`的时候，最小的是`2`。

也就是说size从1开始最小的就是5，从4开始最小的就是2。那么currMin可以这么存，[5, 1] -> [2, 4]。

`push`操作，先正常把`x`压入`stack`。再看看currMin是否为空，为空的话说明这次压的是首个元素，所以它当然是最小的，压入[x, 1]。currMin不为空的话，那就看看currMin的顶部元素是不是比`x`大，是的话那就意味着从目前的size开始`x`是最小值了，所以压入[x, stack.size()]。这样我们可以极大程度上压缩`currMin`需要存的数量。

`pop`的时候，`stack`正常弹出。至于`currMin`的话，把此时`stack`的元素数量和`currMin`的顶部作对比。如果`stack`的数量小的话，说明此时`currMin`的顶部元素已经不能代表`stack`里最小的元素了。因为`currMin`的顶部元素[x, y]表示`stack`从有了`y`个元素开始，最小值是`x`。因此，弹出`currMin`的顶部元素。否则，不碰`currMin`。

Time complexity: O(1) for all operations.

Space complexity: O(N) but we can save a lot of space by the compress.

```java
class MinStack {
    private Deque<Integer> stack;
    private Deque<int[]> currMin;
    public MinStack() {
        stack = new ArrayDeque<>();
        currMin = new ArrayDeque<>();
    }

    public void push(int x) {
        stack.push(x);
        if(currMin.isEmpty() || currMin.peek()[0] > x){
            currMin.push(new int[]{ x, stack.size() });
        }
    }

    public void pop() {
        int res = stack.pop();
        if(currMin.peek()[1] > stack.size()){
            currMin.pop();
        }
    }

    public int top() {
        return stack.peek();
    }

    public int getMin() {
        return currMin.peek()[0];
    }
}
```