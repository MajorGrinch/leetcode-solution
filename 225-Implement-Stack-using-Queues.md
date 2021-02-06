# 225. Implement Stack using Queues

用队列来模拟栈的实现。

## Two-Queue Approach, push O(1) pop (N)

用两个队列，`q1`和`q2`来存放元素。

`push`的时候就老老实实压入`q1`，这样q1就是按序存放队列元素。O(1)

`pop`的时候，由于我们想要弹出的是最后进来的元素，也就是`q1`的队尾。所以我们可以把`q1`除了队尾的元素，全都按序放到`q2`里面来。这样一来，`q1`就只有最后进来的元素了，就直接`pop`出去就可以实现栈的`pop`操作了。但是呢，这时候`q2`存了我们所有现有的元素了，所以把q1和q2对调一下。由于有把`q1`元素挪到`q2`的过程，所以此操作时间复杂度是`O(N)`。

`top`操作和`pop`差不多，只不过`top`不会真的弹出元素，所以没有必要对调`q1`和`q2`。

`empty`操作无需多说。

```java
class MyStack {

    private Queue<Integer> q1, q2;
    /** Initialize your data structure here. */
    public MyStack() {
        q1 = new ArrayDeque<>();
        q2 = new ArrayDeque<>();
    }

    /** Push element x onto stack. */
    public void push(int x) {
        q1.offer(x);
    }

    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        if(q1.size() > 1){
            feedQ2();
        }
        int res = q1.poll();
        Queue<Integer> temp = q1;
        q1 = q2;
        q2 = temp;
        return res;
    }

    /** Get the top element. */
    public int top() {
        if(q1.size() > 1){
            feedQ2();
        }
        return q1.peek();
    }

    /** Returns whether the stack is empty. */
    public boolean empty() {
        return q1.isEmpty() && q2.isEmpty();
    }

    private void feedQ2(){
        while(q1.size() > 1){
            q2.offer(q1.poll());
        }
    }
}
```

## Two-Queue Approach, push O(N) pop O(1)

上一个方法是用两个队列以及O(1)的`push`和O(N)的`pop`为代价实现的栈，其实也可以用O(N)的`push`和O(1)的`pop`为代价来实现。

还是`q1`和`q2`。`q2`提供缓冲，在所有操作结束后`q2`一定要保持空队列。而`q1`则永远倒序存放所有元素——也就是先进来的元素在队尾，后进来的元素在队头，这样就可以实现O(1)的`pop`了。

`push`的时候，把`x`放进`q2`，然后把`q1`所有元素按序放入`q2`这样就实现了后进来的元素在队头的性质。把`q1`和`q2`对调一下，就满足了`q1`和`q2`的性质。

`pop`的时候，直接弹出`q1`的队头，时间复杂度是O(1)。

其他操作不变。

```java
class MyStack2 {

    /**
     * O(N) push, O(1) pop
     */
    private Queue<Integer> q1, q2;
    /** Initialize your data structure here. */
    public MyStack() {
        q1 = new ArrayDeque<>();
        q2 = new ArrayDeque<>();
    }

    /** Push element x onto stack. */
    public void push(int x) {
        q2.offer(x);
        while(!q1.isEmpty()){
            q2.offer(q1.poll());
        }
        Queue<Integer> temp = q1;
        q1 = q2;
        q2 = temp;
    }

    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        return q1.poll();
    }

    /** Get the top element. */
    public int top() {
        return q1.peek();
    }

    /** Returns whether the stack is empty. */
    public boolean empty() {
        return q1.isEmpty() && q2.isEmpty();
    }
}
```

## One-Queue Approach

省点空间，用一个队列也可以直接模拟。那么这个队列就保持后进来的元素在队头，先进来的元素在队尾的性质就好了。

`push`的时候，先把`x`放入队尾，再把前面所有的元素弹出并重新入队。这样`x`作为后来的元素，成功的到了队头。时间复杂度是O(N)。

`pop`的时候，直接弹出队头的元素就好。

其他操作不变。

```java
class MyStack3 {

    private Queue<Integer> q;
    /** Initialize your data structure here. */
    public MyStack() {
        q = new ArrayDeque<>();
    }

    /** Push element x onto stack. */
    public void push(int x) {
        int size = q.size();
        q.offer(x);
        while(size-- > 0){
            q.offer(q.poll());
        }
    }

    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        return q.poll();
    }

    /** Get the top element. */
    public int top() {
        return q.peek();
    }

    /** Returns whether the stack is empty. */
    public boolean empty() {
        return q.isEmpty();
    }
}
```