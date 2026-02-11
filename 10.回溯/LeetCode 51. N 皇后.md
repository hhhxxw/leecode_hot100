# 题目概述

**题目描述**：  
将 `n` 个皇后放置在 `n x n` 的棋盘上，使得它们彼此之间不能相互攻击。

- **皇后攻击规则**：皇后可以攻击同一行、同一列或同一条对角线上的任何棋子。
- **目标**：返回所有不同的解决方案。每一种解法包含一个明确的 `n` 皇后放置方案，其中 `'Q'` 和 `'.'` 分别代表皇后和空位。

**示例** (`n=4`)：

```
输出: [
 [".Q..",  // 方案1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // 方案2
  "Q...",
  "...Q",
  ".Q.."]
]
```

---

# 解题思路

算法：**回溯**

核心逻辑是**一行一行地放皇后**。  
因为规则限制“同一行只能有一个皇后”，所以我们可以直接遍历行 `row`。在每一行中，我们尝试在每一列 `col` 放置皇后。

#### 1. 递归函数定义

`backtrack(row)`: 尝试在第 `row` 行放置皇后。

#### 2. 约束条件（剪枝）

当我们尝试在 `(row, col)` 放置皇后时，必须检查：

- **列冲突**：第 `col` 列之前有没有放过皇后？
- **主对角线冲突 (左上到右下)**：之前的行有没有皇后在对角线上？

- 主对角线特征：`row - col` 是常数。

- **副对角线冲突 (右上到左下)**：之前的行有没有皇后在对角线上？

- 副对角线特征：`row + col` 是常数。

#### 3. 状态记录

为了快速检查上述冲突，我们可以用三个布尔数组（或 Set）来记录：

- `cols[n]`：记录哪一列被占用了。
- `diag1[2*n]`：记录主对角线 `row - col`（注意要加偏移量 `n` 防止负数）。
- `diag2[2*n]`：记录副对角线 `row + col`。

#### 4. 回溯流程

- **选择**：在 `(row, col)` 放皇后，更新三个状态数组，把 `Q` 放入当前棋盘状态。
- **递归**：`backtrack(row + 1)`。
- **撤销**：把 `Q` 拿走，还原三个状态数组（回溯到上一步，尝试下一列）。

---

# Java 代码

```java
class Solution {
    List<List<String>> res = new ArrayList<>();
    // 用数组记录列和对角线的占用情况
    boolean[] cols;
    boolean[] diag1; // 主对角线: row - col
    boolean[] diag2; // 副对角线: row + col
    
    public List<List<String>> solveNQueens(int n) {
        cols = new boolean[n];
        // 对角线数量是 2*n - 1。diag1 索引范围是 -n 到 n，加 n 偏移量变为 0 到 2n
        diag1 = new boolean[2 * n]; 
        diag2 = new boolean[2 * n];
        
        // 用于构建棋盘，先填满 '.'
        char[][] board = new char[n][n];
        for (char[] row : board) {
            Arrays.fill(row, '.');
        }
        
        backtrack(board, 0, n);
        return res;
    }

    private void backtrack(char[][] board, int row, int n) {
        // 终止条件：成功放完了 n 行
        if (row == n) {
            res.add(construct(board));
            return;
        }

        // 尝试在当前行的每一列放置皇后
        for (int col = 0; col < n; col++) {
            // 检查冲突
            // row - col + n 是为了把负数索引转正
            if (cols[col] || diag1[row - col + n] || diag2[row + col]) {
                continue; // 剪枝：如果不合法，直接跳过
            }

            // 1. 做选择
            board[row][col] = 'Q';
            cols[col] = true;
            diag1[row - col + n] = true;
            diag2[row + col] = true;

            // 2. 递归下一行
            backtrack(board, row + 1, n);

            // 3. 撤销选择 (回溯)
            board[row][col] = '.';
            cols[col] = false;
            diag1[row - col + n] = false;
            diag2[row + col] = false;
        }
    }

    // 辅助方法：将 char[][] 转为 List<String>
    private List<String> construct(char[][] board) {
        List<String> list = new ArrayList<>();
        for (char[] row : board) {
            list.add(new String(row));
        }
        return list;
    }
}
```

### 关键点总结

1. **空间换时间**：使用 `boolean[]` 数组记录列和对角线的占用情况，使得检查冲突的时间复杂度从 O(N) 降为 **O(1)**。这是 N 皇后问题性能优化的关键。
2. **对角线公式**：

- **主对角线** (`\`): `row - col` 恒定。
- **副对角线** (`/`): `row + col` 恒定。

3. **回溯模板**：

- `if (满足条件) 记录结果`
- `for (选择 in 选择列表)`

- `if (不合法) continue`
- `做选择`
- `backtrack()`
- `撤销选择`