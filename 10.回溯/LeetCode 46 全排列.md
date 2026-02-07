# 题目概述

**题目描述：** 给定一个**不含重复数字**的数组 `nums` ，返回其所有可能的**全排列**。你可以按任意顺序返回答案。

**示例：**

- **输入：**`nums = [1, 2, 3]`
- **输出：**`[[1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]]`

**核心点：**

1. 数字是不重复的（这简化了去重逻辑）。
2. 需要穷举所有可能的情况（典型的搜索问题）。

# 解题思路

### 回溯法 (Backtracking)

你可以把全排列问题想象成一棵 **N叉树** 的遍历过程。

**具体步骤：**

我们可以把问题转化为：“在这个位置上，我应该放哪个数字？”

1. **路径 (Path)：** 也就是如果你已经做出的选择。比如你已经选了 `[1, 3]`。
2. **选择列表 (Selection List)：** 你当前还可以选哪些数字？如果 `nums=[1, 2, 3]`，你选了 `[1, 3]`，那么选择列表里只剩下 `[2]`。
3. **结束条件 (Base Case)：** 当路径的长度等于 `nums` 的长度时，说明所有的数字都选完了，这就是一个合法的排列，把它加入结果集。

### 回溯的“三部曲”：

对于每一个节点（每一次递归调用），我们做三件事：

1. **做选择：** 从剩余的数字中选一个，加入路径。
2. **递归：** 进入下一层决策树。
3. **撤销选择：** 递归回来后，把刚才选的数字拿出来（回溯），以便去选别的数字。

### 如何记录“已使用的数字”？

为了避免重复使用同一个位置的数字（比如 `[1, 1, 1]` 是不合法的），我们需要一个标记。

- **方法 A：** 每次检查路径里是否包含该数字 (`path.contains(val)`)。但这需要 ![](https://cdn.nlark.com/yuque/__latex/8c51f5913186f8ac629f1d5838940f33.svg) 的时间。
- **方法 B (推荐)：** 使用一个 `boolean[] used` 数组，记录哪个下标的数字已经被用过了。查表只需要 ![](https://cdn.nlark.com/yuque/__latex/a2006f1ac61cb1902beacb3e29fff089.svg)。

时空复杂度

- **时间复杂度：**![](https://cdn.nlark.com/yuque/__latex/bb9ce5155456bf7115b96fbe7027d0b0.svg)

- 全排列的总数是 ![](https://cdn.nlark.com/yuque/__latex/92e72dd186a1024985f67940629cf965.svg)（阶乘）。
- 对于每一个排列，我们需要 $O(N)$ 的时间将其复制到结果列表中。
- 因此总时间是 ![](https://cdn.nlark.com/yuque/__latex/681ac3a699bf81d6caafd5434250b7a7.svg)。

- **空间复杂度：**![](https://cdn.nlark.com/yuque/__latex/8c51f5913186f8ac629f1d5838940f33.svg)

- **递归栈深度：** 最深为 ![](https://cdn.nlark.com/yuque/__latex/459f3c80a50b7be28751b0869ef5386a.svg)。
- **临时存储：**`path` 列表和 `used` 数组都需要 ![](https://cdn.nlark.com/yuque/__latex/8c51f5913186f8ac629f1d5838940f33.svg) 的空间。

# Java代码

```java
class Solution {
    List<List<Integer>> res;
    public List<List<Integer>> permute(int[] nums) {
        res = new ArrayList<>();
        if (nums == null) {
            return res;
        }
        boolean[] isVisited = new boolean[nums.length];
        dfs(nums, 0, new ArrayList<>(), isVisited);
        return res;
    }
    public void dfs(int[] nums, int current, List<Integer> list, boolean[] isVisited) {
        if (current == nums.length) {
            res.add(new ArrayList<>(list));
            return;
        }
        for (int i = 0; i < nums.length; i ++) {
            if (isVisited[i] == false) {
                isVisited[i] = true;
                list.add(nums[i]);
                dfs(nums, current + 1, list, isVisited);
                list.remove(list.size() - 1);
                isVisited[i] = false;
            }
        }
    }
}
```

result.add(new ArrayList<>(list)); 这个地方是值得关注的点， 不要写成了result.add(list);