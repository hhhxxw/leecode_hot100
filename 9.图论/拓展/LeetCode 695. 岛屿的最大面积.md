#dfs 
和[[LeetCode 200. 岛屿数量]]是相同的方法-DFS，去锻炼熟练度

---

# 题目概述

LeetCode 695. **岛屿的最大面积** 题目要求给定一个二维网格 `grid`，其中 `1` 代表陆地，`0` 代表水域。你需要找到并返回最大的岛屿面积。岛屿是由相邻的 `1` 组成的，陆地上的 `1` 可以通过上下左右四个方向相邻。

- 输入一个二维数组 `grid`，该数组表示一个矩阵，其中每个元素的值是 `0` 或 `1`，代表是否为陆地。
- 输出一个整数，表示最大岛屿的面积。

**示例：**

输入：

```
grid = [
  [0, 1, 0, 0, 0],
  [1, 1, 0, 0, 1],
  [0, 1, 0, 1, 1],
  [0, 0, 0, 0, 0]
]
```

输出：

```
6
```

解释：岛屿的最大面积为 `6`，它是由以下陆地组成：

```
[
  [1, 1, 0],
  [1, 1, 0],
  [1, 1, 1]
]
```

# 解题思路

1. **遍历整个网格：**

- 遍历网格中的每个单元格，遇到值为 `1`（即陆地）时，表示找到一个新的岛屿的起点。

2. **深度优先搜索（DFS）：**

- 当遇到值为 `1` 的单元格时，执行 DFS 搜索，遍历所有与该单元格相连的陆地。每次访问一个陆地时，标记为已访问（可以将它改为 `2` 或其他数字），并增加岛屿面积的计数。

3. **记录最大岛屿面积：**

- 通过 DFS 搜索得到一个岛屿的面积后，与之前的最大岛屿面积进行比较，更新最大面积。

4. **DFS 递归：**

- 对每个陆地单元格，尝试向上下左右四个方向扩展，递归地访问相邻的陆地。

5. **时间复杂度：**

- 时间复杂度为 `O(m * n)`，其中 `m` 是网格的行数，`n` 是列数。每个单元格最多访问一次。
- 空间复杂度为 `O(m * n)`

# Java代码

```java
class Solution {
    public int maxAreaOfIsland(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) {
            return 0;
        }
        int m = grid.length;
        int n = grid[0].length;
        int maxArea = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    int[] count = new int[1];  // 用数组来传递count
                    dfs(grid, i, j, count);
                    maxArea = Math.max(maxArea, count[0]);
                }
            }
        }
        return maxArea;
    }

    public void dfs(int[][] grid, int i, int j, int[] count) {
        if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length || grid[i][j] != 1) {
            return;
        }
        grid[i][j] = 2;  // 标记已访问
        count[0] += 1;  // 更新面积
        dfs(grid, i + 1, j, count);
        dfs(grid, i - 1, j, count);
        dfs(grid, i, j + 1, count);
        dfs(grid, i, j - 1, count);
    }
}
```

这里关于count使用数组的原因是因为，Java是值传递，具体可以参考[✅Java是值传递还是引用传递？](https://www.yuque.com/hollis666/zuguif/lbdoqe "✅Java是值传递还是引用传递？")