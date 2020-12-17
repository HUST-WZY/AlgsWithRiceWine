我们从贪心的角度考虑这道题：既然不限制交易次数，同时我们是开了天眼知道这段时间的价格走势的，那么当任意两天之间的价差让我们有利可图时，就做交易。贪心交易的过程，必须假定在一天内，可以先买入再卖出，虽然这在实际情况下是不合适的，因为相同的价格买入卖出是很无聊的；但是贪心的过程为了积少成多，就要这样搞。

那么我们的思路就是遍历，当任意两天的价差为正，就交易，赚差价；所有的这些差价加一起就是最终的收益。

The time complexity is `O(n)`, space complexity is `O(1)`.

```java
public int maxProfit(int[] prices) {
    int n = prices.length;
    int ans = 0;
    for (int i = 1; i < n; i++) {
        if (prices[i] > prices[i - 1]) {
            ans += prices[i] - prices[i - 1];
        }
    }
    return ans;
}
```
