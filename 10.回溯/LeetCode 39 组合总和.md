## 题目理解

**题目描述**：给定一个无重复元素的正整数数组 `candidates` 和一个目标整数 `target`，找出 `candidates` 中所有可以使数字和为目标数的唯一组合。

**关键特点**：

- 同一个数字可以被**无限次使用**
- 返回的组合中不能有重复
- 组合内部顺序无关（[2,3] 和 [3,2] 视为同一个）

**示例**：

- 输入: candidates = [2,3,6,7], target = 7 → 输出: [[2,2,3], [7]]
- 输入: candidates = [2,3,5], target = 8 → 输出: [[2,3,3], [2,6], [3,5]]
- 输入: candidates = [2], target = 1 → 输出: []

## 解题思路

这是一个**回溯 + 剪枝**问题。核心思想：

1. 递归尝试每个候选数字
2. 同一个数字可以重复选择（不移动指针）
3. 当和等于目标时，加入结果
4. 当和超过目标时，剪枝（停止递归）
5. 为避免重复，始终从当前或后续元素开始选择

## Java 代码实现

### 回溯法

```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<>();
        
        if (candidates == null || candidates.length == 0) {
            return result;
        }
        
        // 排序便于剪枝
        Arrays.sort(candidates);
        
        backtrack(candidates, target, 0, 0, new ArrayList<>(), result);
        return result;
    }
    
    private void backtrack(int[] candidates, int target, int start, 
                          int current, List<Integer> combination, 
                          List<List<Integer>> result) {
        // 递归终止条件：和等于目标
        if (current == target) {
            result.add(new ArrayList<>(combination));
            return;
        }
        
        // 剪枝：和已经超过目标
        if (current > target) {
            return;
        }
        
        // 遍历候选数字
        for (int i = start; i < candidates.length; i++) {
            int num = candidates[i];
            
            // 剪枝：当前数字已经超过剩余目标
            if (current + num > target) {
                break;
            }
            
            // 选择
            combination.add(num);
            
            // 递归：从当前位置开始（允许重复使用）
            backtrack(candidates, target, i, current + num, combination, result);
            
            // 回溯
            combination.remove(combination.size() - 1);
        }
    }
}
```

