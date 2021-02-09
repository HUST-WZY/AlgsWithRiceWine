# 计算整数 `int` 的长度/位数

首先，`int` 的最大值 `Integer.MAX_VALUE = 2147483647` 的长度为`10`，那么要求的长度最长也就是`10`了。

别说什么做除法、求对数，统统是垃圾，这个代码无敌：

```java
private final int[] sizeTable = {9, 99, 999, 9999, 99999, 999999, 9999999, 99999999, 999999999, Integer.MAX_VALUE};

public int stringSize(int x) {
    for (int i = 0; ; i++) {
        if (x <= sizeTable[i]) {
            return i + 1;
        }
    }
}
```


# 列表反向插入

有时，我们要从前往后插入列表，即过来的数据流的顺序跟我们实际要得到的列表中的顺序是反着的。

此时会有两种思路，三种方法:
* 正常插入列表，插入完了以后反转一下列表`Collections.reverse()`
* 每次都往列表的头部插入，即把已有的元素往后面推
    * 数组实现
    * 链表实现
    
根据实际测试，中间的方法最慢，第一种方法跟第三种方法差不多，第三种更快。


# 最大公约数

欧几里得的辗转相除法，在《算法4》中讲递归时提到了，主要基于两个事实：

* 对于两个非负整数，如果其中一个是`0`，那么最大公约数`gcd`就是另外一个数；
* 对于两个正整数，它们两个的`gcd`，等于，较小的数与它们两个相除得到的余数的`gcd`相同。

在调用该方法时，应该保证`max >= min`。而在之后的递归中，就自动保证了`min >= max % min`。

```java
public int gcd(int max, int min) {
    if (min == 0) {
        return min;
    }
    return gcd(min, max % min);	
}
```

# 最小公倍数

计算方法主要基于下面的事实：

* 两个非负整数的最小公倍数，与它们的最大公约数的乘积，等于它们两个的乘积

```java
public int lcm(int max, int min) {
    return max * min / gcd(max, min);
}
```


# 转字符串

将 char、int、double 转成 **字符串 String**，一种简单的方法是 `+ ""`。但是这种方法的效率不高，高效的做法是下面的。
同时记录一种将 char[] 转为 **字符串 String**的方法

```java
char c;
String s = Character.toString(c);

int i;
String s = Integer.toString(i);

double d;
String s = Double.toString(d);

char[] c;
String s = new String(c);
```

# 字符串比较

比较两个字符串的值是否相等时，一定要用`equals()`方法，不能使用`==`。因为`==`比较的是地址值，对于两个字符串对象，它们的地址值是不同的，那么就算它们的字符串的值是相同的，也会返回`false`；而使用`equals()`方法就是比较的字符串的值了。

> 进一步，对于所有的包装类的比较，如`Character`、`Integer`等，都要用`equals()`方法进行比较。而基本数据类型`int`等才用`==`比较。
