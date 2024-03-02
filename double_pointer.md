# 算法解释 
双指针主要用于遍历数组，两个指针指向不同的元素，协同完成任务  
核心：指针的移动逻辑
## 11. [盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/description/?envType=study-plan-v2&envId=top-100-liked)

### 要点 
优化的目标：max_area  = min(height[i], height[j]) * (j-i)  
指针的移动逻辑:移动较短的那一边
### 实现
```
    def maxArea(height):
        i = 0
        j = len(height) - 1
        max_area = 0
        while i < j:
            area = min(height[i], height[j])*(j-i)
            if area > max_area:
                max_area = area
            if height[i] < height[j]:
                i += 1
            else:
                j -= 1
        return max_area
```

## 15.[三数之和](https://leetcode.cn/problems/3sum/description/?envType=study-plan-v2&envId=top-100-liked)  
### 要点
1.暴力的解法需要O(n^3)  
2.排序后，使用双指针可将时间复杂度降低至O(n^2)  
3.去重需要对k和i，j分别处理  
4.k大于0时，直接结束搜索  
### 实现-原始版本
```
    def threeSum(nums):
        ans = []
        nums.sort()
        k_vals = []
        for k in range(len(nums)-2): #k不可能取到最后两位
            k_val = nums[k]
            if k_val > 0: #排序后k如果大于0，则i和j必大于0，不可能出现和为0
                break
            if k_val in k_vals: #相同的k对应相同的结果（去重1）
                continue
            i = k + 1
            j = len(nums)-1
            i_vals = []
            j_vals = []
            while (i < j):
                if nums[i] in i_vals: 
                    i += 1
                    continue
                if nums[j] in j_vals:
                    j -= 1
                    continue
                margin = -1 * k_val
                two_num_sum = nums[i] + nums[j]
                if two_num_sum > margin:
                    j -= 1
                elif two_num_sum < margin:
                    i += 1
                else:
                    i_vals.append(nums[i])
                    j_vals.append(nums[j]) #相同的i和j要跳过（去重2）
                    k_vals.append(k_val)
                    ans.append([k_val, nums[i], nums[j]])
                    i += 1
                    j -= 1
                    
        return ans
```
#### 问题  
k、i、j的值分别保存，再用in来去重，首先空间上多了n，其次保存，读取也增加了时间消耗
### 实现-优化后的版本
```
    def threeSum(nums):
        ans = []
        nums.sort()
        for k in range(len(nums)-2): 
            k_val = nums[k]
            if k_val > 0: 
                break
            if k > 0 and k_val == nums[k-1]: 
                continue
            i = k + 1
            j = len(nums)-1
            while (i < j):
                margin = -1 * k_val
                two_num_sum = nums[i] + nums[j]
                if two_num_sum > margin:
                    j -= 1
                    while i < j and nums[j] == nums[j+1]:
                        j -= 1
                elif two_num_sum < margin:
                    i += 1
                    while i < j and nums[i] == nums[i-1]:
                        i += 1
                else:
                    ans.append([k_val, nums[i], nums[j]])
                    i += 1
                    while i < j and nums[i] == nums[i-1]:
                        i += 1
                    j -= 1
                    while i < j and nums[j] == nums[j+1]:
                        j -= 1        
        return ans
```