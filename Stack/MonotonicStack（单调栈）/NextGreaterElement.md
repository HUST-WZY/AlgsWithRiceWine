这是典型的用“单调栈”来解决的一类问题。

首先“单调栈”就是一个栈，其中的元素排列是单调的。

给定一个数组 `nums` ，我们希望找到：对于数组中的每个索引`i`，在它的右边，比它更大的第一个元素的值。直观的想法就是往后遍历，那就要在每个索引处都遍历，时间复杂度为 `O(n^2)`。

当我们站在索引`i`处，我们要找的，其实就是站在索引`i`往右看，能看到的第一个元素：如果`i`的右边有多个比`nums[i]`大的值，要第一个就好；如果没有，那就看不到，就是-1。

那么，我们就想维护一个栈，其中存放站在索引`i`处，往后边看，能看到的所有元素，即后面的所有比`nums[i]`大的元素。这个栈的栈顶，应为我们要的`ans[i]`。

因为我们要从左往右看，所以我们让元素从右往左入栈，那出栈顺序就跟看的顺序一样了。

对于每次循环，首先拿`nums[i]`与栈顶的元素比较：

* 如果`nums[i]`小，那`ans[i]`就是栈顶；
* 如果`nums[i]`大，那就让栈顶的元素出栈，直到`nums[i]`小；如果到不了这样，就给个`-1`。
* 最后`nums[i]`都要进栈。

主要代码如下。注意，看起来是嵌套了一个循环，但由于每个元素都只会进栈一次，所以时间复杂度为 `O(n)`。

```java
int n = nums.length;
int[] ans = new int[n];
Deque<Integer> stack = new LinkedList<>();
for (int i = n - 1; i >= 0; i--) {
    while (!stack.isEmpty() && nums[i] >= stack.peek()) {
        stack.pop();
    }
    ans[i] = stack.isEmpty() ? -1 : stack.peek();
    stack.push(nums[i]);
}
return ans;
```


### 496. 下一个更大元素 I

首先我们弄懂题意：`nums1`中的元素，在`nums2`中都能找到值相同的元素。然后在`nums2`中，找下一个更大的。

这题我们提供两种解法。

* 法一：沿用上面的思路。就对`nums2`求，然后把答案记录在`map`中，键为数组中的元素的值，值为答案。然后再把`map`中的装到答案数组中。

```java
public int[] nextGreaterElement(int[] nums1, int[] nums2) {
    int n = nums1.length;
    int[] ans = new int[n];
    Deque<Integer> s = new LinkedList<>();
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = nums2.length - 1; i >= 0; --i) {
        while (!s.isEmpty() && nums2[i] >= s.peek()) {
            s.pop();
        }
        map.put(nums2[i], s.isEmpty() ? -1 : s.peek());
        s.push(nums2[i]);
    }
    for (int i = 0; i < n; ++i) {
        ans[i] = map.get(nums1[i]);
    }
    return ans;
}
```

* 法二：
法一很直观，但明显用了多余的空间。我们可以首先把`nums1`中的值和索引装进`map`中，即反向映射一下；然后遍历`nums2`，当遍历到的`nums2[i]`为`map`中的键时，就判断它很栈中元素的关系；否则，就直接进行下一个遍历。注意，不管`nums2[i]`是否为键，都要入栈的。

```java
public int[] nextGreaterElement(int[] nums1, int[] nums2) {
    int n1 = nums1.length;
    int n2 = nums2.length;
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < n1; ++i) {
        map.put(nums1[i], i);
    }
    int[] ans = new int[n1];
    Deque<Integer> s = new LinkedList<>();
    for (int i = n2 - 1; i >= 0; --i) {
        int cur = nums2[i];
        if (map.containsKey(cur)) {
            while (!s.isEmpty() && cur >= s.peek()) {
                s.pop();
            }
            ans[map.get(cur)] = s.isEmpty() ? -1 : s.peek();
        }
        s.push(cur);
    }
    return ans;
}
```

方法二的空间复杂度更优，方法一的时间复杂度更优。


### 503. 下一个更大元素 II

首先何为循环数组？就是从左往右遍历，遍历到最右边以后，再回到最左边，继续从左往右遍历，这样循环。

一种构造循环数组的方法，比如要无限遍历
```java
int n = nums.length;
int i = 0;
while(true) {
    sout(nums[i % n]);
    i++;
}
```
比如要遍历两遍
```java
int n = nums.length;
for (int i = 0; i < 2 * n; ++i) {
    sout(nums[i % n]);
}
```
再比如要倒着遍历两边
```java
int n = nums.length;
for (int i = 2 * n - 1; i >= 0; --i) {
    sout(nums[i % n]);
}
```
那么现在，这道题，就是要倒着遍历，而且，是要倒着遍历两次：
* 第一次：就是像原来一样，站在索引`i`处，从左往右看；
* 第二次：不仅在索引`i`处从左往右看了，而且到了最右边，又拐回开头，再次从左往右看。
这样就达到题目的那个效果了。
```java
public int[] nextGreaterElements(int[] nums) {
    int n = nums.length;
    int[] ans = new int[n];
    Deque<Integer> stack = new LinkedList<>();
    for (int i = 2 * n - 1; i >= 0; i--) {
        while (!stack.isEmpty() && nums[i % n] >= stack.peek()) {
            stack.pop();
        }
        ans[i % n] = stack.isEmpty() ? -1 : stack.peek();
        stack.push(nums[i % n]);
    }
    return ans;
}
```
