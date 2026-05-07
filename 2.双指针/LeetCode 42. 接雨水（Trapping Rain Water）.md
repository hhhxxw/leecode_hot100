## 题目描述

给定 `n` 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

**示例：**

```
输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，
     在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）
```

```
        █
    █ ░░█░█
  █ ░█░█████
0 1 0 2 1 0 1 3 2 1 2 1
```

## 核心思路

**关键观察**：对于每个位置 `i`，能接住的雨水量取决于：

- 它**左边最高**的柱子高度 `leftMax`
- 它**右边最高**的柱子高度 `rightMax`
- 当前位置能接的水 = `min(leftMax, rightMax) - height[i]`（如果为正）

想象一个木桶原理：水位由较短的那块木板决定。

## 解法一：动态规划（推荐理解）

### 思路

1. 预处理两个数组：

- `leftMax[i]`：位置 i 左边（包括 i）的最大高度
- `rightMax[i]`：位置 i 右边（包括 i）的最大高度

2. 遍历每个位置，计算能接的雨水

### 代码实现

```
class Solution {
    public int trap(int[] height) {
        if (height == null || height.length < 3) {
            return 0;
        }
        
        int n = height.length;
        
        // leftMax[i] 表示 i 位置左边（包括i）的最大高度
        int[] leftMax = new int[n];
        leftMax[0] = height[0];
        for (int i = 1; i < n; i++) {
            leftMax[i] = Math.max(leftMax[i - 1], height[i]);
        }
        
        // rightMax[i] 表示 i 位置右边（包括i）的最大高度
        int[] rightMax = new int[n];
        rightMax[n - 1] = height[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            rightMax[i] = Math.max(rightMax[i + 1], height[i]);
        }
        
        // 计算每个位置能接的雨水
        int water = 0;
        for (int i = 0; i < n; i++) {
            // 当前位置能接的水 = 左右最大值的较小值 - 当前高度
            water += Math.min(leftMax[i], rightMax[i]) - height[i];
        }
        
        return water;
    }
}
```

**时间复杂度**：O(n) - 三次遍历  
**空间复杂度**：O(n) - 两个辅助数组

---

## 解法二：双指针（最优解）

### 思路

优化动态规划的空间，不需要额外数组。使用两个指针从两端向中间移动：

- 维护 `leftMax` 和 `rightMax` 两个变量
- 哪边低就处理哪边（因为水位由低的一边决定）

### 代码实现

```java
class Solution {
    public int trap(int[] height) {
        if (height == null || height.length < 3) {
            return 0;
        }
        
        int left = 0, right = height.length - 1;
        int leftMax = 0, rightMax = 0;
        int water = 0;
        
        while (left < right) {
            if (height[left] < height[right]) {
                // 左边低，处理左边
                if (height[left] >= leftMax) {
                    // 更新左边最大值
                    leftMax = height[left];
                } else {
                    // 能接雨水
                    water += leftMax - height[left];
                }
                left++;
            } else {
                // 右边低，处理右边
                if (height[right] >= rightMax) {
                    // 更新右边最大值
                    rightMax = height[right];
                } else {
                    // 能接雨水
                    water += rightMax - height[right];
                }
                right--;
            }
        }
        
        return water;
    }
}
```

```python
class Solution:
    def trap(self, height: List[int]) -> int:
        if not height or len(height) < 3:
            return 0
        left = 0
        right = len(height) - 1
        leftMax = 0
        rightMax = 0
        water = 0

        while left < right:
            if height[left] < height[right]:
                if height[left] >= leftMax:
                    leftMax = height[left]
                else:
                    water += leftMax - height[left]
                left += 1
            else:
                if height[right] >= rightMax:
                    rightMax = height[right]
                else:
                    water += rightMax - height[right]
                right -= 1
        return water

```

**时间复杂度**：O(n) - 一次遍历  
**空间复杂度**：O(1) - 只用常量空间

---

## 解法三：单调栈

### 思路

按行计算而不是按列计算，使用单调递减栈。

### Java 代码

```
class Solution {
    public int trap(int[] height) {
        if (height == null || height.length < 3) {
            return 0;
        }
        
        Stack<Integer> stack = new Stack<>();
        int water = 0;
        
        for (int i = 0; i < height.length; i++) {
            // 当前高度大于栈顶，说明形成凹槽，可以接水
            while (!stack.isEmpty() && height[i] > height[stack.peek()]) {
                int top = stack.pop();
                
                if (stack.isEmpty()) {
                    break;
                }
                
                int left = stack.peek();
                int width = i - left - 1;
                int h = Math.min(height[i], height[left]) - height[top];
                water += width * h;
            }
            stack.push(i);
        }
        
        return water;
    }
}
```

**时间复杂度**：O(n)  
**空间复杂度**：O(n) - 栈空间

---

## 图解示例

以 `height = [0,1,0,2,1,0,1,3,2,1,2,1]` 为例：

```
索引:  0  1  2  3  4  5  6  7  8  9  10 11
高度:  0  1  0  2  1  0  1  3  2  1  2  1
左最大: 0  1  1  2  2  2  2  3  3  3  3  3
右最大: 3  3  3  3  3  3  3  3  2  2  2  1

索引2: min(1,3) - 0 = 1
索引4: min(2,3) - 1 = 1
索引5: min(2,3) - 0 = 2
索引9: min(3,2) - 1 = 1
索引10: min(3,2) - 2 = 0

总计: 1 + 1 + 2 + 1 + 1 = 6
```

## 推荐方案

- **学习理解**：先掌握**动态规划**解法，思路最清晰
- **面试最优**：使用**双指针**解法，空间复杂度 O(1)
- **进阶**：理解**单调栈**解法，适用于类似问题

希望这个讲解对你有帮助！如果有任何疑问，欢迎继续提问。