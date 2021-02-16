我们把`dfs`也要分成两种情况：需要回溯的和不需要回溯的。如何判断是否需要呢？最简单的判断方法是：看我们要遍历的那些点，每个点，是否是只遍历一次。如果只遍历一次，那就不需要回溯；如果遍历多次，就需要回溯。

时间复杂度为`O(n ^ M)`，其中`n`为在递归的每一层，需要遍历的点的个数；`M`为递归的层数；空间复杂度为`O(n ^ M)`，`visited`或者递归用到的系统栈。

## 不需要回溯

不需要回溯的情况有两个特点，这应该也是`dfs`区别于`回溯`的地方：

* 所有元素都只会被访问一次

* 先进入到某个元素的`dfs`递归中，再判断是否重复访问

### 不需要手动去重

对于树形结构，天然的是从根节点到叶子节点的单向路径，不可能是到了叶子节点后，再往上回到根节点。所以这种结构的数据就无需手动去重，无需`visited`数组、集合。那么类似的，在有向图中，也是不需要手动去重的。

* 二叉树

```java
public void dfs (TreeNode root) {
    if (root == null) {
        return;
    }
    // 对 root 进行操作
    dfs(root.left);
    dfs(root.right);
}
```

* N 叉树

```java
public void dfs (TreeNode root, int n) {
    if (root == null) {
        return;
    }
    // 对 root 进行操作
    for (int i = 0; i < n; ++i) {
        dfs(root.next(i), n);
    }
}
```

### 需要手动去重

作为对比，数据结构中没有明确的方向指向的话，如无向图，就需要去重了。

* 无向图


```java
public void dfs(Node u, Set<Node> visited) {
    if (u == null) {
        return;
    }
    
    if (visited.contains(u)) {
        return;
    }
    visited.add(u);
    
    // 对 u 进行操作
    
    for (Node v : u.neibor()) {
        dfs(v, visited);
    }
}
```

* 矩阵数据

```java
private int[][] directs = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

public void dfs(int[][] nums, int r, int c, boolean[][] visited) {
    int m = nums.length;
    int n = nums[0].length;
    if (r == m - 1 && c == n - 1) {
        return;
    }
    
    if (visited[r][c]) {
        return;
    }
    visited[r][c] = true;
    
    // 对 r c 处的元素进行操作
    
    for (int[] direct : directs) {
        int newR = r + direct[0];
        int newC = c + direct[1];
        if (!inRange(newR, newC, n)) {
            continue;
        }
        
        dfs(nums, newR, newC, visited);
    }
}

private boolean inRange(int r, int c, int n) {
    return r >= 0 && r < n && c >= 0 && c < n;
}
```

对于矩阵来说，还有一种手动去重的方法：比如，如果我们知道矩阵中元素的值都应该是正值，那就当我们遍历过一个位置之后，就把它的值设为`-1`；那每次在遍历之前，就先判断一下当前元素的是否为正值。

```java
private int[][] directs = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

public void dfs(int[][] nums, int r, int c) {
    int m = nums.length;
    int n = nums[0].length;
    if (r == m - 1 && c == n - 1) {
        return;
    }
    
    if (nums[r][c] == -1) {
        return;
    }
    
    // 对 r c 处的元素进行操作
    
    nums[r][c] = -1;
    
    for (int[] direct : directs) {
        int newR = r + direct[0];
        int newC = c + direct[1];
        if (!inRange(newR, newC, n)) {
            continue;
        }
        
        dfs(nums, newR, newC);
    }
}

private boolean inRange(int r, int c, int n) {
    return r >= 0 && r < n && c >= 0 && c < n;
}
```

## 需要回溯

* 所有元素会被访问多次

* 先判断是否重复访问，没有的话，再进入到某个元素的`dfs`递归中

当数据结构中的元素需要被遍历多次，就需要回溯了。需要回溯的肯定要是 需要手动去重 的情况，而且很简单，只要把`visited`还原一下即可，以无向图为例，只需要改变判断重复的位置，然后添加一行代码。

大多数情况下，感觉还是不需要回溯的。

```java
private Set<Node> visited = new HashSet<>();
visited.add(startNode);

public void dfs(Node u) {
    if (u == null) {
        return;
    }
    
    for (Node v : u.neibor()) {
        if (visited.contains(v)) {
            continue;
        }
        visited.add(v);
        dfs(v);
        visited.remove(v);  //
    }
}
```
