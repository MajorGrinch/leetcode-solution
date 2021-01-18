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

这是两个不同的循环条件，有时候不知道该用哪个。

我们仔细看看这个循环的终止状态，`while(l < r)`会因为`l == r` or `l - 1 == r`而结束，此时对应着我们区间里只有1个数或者0个数。很好理解 ，区间里只有1个数时就看看这个数到底是不是target，0个数就说明我没找到target。


而`while(l <= r)`只会因为`l > r`而结束，此时我们搜索区间就必然是0个数。但是，`while(l <= r)`有个性质，就是这个情况有可能会发生`l == mid == r`。即l和r相等，算出来的mid也就和他俩一样。由于这个性质，所以如果我们的目的是找到target就行了，诸如
```java
if(nums[mid] == target){
    return true;  // or return mid;
}else...
```
那么用`while(l <= r)`的话，如果target真的出现了最后一定会被捕捉到直接return，如果这个循环因为`l > r`而结束的话，那就肯定没有target。

不过，如果我们是找target的first occurrence or last occurrence，那么`while(l <= r)`还是就不行了。因为这里的目的不仅仅是找到target就行，而是附带条件的，所以我们没有直接return的操作。当然了，你可以加上return，只是代码稍微复杂一点。
```java
private int lastOccurrence2(int[] nums, int target){
    int l = 0, r = nums.length - 1;
    while(l <= r){
        int mid = l + (r-l >> 1);
        if(nums[mid] == target){
            if(mid == nums.length - 1 || nums[mid + 1] > target){
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

总结一下，如果循环体里有直接return或者break的情况，那么可以用`while(l <= r)`，因为如果有满足条件的target，肯定会被找到。如果循环体里只是不断地调整边界，那么还是用`while(l < r)`，最后检查一下l就行，同时注意死循环的问题。