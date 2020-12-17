对于股票类的动态规划问题，我们给出统一的解题思路。

```
dp[i][k][0]：截止到第i天，最多交易k次，当前手里 没有股票 了，能获得的最大收益
dp[i][k][0]：截止到第i天，最多交易k次，当前手里 有股票，能获得的最大收益
```

其中`i`从 0 开始，`买 + 卖`称为一次交易。如果一共可以交易`K`次，那么最终我们要的就是：`dp[n - 1][K][0]`。

对于递推公式，因为`买 + 卖`称为一次交易，所以`k`值的减少就有两种写法，我们统一在`卖`的时候减少：

```
dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])
dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])
```

边界条件，需要讨论`K`的情况：

```
while (i == 0 || k == 0)
	dp[i][k][0] = 0
	dp[i][k][1] = - prices[i]
```

```
dp[-1][k][0] = 0
dp[-1][k][1] = Integer.MIN_VALUE
```