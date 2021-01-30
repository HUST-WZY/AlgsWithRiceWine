我们首先弄明白二分查找的三种形式，背会；之后，二分查找的难点就是如何定义二分查找问题。

对于二分查找，首先我们要保证，被查找的函数`f(x)`，它是单调的：

* 数学上的单调
* 二值分段，如前一段为`false`，后一段为`true`

## 二分查找的第一种情况：查找值为某一定值

这种情况具体为：给定被查找的数学上单调的函数`f(x)`，寻找使`f(x) == target`的值`x`。代码如下：

```java
private int f(int x) {}

public int binartSearch(int min, int max, int target) {
    int lo = min, hi = max;
    
    while(lo <= hi) {
        
        int mid = lo + (hi - lo) / 2;
        
        if (f(mid) == target) {
            return mid;
        }
        else if (f(mid) < target) {
            lo = mid + 1;
        }
        else {
            hi = mid - 1;
        }
    }
    return -1;   // lo-1 or lo
}
```

如果最终是找不到可以满足`f(x) == target`的值`x`，那么，我们可以返回`-1`。也可以返回最接近满足`f(x) == target`的值`x`，那么就需要根据`f(x)`的单调性，去判断返回`lo - 1`或是`lo`：（就是我们的直觉）

* `f(x)`单调递增：
  * 返回最接近目标`x`但小于它的：`return lo - 1;`
  * 返回最接近目标`x`但大于它的：`return lo;`
* `f(x)`单调递减：
  * 返回最接近目标`x`但大于它的：`return lo - 1;`
  * 返回最接近目标`x`但小于它的：`return lo;`

## 查找值为某一范围内的最值

所要查找的`x`关于`f(x)`所满足的条件，不再是只有一个`x`满足了，而是一个连续的区间。我们要在该区间中，返回区间的左端点或者右端点，也就满足关于`f(x)`的某一条件的`x`的最值。并且，不管`f(x)`是数学单调还是二值分段，代码都一样。

### 二分查找的第二种情况：最小值

所要求值的区间为`[min, max]`，满足与`f(x)`相关的某一条件`boolean isOK(x, f(x))`的`x`的值的为一个范围，求该范围中的最小值。

```java
private int f(int x) {}
private boolean isOk(int x)

public int binartSearch(int min, int max) {
    int lo = min, hi = max;
    
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        
        if (isOK(mid)) {
            hi = mid - 1;
        } else {
            lo = mid + 1;
        }
    }
    
    if (lo > max || !isOK(lo)) {
        return -1;
    }
    return lo;
}
```

### 二分查找的第三种情况：最大值

跟最小值的情况其实差不多，只是写法变了一下。

```java
private int f(int x) {}
private boolean isOk(int x)

public int binartSearch(int min, int max) {
    int lo = min, hi = max;
    
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        
        if (isOK(mid)) {
            lo = mid + 1;
        } else {
            hi = mid - 1;
        }
    }
    
    if (hi < min || !isOK(hi)) {
        return -1;
    }
    return hi;
}
```

### 记忆

对于查找定值的情况，就稍微思考一下吧；对于查找范围内最值的情况，如果要查找的是最大值，那就要让区间下限`lo`去变大呀，所以在满足`isOK`时，就搞`lo`；否则就搞`hi`；那要最大值，最后肯定要返回区间上限`hi`。
