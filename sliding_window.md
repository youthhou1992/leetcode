# [算法解释](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/solution/hua-dong-chuang-kou-tong-yong-si-xiang-jie-jue-zi-/)
```
/* 滑动窗口算法框架 */
def slidingWindow(s, t):
    need = dict()
    window = dict()
    for ch in p:
        if ch not in need.keys():
            need[ch] = 0
        need[ch] += 1
    
    left = 0
    right = 0
    ans = []
    valid = 0
    while right < len(s):
        c = s[right] #将移入窗口的字符
        right += 1 #右移窗口

        #窗口内数据的一系列更新
        ...

        #判断左侧窗口是否要收缩，即是否移动左指针
        while window needs shrink:
            
            d = s[left] #将移出窗口的字符
            left += 1
            #窗口内数据的一系列更新
            ...
```  

## 3.[无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/description/?envType=study-plan-v2&envId=top-100-liked)

### 要点  
1.重复字符的判断（哈希表：字典或集合）  
2.根据右指针和左指针的间隔长度，动态的计算最长的子串 
3.移动左指针的逻辑：出现重复字符，且该字符的位置在左指针的右边，移动到左指针到该字符的位置；如果少了右边的判定，会出现向左滑动的场景，则左右指针之间可能出现重复字符  

### 实现  
```
    def lengthOfLongestSubstring(s):
        max_len = 0
        ch = dict()
        start = -1
        for end in range(len(s)):
            if s[end] in ch.keys() and ch[s[end]] >= start:
                start = ch[s[end]]
            max_len = max(end - start, max_len)
            ch[s[end]] = end
        return max_len
```
## 438.[找到字符串中所有字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string/description/?envType=study-plan-v2&envId=top-100-liked)
### 要点  
1.异位词判断：按照出现频率建立字符-频数的哈希，并判断字符的频数相等
### 实现
```
    def findAnagrams(s, p):
        need = dict()
        window = dict()
        for ch in p:
            if ch not in need.keys():
                need[ch] = 0
            need[ch] += 1
        
        left = 0
        right = 0
        ans = []
        valid = 0
        while right < len(s):
            c = s[right]
            right += 1 #移动右指针
            if c in need.keys(): #如果当前字符在p中，窗口中该字符数量加1，判断该字符是否与p中字符数量一致
                if c not in window.keys():
                    window[c] = 0
                window[c] += 1
                if window[c] == need[c]:
                    valid += 1

            while right - left >= len(p): #窗口大小等于p时
                if valid == len(need):    #是否满足异位词条件
                    ans.append(left)
                d = s[left]               #移动左指针
                left += 1
                if d in need.keys():      #移动左指针涉及到window和valid状态的修改
                    if window[d] == need[d]:
                        valid -= 1
                    window[d] -=1
        return ans
            
```

## 76.[最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/) 
### 实现
```
    def minWindow(self, s: str, p: str) -> str:
        need = dict()
        window = dict()
        for ch in p:
            if ch not in need.keys():
                need[ch] = 0
            need[ch] += 1
        
        left = 0
        right = 0
        ans = ''
        min_len = 999999
        valid = 0
        while right < len(s):
            c = s[right]
            right += 1
            if c in need.keys():
                if c not in window.keys():
                    window[c] = 0
                window[c] += 1
                if window[c] == need[c]:
                    valid += 1

            while valid == len(need):
                if right - left < min_len:
                    ans = s[left:right]
                    min_len = right - left
                d = s[left]
                left += 1
                if d in need.keys():
                    if window[d] == need[d]:
                        valid -= 1
                    window[d] -=1
        return ans
            
```