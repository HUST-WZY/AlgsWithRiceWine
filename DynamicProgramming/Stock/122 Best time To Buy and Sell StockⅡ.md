本题中，`K = +infinity`，那么第二个维度我们就因为相同的原因不再需要了，因此递推公式为：

```
dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i])
dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i])
```

```java
public int maxProfit(int[] prices) {
	int n = prices.length;
    if (n <= 1) return 0;
    int[][] dp = new int[n][2];
    dp[0][0] = 0;
    dp[0][1] = -prices[0];
    for (int i = 1; i < n; i++) {
        dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1] + prices[i]);
        dp[i][1] = Math.max(dp[i-1][1], dp[i-1][0] - prices[i]);
    }
    return dp[n - 1][0];
}
```

```java
public int maxProfit(int[] prices) {
    int n = prices.length;
    if (n <= 1) return 0;
    int dp0 = 0;
    int dp1 = -prices[0];
    for (int i = 1; i < n; i++) {
        int temp = dp0;
        dp0 = Math.max(dp0, dp1 + prices[i]);
        dp1 = Math.max(dp1, temp - prices[i]);   // 
    }
    return dp0;
}
```

上面的空间优化是一种非常常规的编写方式。但是实际上，不需要使用`temp`来记录`dp0`。

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
        dp1 = Math.max(dp1, dp0 - prices[i]);
    }
    return dp0;
}
```

关键矛盾是第一行更新了`dp0`，第二行想要使用更新前的`dp0`。 更新`dp0`的情况只有两种：不变，那就不存在矛盾； 变为`dp1 + price [i]`，这意味着股票在第`i`天卖出，也就是说，第`i-1`天是持有股票的。那更新`dp1`代表第`i`天也持有股票，这就不需要更新的，而把`dp0`的值代入，正好就是不更新的。
