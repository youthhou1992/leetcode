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

### 5.[最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/description/?envType=study-plan-v2&envId=top-100-liked)
#### 思路
1. 当i = j时：子串长度为1，是回文串
2. 当i > j时：子串非法
3. 当i < j时：dp[i][j] = dp[i+1][j-1]&&s[i] == s[j]  
dp是一个二维数组，基于上文，则有对角线上的值为True，对角线下的值为False,可以看到dp的值与其左下对角的值有关，另外其左下对角的值也是其长度减2的串，因此第一层迭代应该按照子串的长度从小到大来进行

#### 实现
```
    def longestPalindrome(s: str) -> str:
        n = len(s)
        if n < 2:
            return s
        dp = [[False for j in range(n)] for i in range(n)]
        for i in range(n):
            dp[i][i] = True  #初始化对角线的值为True
        max_len = 1
        start = 0
        for L in range(2, n+1): #子串长度从2开始
            for i in range(n):
                j = L + i -1
                if j >= n:     #右端点超出界限
                    break
                if s[i] != s[j]:
                    dp[i][j] = False
                else:
                    if j - i < 3:   #子串长度小于4，且s[i] = s[j]，则必为回文串
                        dp[i][j] = True
                    else:
                        dp[i][j] = dp[i+1][j-1]
                if dp[i][j]:        #记录最长子串的左端点和长度
                    max_len = L 
                    start = i 
        return s[start:start+max_len]
```

### 72.[编辑距离](https://leetcode.cn/problems/edit-distance/description/?envType=study-plan-v2&envId=top-100-liked)  
#### 思路
1. 首先以横轴表示word2的元素，以纵轴表示word1的元素，手动推出每个位置的值
2. 构造二维dp数组，第一维为word1的长度，第二维为word2的长度
3. 递推公式：  
(1)word1[i] == word2[j]时，dp[i][j] = dp[i-1][j-1]  
(2)word1[i] != word2[j]时，dp[i][j] = min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])
4. 递推公式依赖于dp[i-1][j], dp[i][j-1], dp[i-1][j-1],需要重新构造dp数组，相当于在word1和word2前加上空字符

#### 实现
```
    def minDistance(word1: str, word2: str) -> int:
        len1 = len(word1)
        len2 = len(word2)
        dp = [[0] * (len2+1) for i in range(len1+1)]
        for i in range(1, len2+1):
            dp[0][i] = dp[0][i-1] + 1
        for i in range(1, len1+1):
            dp[i][0] = dp[i-1][0] + 1
        for i, ch1 in enumerate(word1):
            for j, ch2 in enumerate(word2):
                if ch1 == ch2:
                    dp[i+1][j+1] = dp[i][j]
                else:
                    dp[i+1][j+1] = min(dp[i][j], dp[i][j+1], dp[i+1][j]) + 1
        return dp[-1][-1]
```

#### 不准确的实现  
##### 不通过的case：  word1 = "pneumonoultramicroscopicsilicovolcanoconiosis" ，word2 = "ultramicroscopically"  
##### 原因：  处理j=0的边界不合理  

```
    def minDistance(word1: str, word2: str) -> int:
        len1 = len(word1)
        len2 = len(word2)
        if len1 == 0:
            return len2
        if len2 == 0:
            return len1
        dp = [[0] * len2 for i in range(len1)]
        for i, ch1 in enumerate(word1):
            for j, ch2 in enumerate(word2):
                if ch1 == ch2:
                    if i == 0 and j > 0:
                        dp[i][j] = dp[0][j-1]
                    elif i > 0 and j == 0:
                        dp[i][j] = dp[i-1][0]
                    elif i == 0 and j == 0:
                        dp[i][j] = 0
                    else:
                        dp[i][j] = dp[i-1][j-1]
                else:
                    if i ==0 and j > 0:
                        dp[i][j] = dp[i][j-1] + 1
                    elif i >0 and j == 0:
                        dp[i][j] = dp[i-1][j] + 1
                    elif i == 0 and j == 0:
                        dp[i][j] = 1
                    else:
                        dp[i][j] = min(dp[i-1][j-1], dp[i-1][j], dp[i][j-1]) + 1
        return dp[-1][-1]
```

### 1143.[最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/?envType=study-plan-v2&envId=top-100-liked)  
#### 思路  
1. 以纵轴为text1的元素，以横轴为text2的元素，手动推出每个元素的值
2. 构造二维dp数组，第一维为text1的长度，第二维为text2的长度
3. 递推公式：  
text1[i] == text2[j], dp[i][j] = dp[i-1][j-1] + 1  
text1[i] != text2[j], dp[i][j] = max(dp[i-1][j], dp[i][j-1])  
4. 递推依赖dp[i-1][j-1]，需要构造dp数组维度为[len(text1)+1, len(text2)+1]

#### 实现
```
    def longestCommonSubsequence(text1: str, text2: str) -> int:
        len1, len2 = len(text1), len(text2)
        dp = [[0]*(len2+1) for i in range(len1+1)]
        for i in range(1, len1+1):
            word1 = text1[i-1]
            for j in range(1, len2+1):
                word2 = text2[j-1]
                if word1 == word2:
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    dp[i][j] = max(dp[i][j-1], dp[i-1][j])
        return dp[-1][-1]
```