## 题目描述

给定两个字符串 `s` 和 `p`，找到 `s` 中所有 `p` 的**字母异位词**的子串，返回这些子串的起始索引。不考虑答案输出的顺序

**字母异位词**：由相同字母重新排列形成的字符串

**示例：**

```
输入: s = "cbaebabacd", p = "abc"
输出: [0,6]
解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。

输入: s = "abab", p = "ab"
输出: [0,1,2]
解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的异位词。
```

**约束：**

- 1 <= s.length, p.length <= 3 * 10^4
- s 和 p 仅包含小写字母

---

## 解题思路

这是一道典型的**固定长度滑动窗口**问题。

### 核心思想：

1. 窗口大小固定为 `p.length()`
2. 窗口在字符串 `s` 上滑动
3. 每次检查窗口内的字符频率是否与 `p` 相同
4. 相同则记录起始索引

固定窗口+数组计数时间复杂度

- **时间复杂度**: O(n)，n为s的长度，Arrays.equals是O(26) = O(1)
- **空间复杂度**: O(1)，固定26长度的数组

### 关键点：

- **字母异位词** = 字符种类和数量完全相同
- 使用**字符频率表**比较（数组或HashMap）
- 优化：增量更新窗口，而非每次重新统计

---

## Java 完整代码实现

### 方法1：固定窗口 + 数组计数（推荐）

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> result = new ArrayList<>();
        
        int sLen = s.length();
        int pLen = p.length();
        
        // 边界判断
        if (sLen < pLen) {
            return result;
        }
        
        // 使用数组记录字符频率（只有小写字母，用26长度数组）
        int[] pCount = new int[26];
        int[] windowCount = new int[26];
        
        // 统计p中每个字符的频率
        for (char c : p.toCharArray()) {
            pCount[c - 'a']++;
        }
        
        // 初始化窗口：先加入前pLen-1个字符
        for (int i = 0; i < pLen - 1; i++) {
            windowCount[s.charAt(i) - 'a']++;
        }
        
        // 滑动窗口
        for (int right = pLen - 1; right < sLen; right++) {
            // 1. 右边界进入窗口
            windowCount[s.charAt(right) - 'a']++;
            
            // 2. 检查窗口内字符频率是否与p相同
            if (Arrays.equals(pCount, windowCount)) {
                result.add(right - pLen + 1); // 记录起始索引
            }
            
            // 3. 左边界移出窗口
            int left = right - pLen + 1;
            windowCount[s.charAt(left) - 'a']--;
        }
        
        return result;
    }
}
```

**Arrays.equals(nums1, nums1)：`Arrays.equals`是 Java 里用来比较两个数组内容是否相等****的工具方法，属于*`java.util.Arrays`，比较的是数组长度+ 对应位置的元素是否都相等（按顺序逐个比）