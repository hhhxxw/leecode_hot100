## 问题描述

在给定的 m × n 网格中，每个单元格可以有以下三种状态：

- `0`：空单元格
- `1`：新鲜橘子
- `2`：腐烂的橘子

每分钟，腐烂的橘子会使相邻的新鲜橘子腐烂（上下左右四个方向）。求所有橘子都腐烂需要的最少分钟数，如果无法全部腐烂则返回 -1。

**示例：**

```
输入: [[2,1,1],[1,1,0],[0,1,1]]
输出: 4

解释:
分钟 0: [[2,1,1],[1,1,0],[0,1,1]]
分钟 1: [[2,2,1],[2,1,0],[0,1,1]]
分钟 2: [[2,2,2],[2,2,0],[0,1,1]]
分钟 3: [[2,2,2],[2,2,0],[0,2,1]]
分钟 4: [[2,2,2],[2,2,0],[0,2,2]]
```

## 解题思路

这是一个多源 BFS（广度优先搜索）问题：

1. **初始化**：

- 统计新鲜橘子的总数
- 将所有腐烂的橘子加入队列（多源起点）

2. **BFS 过程**：

- 每一轮从队列中取出所有当前腐烂的橘子
- 将其相邻的新鲜橘子变为腐烂，并加入队列
- 记录时间步数，这里通过设置一个boolean变量，如果有橘子变为腐烂，则时间步数 + 1

3. **结果判断**：

- 如果最后还有新鲜橘子，返回 -1
- 否则返回所需的分钟数

**时间复杂度：** O(m × n)  
**空间复杂度：** O(m × n)

## Java代码

```java
class Solution {
    public int orangesRotting(int[][] grid) {
        if (grid == null || grid.length == 0) {
            return 0;
        }
        
        int m = grid.length;
        int n = grid[0].length;
        
        // 队列存储腐烂橘子的坐标
        Queue<int[]> queue = new LinkedList<>();
        int freshCount = 0; // 新鲜橘子计数
        
        // 第一步：初始化，找出所有腐烂的橘子和新鲜橘子数量
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 2) {
                    queue.offer(new int[]{i, j});
                } else if (grid[i][j] == 1) {
                    freshCount++;
                }
            }
        }
        
        // 如果没有新鲜橘子，直接返回 0
        if (freshCount == 0) {
            return 0;
        }
        
        // 第二步：BFS 过程
        int minutes = 0;
        int[][] directions = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        
        while (!queue.isEmpty()) {
            int size = queue.size();
            boolean hasRotted = false; // 标记本轮是否有橘子腐烂
            
            // 处理当前队列中的所有腐烂橘子
            for (int i = 0; i < size; i++) {
                int[] rotten = queue.poll();
                int row = rotten[0];
                int col = rotten[1];
                
                // 探索四个方向
                for (int[] dir : directions) {
                    int newRow = row + dir[0];
                    int newCol = col + dir[1];
                    
                    // 检查边界和是否为新鲜橘子
                    if (newRow >= 0 && newRow < m && 
                        newCol >= 0 && newCol < n && 
                        grid[newRow][newCol] == 1) {
                        
                        grid[newRow][newCol] = 2; // 变为腐烂
                        queue.offer(new int[]{newRow, newCol});
                        freshCount--;
                        hasRotted = true;
                    }
                }
            }
            
            // 如果本轮有橘子腐烂，时间加 1
            if (hasRotted) {
                minutes++;
            }
        }
        
        // 第三步：检查是否还有新鲜橘子
        return freshCount == 0 ? minutes : -1;
    }
}
```