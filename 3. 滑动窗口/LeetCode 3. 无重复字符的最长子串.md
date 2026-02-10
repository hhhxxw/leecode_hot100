处理的是字串问题，可以用滑动窗口来解决，属于长度不固定的滑动窗口问题

## 题目描述

给定一个字符串 `s`，请你找出其中不含有重复字符的**最长子串**的长度

**示例：**

```
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是子串的长度，"pwke" 是一个子序列，不是子串。
```

子串必须是连续的，子序列不一定

## 解题思路

这是一道典型的**滑动窗口**问题

### 核心思想：

1. 使用**左右双指针**维护一个窗口
2. 右指针不断向右扩展，将新字符加入窗口
3. 当窗口中出现重复字符时，左指针右移缩小窗口，直到窗口内无重复字符
4. 每次移动后更新最大长度

这里窗口用Map<Character, Integer> window用来统计字符出现的次数。

时空复杂度

- **时间复杂度**: O(n)，每个字符最多被访问两次（right和left各一次）
- **空间复杂度**: O(min(m, n))，m是字符集大小，n是字符串长度

## Java代码

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        // 使用HashMap存储字符及其出现次数
        Map<Character, Integer> window = new HashMap<>();
        
        int left = 0, right = 0;
        int maxLen = 0;
        
        while (right < s.length()) {
            // 1. 右指针扩展：将right指向的字符加入窗口
            char c = s.charAt(right);
            right++;
            window.put(c, window.getOrDefault(c, 0) + 1);
            
            // 2. 当窗口中出现重复字符时，收缩左边界
            while (window.get(c) > 1) {
                char d = s.charAt(left);
                left++;
                window.put(d, window.get(d) - 1);
            }
            
            // 3. 更新最大长度
            maxLen = Math.max(maxLen, right - left);
        }
        
        return maxLen;
    }
}
```