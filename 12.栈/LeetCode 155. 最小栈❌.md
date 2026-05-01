# 题目概述

设计一个支持 `push`，`pop`，`top` 操作，并能在 **常数时间** `O(1)` 内检索到最小元素的栈。

**功能要求：**

- `push(x)` —— 将元素 x 推入栈中。
- `pop()` —— 删除栈顶的元素。
- `top()` —— 获取栈顶元素。
- `getMin()` —— 检索栈中的最小元素。

---

# 解题思路

普通的栈只能获取栈顶元素，如果要获取最小值通常需要 `O(N)` 的时间去遍历。要在 `O(1)` 时间内获取最小值，需要 **用空间换时间**。

**核心思想：**  
维护两个栈：

1. **数据栈 (dataStack)**：正常的栈，用于存储所有压入的数据。
2. **辅助栈 (minStack)**：专门用于存储当前栈内的最小值。

**详细逻辑：**

- **push(x)**：

- 将 `x` 压入 `dataStack`。
- 如果 `minStack` 为空，或者 `x` 小于等于 `minStack` 的栈顶元素，则将 `x` 也压入 `minStack`。这样 `minStack` 的栈顶始终是当前的最小值。

- **pop()**：

- 弹出 `dataStack` 的栈顶元素 `val`。
- 如果 `val` 等于 `minStack` 的栈顶元素，说明我们要移除的正好是当前的最小值，所以 `minStack` 也要弹出栈顶。

- **top()**：

- 直接返回 `dataStack` 的栈顶。

- **getMin()**：

- 直接返回 `minStack` 的栈顶。

**举例演示：**  
`push(-2), push(0), push(-3)`

- Stack: `[-2, 0, -3]` (栈顶)
- MinStack: `[-2, -3]` (栈顶) -> 此时 getMin() 返回 -3

`pop()` (移除 -3)

- Stack: `[-2, 0]`
- MinStack: `[-2]` -> 此时 getMin() 返回 -2

---

# Java 代码

```
import java.util.Stack;

class MinStack {
    private Stack<Integer> dataStack;
    private Stack<Integer> minStack;

    public MinStack() {
        dataStack = new Stack<>();
        minStack = new Stack<>();
    }
    
    public void push(int val) {
        dataStack.push(val);
        // 如果辅助栈为空，或者新元素 <= 当前最小值，则入辅助栈
        // 注意：这里必须是 <=，因为如果最小值重复出现，我们也要记录，否则 pop 时会出错
        if (minStack.isEmpty() || val <= minStack.peek()) {
            minStack.push(val);
        }
    }
    
    public void pop() {
        if (dataStack.isEmpty()) return;
        
        int val = dataStack.pop();
        // 如果弹出的元素等于当前最小值，辅助栈也要弹出
        // 使用 equals 比较 Integer 对象的值
        if (val == minStack.peek()) {
            minStack.pop();
        }
    }
    
    public int top() {
        return dataStack.peek();
    }
    
    public int getMin() {
        return minStack.peek();
    }
}
```