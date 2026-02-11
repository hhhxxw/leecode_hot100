# 题目概述

**题目描述**：  
给定一个 `m x n` 的字符网格 `board` 和一个字符串单词 `word`。  
如果 `word` 存在于网格中，返回 `true`；否则，返回 `false`。

**规则**：

- 单词必须按照字母顺序，通过相邻的单元格内的字母构成。
- “相邻”单元格是那些水平相邻或垂直相邻的单元格。
- 同一个单元格内的字母**不允许被重复使用**。

**示例**：

```
board = [
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]
word = "ABCCED" -> 返回 true
word = "SEE"    -> 返回 true
word = "ABCB"   -> 返回 false
```

---

# 解题思路

相关算法：**回溯算法 (Backtracking)** / **深度优先搜索 (DFS)**

#### 核心逻辑

1. **遍历起点**：我们需要在网格中找到单词的第一个字母。遍历整个网格，一旦找到 `board[i][j] == word[0]`，就从这个位置开始进行 DFS 搜索。
2. **DFS 递归**：

- **终止条件 (成功)**：如果当前已经匹配到了单词的最后一个字母（索引 `index == word.length() - 1`），说明找到了，返回 `true`。
- **终止条件 (失败)**：

- 越界：`i` 或 `j` 超出了网格范围。
- 不匹配：当前格子字符 `board[i][j]` 不等于单词当前要找的字符 `word[index]`。
- 已访问：当前格子已经被用过了（防止走回头路）。

- **递归过程**：

- **标记访问**：为了不重复使用，先把当前格子标记一下（通常是改成一个特殊字符，比如 `#`，或者用一个 `visited` 数组）。
- **四个方向探索**：向 上、下、左、右 四个方向递归调用 DFS，寻找单词的下一个字符 `index + 1`。
- **回溯 (Backtrack)**：如果四个方向都没找到，说明这条路不通。**一定要把当前格子改回原来的字符**，以便其他路径还能用它。

3. **剪枝优化**：

- 如果单词长度大于网格总格子数，直接 false。
- 可以先统计网格中各字符数量，如果单词里某个字符的数量比网格里的还多，直接 false（虽然面试时不一定要写这个，但这是很好的优化点）。

---

# Java 代码实现

```java
class Solution {
    public boolean exist(char[][] board, String word) {
        int m = board.length;
        int n = board[0].length;

        // 遍历每一个格子作为起点
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // 如果 DFS 返回 true，说明找到了
                if (dfs(board, word, i, j, 0)) {
                    return true;
                }
            }	
        }
        return false;
    }

    /**
     * @param i 当前行
     * @param j 当前列
     * @param index 当前正在匹配 word 的第几个字符
     */
    private boolean dfs(char[][] board, String word, int i, int j, int index) {
        // 1. 越界检查 或 字符不匹配
        if (i < 0 || i >= board.length || j < 0 || j >= board[0].length || board[i][j] != word.charAt(index)) {
            return false;
        }

        // 2. 成功条件：已经匹配到了最后一个字符
        if (index == word.length() - 1) {
            return true;
        }

        // 3. 标记访问 (防止走回头路)
        char temp = board[i][j];
        board[i][j] = '#'; // 用特殊字符标记为已访问

        // 4. 递归四个方向 (上、下、左、右)
        // 只要有一个方向能走通，就返回 true
        boolean found = dfs(board, word, i + 1, j, index + 1) ||
                        dfs(board, word, i - 1, j, index + 1) ||
                        dfs(board, word, i, j + 1, index + 1) ||
                        dfs(board, word, i, j - 1, index + 1);

        // 5. 回溯 (恢复现场)
        board[i][j] = temp;

        return found;
    }
}
```

### 关键点总结

1. **原地修改 (In-place modification)**：我们直接修改 `board[i][j]` 为 `'#'` 来标记已访问，比单独开一个 `visited[][]` 数组更省空间。记得最后改回来。
2. **短路逻辑**：在递归调用时，使用 `||` 连接四个方向。只要有一个返回 `true`，后面的就不会执行了，这能提升效率。
3. **边界条件**：先判断 `index == word.length() - 1`，再判断字符是否匹配（或者反过来也行，但要注意逻辑顺序）。上面的代码是先判断不匹配/越界，再判断是否到达末尾。