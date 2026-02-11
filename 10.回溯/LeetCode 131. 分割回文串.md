## 问题分析

题目链接：[131. 分割回文串 - 力扣（LeetCode）](https://leetcode.cn/problems/palindrome-partitioning/description/?envType=study-plan-v2&envId=top-100-liked)

**题目要求**：给定一个字符串 `s`，将 `s` 分割成若干子串，使得每个子串都是回文串。返回所有可能的分割方案。

**例子**：

- 输入：`s = "nitin"`
- 输出：`[["n","i","t","i","n"], ["nitin"]]`

## 解题思路

**算法：**回溯

**核心思路：**

1. 定义 backtrack(startIndex) ：表示我们正在处理从 startIndex 开始到字符串末尾的子串。
2. 遍历切割点 ：从 startIndex 开始，尝试在 startIndex , startIndex+1 , ... s.length()-1 这些位置后面切一刀。

3. 当前切出来的子串是 s.substring(startIndex, i + 1) 。

4. 判断回文 ：如果这个子串是回文：

5. 把它加入当前路径 path 。
6. 递归 ：调用 backtrack(i + 1) ，去处理剩下的部分。
7. 回溯 ：从 path 中移除刚才加入的子串，尝试下一个切割点。

8. 终止条件 ：当 startIndex >= s.length() 时，说明已经切到字符串末尾了，说明找到了一种合法的分割方案。把当前 path 加入结果集 res 。

**优化点**

判断一个子串 s[i...j] 是否是回文，通常需要 O(N) 的时间。如果在回溯中反复判断，效率较低。

我们可以先用 DP 预处理出一个二维数组 isPalindrome[i][j] ，表示 s[i...j] 是否是回文。

- dp[i][j] = (s[i] == s[j]) && (j - i <= 2 || dp[i+1][j-1]) 。这样回溯里的判断就变成了 O(1)。

**时空复杂度**

- 时间 ： ![](https://cdn.nlark.com/yuque/__latex/8df64d20cb05fac00e7c8f72d79fa556.svg) —— 这是由问题本身的解空间大小决定的，无法进一步显著优化（除非不需要输出所有方案）
- 空间 ： ![](https://cdn.nlark.com/yuque/__latex/44987cdf24dd308d34c4c7af30ad5b0b.svg) —— 主要用于 DP 优化回文判断。如果不使用 DP，空间可以降到 O(N) （只用栈），但时间常数会变大（判断回文变成 O(N)）

## Java 代码实现

```java
class Solution {
    List<List<String>> res = new ArrayList<>();
    List<String> path = new ArrayList<>();
    boolean[][] dp;

    public List<List<String>> partition(String s) {
        int n = s.length();
        // 1. 动态规划预处理回文串
        dp = new boolean[n][n];
        for (int i = 0; i < n; i++) {
            Arrays.fill(dp[i], true); // 单个字符肯定是回文，先默认true或在循环里处理
        }
        
        // 从下往上，从左往右填表
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i + 1; j < n; j++) {
                // 如果两端字符相等，且内部也是回文（或者长度不超过2），则是回文
                dp[i][j] = (s.charAt(i) == s.charAt(j)) && dp[i + 1][j - 1];
            }
        }

        // 2. 开始回溯
        backtrack(s, 0);
        return res;
    }

    private void backtrack(String s, int startIndex) {
        // 终止条件：切到了字符串末尾
        if (startIndex >= s.length()) {
            res.add(new ArrayList<>(path)); // 必须new一个新list，否则path后续会被修改
            return;
        }

        // 尝试每一个切割点
        for (int i = startIndex; i < s.length(); i++) {
            // 判断 s[startIndex...i] 是否是回文
            // 如果不是回文，直接跳过（剪枝），尝试切更长一点
            if (dp[startIndex][i]) {
                String substring = s.substring(startIndex, i + 1);
                path.add(substring);
                
                // 递归处理剩下的部分
                backtrack(s, i + 1);
                
                // 回溯
                path.remove(path.size() - 1);
            }
        }
    }
}
```