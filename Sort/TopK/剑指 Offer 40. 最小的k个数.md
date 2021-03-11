对 [基于快排的TopK](https://github.com/HUST-WZY/AlgsWithRiceWine/blob/main/Sort/TopK/215.%20%E6%95%B0%E7%BB%84%E4%B8%AD%E7%9A%84%E7%AC%ACK%E4%B8%AA%E6%9C%80%E5%A4%A7%E5%85%83%E7%B4%A0.md) 的理解，我们知道：我们可以改变数组，使得对于索引`pIndex`，它前面的数都小于它；而要找“第`k`小的数”，只需要令`pIndex == k - 1`。此时，数组从索引`0`到索引`k - 1`，就是数组中最小的`k`个数。而题目又不要求返回结果排序，就很简单了。

```java
public int[] getLeastNumbers(int[] arr, int k) {

    quickSelect(arr, 0, arr.length - 1, k - 1);
    
    int[] ans = new int[k];
    for (int i = 0; i < k; i++) {
        ans[i] = arr[i];
    }
    return ans;
}

private static final Random random = new Random();

public void quickSelect(int[] nums, int lo, int hi, int index) {
    if (lo >= hi) {
        return;
    }
    
    int pIndex = partition(nums, lo, hi);
    
    if (pIndex < index) {
        quickSelect(nums, pIndex + 1, hi, index);
    }
    if (pIndex > index) {
        quickSelect(nums, lo, pIndex - 1, index);
    }
}

private int partition(int[] nums, int lo, int hi) {
    int r = lo + random.nextInt(hi - lo + 1);
    swap(nums, lo, r);
    int pivot = nums[lo];
    
    int lt = lo + 1, gt = hi;
    while (true) {
        while (lt < hi && pivot > nums[lt]) {
            lt++;
        }
        while (gt > lo && pivot < nums[gt]) {
            gt--;
        }
        
        if (lt >= gt) {
            break;
        }
        
        swap(nums, lt, gt);
        lt++;
        gt--;
    }
    
    swap(nums, gt, lo);
    
    return gt;
}

private void swap(int[] nums, int i, int j) {
    int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
}
```
