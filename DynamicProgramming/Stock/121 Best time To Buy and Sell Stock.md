本题中，`K = 1`，那么第二个维度我们就不再需要，因为此时递推公式为：

```
dp[i][1][0] = max(dp[i-1][1][0], dp[i-1][1][1] + prices[i])
dp[i][1][1] = max(dp[i-1][1][1], dp[i-1][0][0] - prices[i])
```

而由初始条件，`dp[i-1][0][0] = 0`，所以递推公式为：

```
dp[i][1][0] = max(dp[i-1][1][0], dp[i-1][1][1] + prices[i])
dp[i][1][1] = max(dp[i-1][1][1], - prices[i])
```

那这个维度只用到了 1 ，就不需要的了，我们的递推公式变成了：

```
dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i])
dp[i][1] = max(dp[i-1][1], - prices[i])
```

这也好理解：

* 第`i`天不持有股票：
  * 第`i - 1`天就不持有
  * 第`i - 1`天持有，但是卖了
* 第`i`天持有股票：
  * 第`i - 1`天就持有
  * 第`i - 1`天没有持有，但第`i`天买了。由于前面都没有持有，所以收益就是`0`了

初始条件还有 `i = 0`的情况。

The time complexity is `O(n)`, the space complexity is `O(n)`.

```java
public int maxProfit(int[] prices) {
    int n = prices.length;
    if (n <= 1) {
        return 0;
    }
    int[][] dp = new int[n][2];
    dp[0][0] = 0;
    dp[0][1] = -prices[0];
    for (int i = 1; i < n; i++) {
        dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1] + prices[i]);
        dp[i][1] = Math.max(dp[i-1][1], -prices[i]);
    }
    return dp[n - 1][0];
}
```

The time complexity is `O(n)`, the space complexity is `O(1)`.

```java
public int maxProfit(int[] prices) {
    int n = prices.length;
    if (n <= 1) {
        return 0;
    }
    int dp0 = 0;
    int dp1 = -prices[0];
    for (int i = 1; i < n; i++) {
        dp0 = Math.max(dp0, dp1 + prices[i]);
        dp1 = Math.max(dp1, -prices[i]);
    }
    return dp0;
}
```

> 不过讲道理，这一题，我们只要找到股票价格相差最大的两天，一买一卖不就好了？整那么复杂干嘛？
>
> 当然找这个最大差还是有讲究的，毕竟要先买才能卖。

```java
public int maxProfit(int prices[]) {
    int minPrice = Integer.MAX_VALUE;
    int maxProfit = 0;
    for (int i = 0; i < prices.length; i++) {
        if (prices[i] < minPrice) {
            minPrice = prices[i];
        } else if (prices[i] - minPrice > maxProfit) {
            maxProfit = prices[i] - minPrice;
        }
    }
    return maxProfit;
}
```
