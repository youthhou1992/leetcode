# 动态规划
## 一维动态规划  
### 118.[杨辉三角](https://leetcode.cn/problems/pascals-triangle/description/?envType=study-plan-v2&envId=top-100-liked)
#### 思路
1. 第i行的元素数目为i
2. 每一行的首元素和尾元素为1
3. 每一行除首尾元素外，状态转移方程：dp[i][j] = dp[i-1][j-1] + dp[i-1][j]
#### 实现
```
    def generate(numRows: int) -> List[List[int]]:

        dp = [[1]]
        for i in range(1, numRows):
            i += 1
            arr = []
            for j in range(i):
                if j == 0 or j == i-1:
                    arr.append(1)
                else:
                    arr.append(dp[-1][j-1] + dp[-1][j])
            dp.append(arr)
        return dp
```
### 152.[乘积最大子数组](https://leetcode.cn/problems/maximum-product-subarray/description/?envType=study-plan-v2&envId=top-100-liked)
#### 思路
1. 状态转移方程：dp[i] = max(nums[i]*pre_max, nums[i]*pre_min, nums[i])
#### 实现
```
    def maxProduct(nums: List[int]) -> int:
        maxi, mini, ans = nums[0], nums[0], nums[0]
        for num in nums[1:]:
            maxi, mini = max(maxi*num, mini*num, num),min(maxi*num, mini*num, num)
            ans = max(maxi, ans)
        return ans
```
## 二维动态规划
### 62.[不同路径](https://leetcode.cn/problems/unique-paths/description/?envType=study-plan-v2&envId=top-100-liked)
#### 思路  
1. 第1行的值全为1  
2. 第一列的值全为1  
3. 状态转移方程：dp[i][j] = dp[i-1][j] + dp[i][j-1]
4. i行的值只与i-1行有关，基于此优化空间
#### 实现
```
    def uniquePaths(m: int, n: int) -> int:
        dp = [1 for k in range(n)] # 只保留i-1行的值
        for i in range(1,m):
            for j in range(n):
                if j == 0:
                    continue
                dp[j] = dp[j] + dp[j-1]
        return dp[-1]
```