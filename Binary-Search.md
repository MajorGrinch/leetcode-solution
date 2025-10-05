# Binary Search Topic

## CH

二分搜索其实很简单，假定一个循环不变量loop invariant：你要找的数在[l, r]这个闭区间里，并且让你的每一轮循环去维持这个循环不变量。

就拿[278](278-First-Bad-Version.md)来说，find first bad version。

我们的循环不变量(loop invariant)是first bad version出现在[l, r]区间里。

如果mid is not bad verison，那么first bad version肯定出现在[mid + 1, r]。如果mid is bad version，那么first bad version肯定出现在[l, mid]。这样，每一轮循环的始末，loop invariant都是成立的。

最后循环终止，l == r，我知道我每轮循环都在维持loop invariant，所以我知道[l, r]这个长度为1的区间里装的这个版本就是first bad version。

## 关于死循环

当循环体内维护loop invariant有`l = mid`这个调整操作时，那一定要小心死循环。如果这时候循环是 `while(l < r)`，那么很可能会引发死循环。因为 mid = l + (r-l)/2，决定了 mid 是有可能等于 l 的。为了防止死循环的发生，建议把循环改为`while(r - l > 1)`，这样保证了只要进入循环，算出来的 mid 就不可能等于 l。代价是，返回结果的时候，需要额外检查一下 l 和 r。

## while(l < r) OR while(l <= r)

这是两个不同的二分查找循环条件，有时候不知道该用哪个。其实他们在语义上就有着根本区别。
+ **`while(l < r)`本质上是希望最后把target逼到一个长度为1或0的区间，并加以判断。如果区间有1个数，说明这个数就是我们要的target。如果区间有0个数，说明target没找到。**
+ **`while(l <= r)`本质上是让你搜完整个区间，并在搜索过程中发现你要的target。如果该循环因为`l > r`而结束的话，说明没有搜到target。**

我们仔细看看他们的终止状态，`while(l < r)`会因为`l == r` or `l - 1 == r`而结束，此时对应着我们区间里只有1个数或者0个数。很好理解 ，区间里只有1个数时就看看这个数到底是不是target，0个数就说明我没找到target。


而`while(l <= r)`只会因为`l > r`而结束，此时我们搜索区间就必然没有数了。但是，`while(l <= r)`有个性质，就是这个情况有可能会发生`l == mid == r`。即l和r相等，算出来的mid也就和他俩一样。由于这个性质，我们的目的是在搜索过程中找到并返回，比如
```java
if(nums[mid] == target){
    return true;  // or return mid;
}else...
```
那么用`while(l <= r)`的话，如果target真的出现了最后一定会被捕捉到直接return，如果这个循环因为`l > r`而结束的话，那就肯定没有target。

所以，我们在写二分的时候首先要明确一点——我这个二分到底是想在搜索过程中找到并直接返回答案，还是想把答案逼近到一个长度为0或者1的区间里用作最后的判断。

比如[34](34-Find-First-and-Last-Position.md)题，找last occurrence的时候。这个方法是不断地缩小[l, r]区间同时维护loop invariant来逼近last occurrence，最后再加以判断，并不是找到了就直接返回，所以`while(l <= r)`就不行了。
```java
private int lastOccurrence(int[] nums, int target){
    int l = 0, r = nums.length - 1;
    while(r - l > 1){
        int mid = l + (r-l >> 1);
        if(nums[mid] <= target){
            l = mid;
        }else{
            r = mid - 1;
        }
    }
    return nums[r] == target ? r : l;
}
```

当然了，这里我们也可以在搜索过程中找到last occurrence并直接返回。这当然是可以的，只不过代码稍微复杂点。如下，
```java
private int lastOccurrence2(int[] nums, int target){
    int l = 0, r = nums.length - 1;
    while(l <= r){
        int mid = l + (r-l >> 1);
        if(nums[mid] == target){
            if(mid == nums.length - 1 || nums[mid + 1] > target){
                // if mid is last element of array or nums[mid + 1] gt target
                return mid;
            }else{
                l = mid + 1;
            }
        }else if(nums[mid] < target){
            l = mid + 1;
        }else{
            r = mid - 1;
        }
    }
    return -1;
}
```

总结一下，关于这两种循环的使用主要在于
+ 如果是想在搜索过程中找到target并直接返回，那就`while(l <= r)`。
+ 如果是想把答案逼近一个长度为0/1的区间并加以判断，那就是`while(l < r)`。