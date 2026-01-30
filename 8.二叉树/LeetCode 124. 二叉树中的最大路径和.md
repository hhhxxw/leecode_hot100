## 问题理解

**题目要求**：找到二叉树中的最大路径和。路径可以从任意节点开始，到任意节点结束，不一定经过根节点。

**关键点**：

- 路径必须是连续的（父子关系）
- 可以只包含单个节点
- 节点值可能为负数

## 解题思路

对于每一个结点，需要计算两个东西，一个是经过这个结点的最大路径和`currentSum`，另一个是从该结点向下延伸的最大路径和(函数返回值)；具体来说，可以设计一个函数`fun(node)`, 返回从结点向下延伸的最大路径和，其中递归的边界是结点为null(返回0)，首先计算经过这个结点的最大路径和`currentSum`，由递归思想，先处理其左右子结点，这里考虑到路径和可能为负数，所以计算计算`leftMax = Math.max(0, fun(node.left))`, `rightMax = Math.max(0, fun(node.right))`，经过这个结点的最大路径和`currentMax = node.val + leftMax + rightMax`，然后就就可以更新全局最大值`maxSum = **Math.max(**maxSum**,** currentMax)`， 最后`fun`返回的是以该结点为起点，向下延伸的最大路径和：`node.val + Math.max(leftMax + rightMax)`

### 核心思想：递归 + DFS

对于每个节点，我们需要计算：

1. **经过该节点的最大路径和**（可能是答案）
2. **从该节点向下延伸的最大路径和**（用于父节点计算）

### 递归函数设计

定义 `maxPathSum(node)` 返回以该节点为起点，向下延伸的最大路径和：

- 递归边界：如果节点为空，返回 0
- 否则返回 `node.val + max(0, fun(left), fun(right))`,

- 这里可以先拆分，计算leftMax = Math.max(0, fun(node.left)), rightMax = Math.max(0, fun(node.right))
- 加上 0 是因为如果子路径为负，不选择它

对于每个节点，计算经过它的最大路径和：

- `left_max` = 左子树向下的最大路径
- `right_max` = 右子树向下的最大路径
- **经过该节点的路径和** = `node.val + left_max + right_max`
- 更新全局最大值

## Java 代码实现

```java
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode() {}
    TreeNode(int val) { this.val = val; }
    TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

class Solution {
    private int maxSum = Integer.MIN_VALUE;
    
    public int maxPathSum(TreeNode root) {
        dfs(root);
        return maxSum;
    }
    private int dfs(TreeNode node) {
        if (node == null) {
            return 0;
        }
        
        // 递归计算左右子树的最大路径和
        // 如果为负数，则不选择（取0）
        int leftMax = Math.max(0, dfs(node.left));
        int rightMax = Math.max(0, dfs(node.right));
        
        // 经过当前节点的最大路径和 = 节点值 + 左边最大 + 右边最大
        int pathThroughNode = node.val + leftMax + rightMax;
        
        // 更新全局最大值
        maxSum = Math.max(maxSum, pathThroughNode);
        
        // 返回向下延伸的最大路径和（只能选择一条分支）
        return node.val + Math.max(leftMax, rightMax);
    }
}
```

## 复杂度分析

- **时间复杂度**：O(n)，每个节点访问一次
- **空间复杂度**：O(h)，h 是树的高度（递归栈深度）