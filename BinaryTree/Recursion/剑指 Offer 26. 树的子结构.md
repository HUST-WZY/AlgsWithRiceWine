这一题是子结构，跟[子树](https://github.com/HUST-WZY/AlgsWithRiceWine/blob/main/BinaryTree/Recursion/%E9%9D%A2%E8%AF%95%E9%A2%98%2004.10.%20%E6%A3%80%E6%9F%A5%E5%AD%90%E6%A0%91.md)最大的不同在于：如果`B`是`A`的子树，那么`A`中存在一个节点，从它开始往下的整棵树，都跟`B`是相同的；但如果`B`是`A`的子结构，`A`中存在一个节点，从它开始往下，有跟`B`相同的结构，但不是说往下的整棵树都相同。

那我们其实只要改写一下[子树](https://github.com/HUST-WZY/AlgsWithRiceWine/blob/main/BinaryTree/Recursion/%E9%9D%A2%E8%AF%95%E9%A2%98%2004.10.%20%E6%A3%80%E6%9F%A5%E5%AD%90%E6%A0%91.md)中的`isSameTree()`方法即可。该方法需要去判断，`q`是否为为`p`的以`p`为根节点的子树。所以直接看代码吧，逻辑也清晰。

时间复杂度最差为`O(A * B)`，空间复杂度平均为`O(logA + logB)`。

```java
public boolean isSubStructure(TreeNode A, TreeNode B) {
    if (A == null || B == null) {
        return false;
    }
    
    if (A.val == B.val && isSameTree(A, B)) {
        return true;
    }
    
    return isSubStructure(A.left, B) || isSubStructure(A.right, B);
}

public boolean isSameTree(TreeNode p, TreeNode q) {
    if (q == null) {
        return true;
    }
    
    if (p == null) {
        return false;
    }
    
    if (p.val != q.val) {
        return false;
    }
    
    return isSameTree(p.left, q.left) && isSameTree(p.right, q.right); 
}
```
