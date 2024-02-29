# 算法解释 
双指针主要用于遍历数组，两个指针指向不同的元素，协同完成任务  
核心：指针的移动逻辑
## 11. [盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/description/?envType=study-plan-v2&envId=top-100-liked)

### 要点 
优化的目标：max_area  = min(height[i], height[j]) * (j-i)  
指针的移动逻辑:移动较短的那一边
### 实现
```
    def maxArea(self, height):
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