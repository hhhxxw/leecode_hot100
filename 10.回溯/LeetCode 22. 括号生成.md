## 问题描述

给定 `n` 对括号，编写一个函数生成所有有效的括号组合。

**例如** `n=3` 时，输出：

```
["((()))","(()())","(())()","()(())","()()()"]
```

## 解题思路

这是一个回溯（Backtracking）问题。核心思想是：

- 在构建字符串的过程中，始终保持括号的有效性
- 只有当**左括号数量 < n** 时才能添加左括号
- 只有当**右括号数量 < 左括号数量** 时才能添加右括号

## 算法步骤

1. **初始化**：创建一个空字符串，左括号计数=0，右括号计数=0
2. **递归构建**：

- 如果左括号数 < n，尝试添加左括号
- 如果右括号数 < 左括号数，尝试添加右括号

3. **终止条件**：当左右括号数都等于 n 时，找到一个有效组合
4. **回溯**：返回上一层，尝试其他分支

复杂度分析

- **时间复杂度**：O(4^n / √n)，这是第 n 个卡特兰数
- **空间复杂度**：O(n)，递归栈的深度


## Java代码实现

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> result = new ArrayList<>();
        backtrack(result, "", 0, 0, n);
        return result;
    }
    
    private void backtrack(List<String> result, String current, 
                          int open, int close, int n) {
        // 终止条件：找到一个有效的括号组合
        if (current.length() == 2 * n) {
            result.add(current);
            return;
        }
        
        // 添加左括号：左括号数量还没达到n
        if (open < n) {
            backtrack(result, current + "(", open + 1, close, n);
        }
        
        // 添加右括号：右括号数量少于左括号数量
        if (close < open) {
            backtrack(result, current + ")", open, close + 1, n);
        }
    }
}
```


