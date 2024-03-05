# 算法解释

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
