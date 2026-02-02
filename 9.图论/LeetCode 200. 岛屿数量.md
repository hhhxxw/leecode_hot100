#dfs
## 问题描述

给定一个由 '1'（陆地）和 '0'（水）组成的二维网格，计算岛屿的数量。岛屿由水平或竖直方向相邻的陆地连接而成，且被水完全包围。

**示例：**

```
输入:
1 1 0 0 0
1 1 0 0 0
0 0 1 0 0
0 0 0 1 1

输出: 3
```

## 解题思路

dfs:

- 遍历网格的每个单元格
- 当遇到 '1' 时，进行 DFS 递归探索所有相邻的陆地
- 将访问过的陆地标记为 '0'，避免重复计算
- 每次 DFS 完成时，岛屿数量加 1

时空复杂度：

- **时间复杂度：** O(m × n)
- **空间复杂度：** O(m × n)（递归栈深度）

## Java 代码实现

```java
class Solution {
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0) {
            return 0;
        }
        
        int m = grid.length;
        int n = grid[0].length;
        int count = 0;
        
        // 遍历每个单元格
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // 发现陆地，进行DFS探索
                if (grid[i][j] == '1') {
                    dfs(grid, i, j);
                    count++;
                }
            }
        }
        
        return count;
    }
    
    // DFS递归函数
    private void dfs(char[][] grid, int i, int j) {
        // 边界检查和陆地检查
        if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length || grid[i][j] == '0') {
            return;
        }
        
        // 标记为已访问
        grid[i][j] = '0';
        
        // 探索四个方向
        dfs(grid, i + 1, j); // 下
        dfs(grid, i - 1, j); // 上
        dfs(grid, i, j + 1); // 右
        dfs(grid, i, j - 1); // 左
    }
}
```