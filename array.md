# 普通数组
## 53.[最大子数组和](https://leetcode.cn/problems/maximum-subarray/?envType=study-plan-v2&envId=top-100-liked)
### 要点  
1. 跟560同样的思路，计算每个位置的前缀和prev[]，则当前位置j的最大的连续子数组[i,j],满足prev[i]为最小值  
### 实现
```
    def maxSubArray(nums: List[int]) -> int:
        max_sum = -9999999
        min_prev = 0
        prev = 0
        for i in range(len(nums)):
            array_sum = 0
            prev += nums[i]               #计算前缀和
            array_sum = prev - min_prev   #当前位置最大的连续子数组的和
            if array_sum > max_sum:
                max_sum = array_sum       #更新全局最大连续子数组和
            if prev < min_prev:           #更新最小的前缀和
                min_prev = prev
        return max_sum
```

## 56.[合并区间](https://leetcode.cn/problems/merge-intervals/description/?envType=study-plan-v2&envId=top-100-liked)
### 要点 
1. 排序
2. 两个区间的交集有三种情况：相交，包含，不相交

### 实现
```
    def merge(intervals: List[List[int]]) -> List[List[int]]:
        intervals.sort(key=lambda x: x[0])
        ans = []
        start = intervals[0][0]
        end = intervals[0][1]
        for inter in intervals[1:]:
            if inter[0] <= end:
                end = max(inter[1], end)
            else:
                ans.append([start, end])
                start = inter[0]
                end = inter[1]
        ans.append([start, end])
        return ans    
```

## 189.[轮转数组](https://leetcode.cn/problems/rotate-array/description/?envType=study-plan-v2&envId=top-100-liked)
### 思路
1. 对k按照nums的长度来取模，计算真正需要轮转的位置个数
2. 取后k个数和前[0,-k]的数，并拼接
### 要点
1. 为什么nums = nums[-k:] + nums[:-k]不行？  
 python中的变量都是对象，对象有两个基本属性：value和id，原位修改指的是运行完毕后，nums的id不变。nums[:]=xxx的赋值方式不会改变nums的id值，而nums=xxx的方式会改变nums的地址
 
 ### 实现
 ```
    def rotate(nums: List[int], k: int) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        k = k % len(nums)
        nums[:] = nums[-k:] + nums[:-k]
 ```

 ## 238.[除自身以外数组的乘积](https://leetcode.cn/problems/product-of-array-except-self/description/?envType=study-plan-v2&envId=top-100-liked)
 ### 思路  
 1. 从后往前，计算每个位置的后缀积
 2. 从前往后，计算每个位置的前缀积和前缀积与后缀积的积，即为结果  

 ### 实现
 ```
     def productExceptSelf(nums: List[int]) -> List[int]:
        post = [0 for i in range(len(nums))]
        cum = 1
        for i in range(len(nums)-1, -1, -1):
            post[i] = cum
            cum *= nums[i]
        ans = []
        cum = 1
        for i in range(len(nums)):
            ans.append(cum*post[i])
            cum *= nums[i]
        return ans
 ```