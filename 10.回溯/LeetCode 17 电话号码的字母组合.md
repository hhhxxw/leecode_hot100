## 题目概述

**题目描述**：给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。

**映射关系**（类似手机九宫格）：

- 2: abc
- 3: def
- 4: ghi
- 5: jkl
- 6: mno
- 7: pqrs
- 8: tuv
- 9: wxyz

**示例**：

- 输入: "23" → 输出: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]
- 输入: "" → 输出: []
- 输入: "2" → 输出: ["a", "b", "c"]

## 解题思路

这是一个典型的**回溯/递归**问题。核心思想：

1. 为每个数字维护一个字母映射表
2. 递归遍历每个数字对应的字母
3. 当处理完所有数字时，将当前组合加入结果

时空复杂度

- **时间复杂度**：O(4^n × n)

- 最坏情况下（如"7777"），每个数字对应4个字母
- 共有 4^n 种组合，每个组合需要 O(n) 时间构建

- **空间复杂度**：O(4^n)

- 存储所有组合结果

## Java 代码实现

### 回溯法

```java
class Solution {
    public List<String> letterCombinations(String digits) {
        List<String> result = new ArrayList<>();
        
        // 边界条件
        if (digits == null || digits.length() == 0) {
            return result;
        }
        
        // 数字到字母的映射
        String[] mapping = {
            "",     // 0
            "",     // 1
            "abc",  // 2
            "def",  // 3
            "ghi",  // 4
            "jkl",  // 5
            "mno",  // 6
            "pqrs", // 7
            "tuv",  // 8
            "wxyz"  // 9
        };
        
        backtrack(digits, 0, new StringBuilder(), result, mapping);
        return result;
    }
    
    private void backtrack(String digits, int index, StringBuilder current, 
                          List<String> result, String[] mapping) {
        // 递归终止条件：已处理所有数字
        if (index == digits.length()) {
            result.add(current.toString());
            return;
        }
        
        // 获取当前数字对应的字母
        int digit = digits.charAt(index) - '0';
        String letters = mapping[digit];
        
        // 遍历该数字对应的每个字母
        for (char letter : letters.toCharArray()) {
            current.append(letter);           // 选择
            backtrack(digits, index + 1, current, result, mapping);  // 递归
            current.deleteCharAt(current.length() - 1);  // 回溯
        }
    }
}
```

