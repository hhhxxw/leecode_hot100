## 题目理解

- **题目描述**  
    给定一个 _不含重复元素_ 的整数数组 `nums`，返回它所有可能的子集（幂集）。  
    要求：

- 子集不能重复
- 可以按任意顺序返回
- 子集包括空集 `[]` 和整个数组本身

- **示例**  
    `nums = [1,2,3]`  
    所有子集为：

```
[
  [], 
  [1], [2], [3],
  [1,2], [1,3], [2,3],
  [1,2,3]
]
```

---

## 解题思路

**回溯**：

- **状态定义**  
    用一个列表 `path` 表示当前已经选择的元素（当前子集）。
- **搜索过程**  
    从某个起始下标 `start` 开始，依次选择后面的元素是否加入当前子集：

- 每进入一次递归，当前的 `path` 就是一个合法子集，直接加入答案。
- 然后从 `start` 到数组末尾遍历：

- 选择 `nums[i]` 加入 `path`
- 递归处理下一个位置 `i + 1`
- 回溯时把刚加入的元素删除，恢复现场

- **为什么不会重复？**  
    因为我们只从当前位置之后（`i = start .. n-1`）往后选，不会回头选之前的元素，从而避免重复组合。

**时空复杂度**

- 时间复杂度：一共 ![](https://cdn.nlark.com/yuque/__latex/055ce37910d06a8239ef5a1ee87765f5.svg) 个子集，每个子集平均长度约为 ![](https://cdn.nlark.com/yuque/__latex/e65a67ac353abeeff44c359310d05c02.svg)，总体时间复杂度：![](https://cdn.nlark.com/yuque/__latex/ec2db7af439714b227c4440503238ca9.svg)
- 空间复杂度：递归栈和临时路径 `path` 最多长度为 ![](https://cdn.nlark.com/yuque/__latex/df378375e7693bdcf9535661c023c02e.svg)， ![](https://cdn.nlark.com/yuque/__latex/e65a67ac353abeeff44c359310d05c02.svg)

---

## Java 代码（回溯解法）

```java
class Solution {
    List<List<Integer>> res;
    public List<List<Integer>> subsets(int[] nums) {
        res = new ArrayList<>();
        if (nums == null) {
            return res;
        }
        dfs(new ArrayList<>(),nums,  0);   
        return res;
    }
    public void dfs(List<Integer> list,int[] nums, int current) {
        if (current == nums.length) {
            res.add(new ArrayList<>(list));
            return;
        }
        list.add(nums[current]);
        dfs(list, nums, current + 1);
        list.remove(list.size() - 1);
        dfs(list, nums, current + 1);

    }
}
```