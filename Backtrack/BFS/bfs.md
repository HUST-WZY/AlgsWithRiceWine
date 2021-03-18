BFS 同样需要讨论是否需要使用`visited`去重。但是我们这里就不写不需要用`visited`的情况了，基本上跟需要的差不多。

> 只需要明确，我们要在`元素`真正进入`队列`之前，去维护它的`visited`

## 一个一个遍历

bfs 是一层一层来的，但是如果整体来看，也可以认为它的一个元素一个元素遍历的。大多时候需要这样。

```java
public void bfs (pointStart, pointTarget) {
    
    Queue q;
    q.offer(pointStart);
    Set visited;
    visited.add(pointStart);
    
    while (!q.isEmpty()) {
    	
        pointCur = q.poll();
    	
        if (pointCur == pointTarget) {
            return;
        }
        
    	for (pointNeibor : pointCur.neighbors()) {
            if (visited.contains(pointNeibor)) {
                continue;
            }
            visited.add(pointNeibor);
            
            q.offer(pointNeibor);
        }
        
    }
}
```

## 一层一层遍历

有时，比如要数一下树的层数，就需要这样一层一层地遍历。

```java
public void bfs (pointStart, pointTarget) {
    
    Queue q;
    q.offer(pointStart);
    Set visited;
    visited.add(pointStart);
    int depth = 0;
    
    while (!q.isEmpty()) {
        
        int n = q.size();
    	
        for (int i = 0; i < n; i++) {
            
            pointCur = q.poll();
    	
            if (pointCur == pointTarget) {
                return;
            }

            for (pointNeibor : pointCur.neighbors()) {
                if (visited.contains(pointNeibor)) {
                    continue;
                }
                visited.add(pointNeibor);
                
                q.offer(pointNeibor);
            }   
        }
        depth++;
    }
}
```

## 双向 bfs

当我们知道遍历的起点，也知道遍历的终点时，就可以考虑用双向 bfs。一般来说双向比单向要快。

> 请注意，之前不管是 [dfs](https://github.com/HUST-WZY/AlgsWithRiceWine/blob/main/Backtrack/DFS/dfs.md)还是前面的 bfs，标记已经访问的代码都是在往下一个节点走的时候，或者说是在真正进入下一个节点之前就做了，这种也需要提前将初始节点做标记。
>
> 但对于双向 bfs，必须在真正进入某个节点时，在给它标记已经访问。那也就不需要提前给初始节点做标记了。

```java
public void bfs (Node pointStart, Node pointTarget) {
    Set q1;
    Set q2;
    q1.add(pointStart);
    q2.add(pointTarget);
    
    Set visited;
    
    while (!q1.isEmpty() && !q2.isEmpty()) {
        Set temp;
        for (cur : q1) {
            
            visited.add(cur);  // 
            
            if (q2.contains(cur)) {
                return;
            }
            
            for (neibor : cur.neighbors()) {
                if (visited.contains(neibor)) {
                    continue;
                }
                temp.add(neibor);
            }
        }
        
        if (temp.size() > q2.size()) {
            q1 = q2;
            q2 = temp;
        } else {
            q1 = temp;
        }
    }
}
```
