## 560.[和为k的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/description/?envType=study-plan-v2&envId=top-100-liked)

### 要点  
1. 暴力枚举时间复杂度O(n^2)可能会超时  
2. 第一层循环无法避免，第二层循环涉及大量重复计算，存在优化空间  
(1). 定义 pre[i]为[0..i]里所有数的和，则pre[i]可以由pre[i-1]递推而来，即：  
  pre[i] = pre[i-1] = nums[i]  
那么[j...i]子数组的和为k，可以转化为：  
  pre[i] - pre[j-1] = k  
则考虑以i为纪委的和为k的子数组个数之需统计由多少个前缀和为pre[i]-k的pre[j]即可，利用哈希表可以快速获取前缀和为pre[i]-k的pre[j]的个数  
### 实现
1. 暴力枚举
```
    def subarraySum(nums, k):
        count = 0
        for i in range(len(nums)):
            array_sum = 0
            for j in range(i, -1, -1):
                array_sum += nums[j]
                if array_sum == k:
                    count += 1
        return count
```
2. 前缀和+哈希  
```
    def subarraySum(nums: List[int], k: int) -> int:
        mp = dict()
        mp[0] = 1
        count = 0
        prev = 0
        for i in range(len(nums)):
            prev += nums[i]
            margin = prev - k
            if margin in mp.keys():
                count += mp[margin]
            if prev not in mp.keys():
                mp[prev] = 0
            mp[prev] += 1
        return count
```