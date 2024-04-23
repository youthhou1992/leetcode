# 贪心算法
### 55 [跳跃游戏](https://leetcode.cn/problems/jump-game/?envType=study-plan-v2&envId=top-100-liked)
#### 思路
1. 问题归类到贪心算法问题
2. 贪心算法特点：每一步选择当前状态下最有利的选择，可以得到最好或最优的全局解
3. 核心思想：遍历每一个位置，如果无法到达当前位置，则一定无法到达最终位置
3. 计算每个位置能到达的最远位置，如果当前位置的索引大于之前位置能到达的最远位置，即无法到达当前位置

#### 实现
```
    def canJump(nums: List[int]) -> bool:
        max_right = nums[0]
        for i in range(len(nums)):
            if i > max_right:
                return False
            max_right = max(max_right, nums[i]+i)
        return True
```

### 45 [跳跃游戏2](https://leetcode.cn/problems/jump-game-ii/description/?envType=study-plan-v2&envId=top-100-liked)
#### 思路
1. 每一跳尽可能跳的最远
2. 每一跳的起始位置都在上一跳的可跳范围内

#### 实现
```
    def jump(nums: List[int]) -> int:
        step = 0
        end = 0 
        max_pos = 0
        for i in range(len(nums)-1):
            max_pos = max(max_pos, nums[i] + i)
            if i == end:
                end = max_pos
                step += 1
        return step
```