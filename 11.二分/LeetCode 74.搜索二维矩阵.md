# 题目理解
给定一个 m × n 的整数矩阵 matrix，矩阵具有以下特性：

- 每行中的整数从左到右按升序排列
- 每列中的整数从上到下按升序排列

在矩阵中搜索一个目标值，如果存在返回 `true`，否则返回 `false`。

**示例：**

```
matrix = [[1,4,7,11,15],
          [2,5,8,12,19],
          [3,6,9,16,22],
          [10,13,14,17,24],
          [18,21,23,26,30]]
target = 5 → true
target = 13 → true
target = 20 → false
```

# 解题思路

### 从右上角或左下角开始

- 从矩阵的右上角（或左下角）开始
- 如果当前值等于目标，返回 true
- 如果当前值大于目标，向左移动
- 如果当前值小于目标，向下移动

时空复杂度

- 时间复杂度：O(m + n)
- 空间复杂度：O(1)

## Java 代码实现

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0) {
            return false;
        }
        
        int m = matrix.length;    // 行数
        int n = matrix[0].length; // 列数
        
        // 从右上角开始
        int row = 0;
        int col = n - 1;
        
        while (row < m && col >= 0) {
            if (matrix[row][col] == target) {
                return true;
            } else if (matrix[row][col] > target) {
                col--;  // 当前值太大，向左移动
            } else {
                row++;  // 当前值太小，向下移动
            }
        }
        
        return false;
    }
}
```

这种解法和二分的思路基本一致