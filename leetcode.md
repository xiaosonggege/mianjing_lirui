# leetCode

## 1. 数组

**1. 在一个数组中，每一个数左边比当前数小的数累加起来，叫做这个数组的小和。求一个数组的小和。**

**例子**：

```
[1,3,4,2,5]
1左边比1小的数，没有；
3左边比3小的数，1；
4左边比4小的数，1、3；
2左边比2小的数，1；
5左边比5小的数，1、3、4、2；
所以小和为1+1+3+1+1+3+4+2=16
要求时间复杂度O(NlogN)，空间复杂度O(N)
```

**思路**

我们可以换一种思路考虑这个问题，对于每一个位置 i 的值s[i]，我们计算它比右边的几个数小或者等于，假设这样的数有 num 个，那么在最后的小和中，s[i]提供的值的总和就是s[i] * num。显而易见，将每一个位置的 s[i] * num 加起来就是最终的小和。

　　使用归并排序的过程可以达到这个目的，试想，在归并排序的组间合并过程中，左右两组都已经有序。假设左组为left[]，右组为right[]，并分别从位置 i 和 j 开始比较。如果left[i] <= right[j]，可以肯定right数组 j 位置右边的所有位置（假设有n个）的值都一定比left[i]大或等于，所以会产生一个小和left[i] * n。当然这时的n不是原数组中所有在left[i]右边比left[i]大或等于的数，因为left和right只是原数组中的两个子数组。

　　整个归并过程该怎么进行就怎么进行，排序过程没有任何变化，只是在组间合并的时候计算所有产生的小和并累加起来就是最终的结果。整个过程时间复杂度O(NlogN)。

```python
class solution:
    def getSmallSum(self, arr):
        self.res = 0
        
        if not arr:
            return 0
        arr = self.mergeSort(arr)
        return self.res, arr
    def mergeSort(self, arr):
        if len(arr) == 1:
            return arr
        mid = len(arr) // 2
        L = self.mergeSort(arr[:mid])
        R = self.mergeSort(arr[mid:])
    
        return self.merge(L, R)

    def merge(self, arr1, arr2):
        left = 0
        right = 0
        m, n = len(arr1), len(arr2)
        arr = []
        while left < m and right < n:
            if arr1[left] <= arr2[right]:
                arr.append(arr1[left])
                self.res += arr1[left] * (n - right)
                left += 1
            else:
                arr.append(arr2[right])
                right += 1
        if left < m:
            arr += arr1[left:]
        else:
            arr += arr2[right:]
        return arr
```

