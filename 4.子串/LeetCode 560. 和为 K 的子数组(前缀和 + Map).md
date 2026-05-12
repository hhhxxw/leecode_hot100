
# 题目描述

给你一个整数数组 `nums` 和一个整数 `k`，请你统计并返回该数组中和为 `k` 的**连续子数组**的个数。

**示例 1：**

```
输入：nums = [1,1,1], k = 2
输出：2
解释：[1,1] 和 [1,1] 两个子数组的和为2
```

**示例 2：**

```
输入：nums = [1,2,3], k = 3
输出：2
解释：[1,2] 和 [3] 两个子数组的和为3
```

**约束条件：**

- `1 <= nums.length <= 2 * 10^4`
- `-1000 <= nums[i] <= 1000`
- `-10^7 <= k <= 10^7`

---

# 解题思路

## 前缀和 + 哈希表（最优解）

**核心思想：**

1. **前缀和定义**：`prefixSum[i]` 表示从数组开头到索引 `i` 的所有元素之和
2. **子数组和的关系**：

- 子数组 `[i, j]` 的和 = `prefixSum[j] - prefixSum[i-1]`
- 如果这个和等于 k，则：`prefixSum[j] - prefixSum[i-1] = k`
- 变形得：`prefixSum[i-1] = prefixSum[j] - k`

3. **关键洞察**：

- 当我们遍历到索引 `j` 时，如果之前存在某个前缀和等于 `prefixSum[j] - k`
- 那么从那个位置到当前位置 `j` 的子数组和就等于 k

4. **使用哈希表**：

- Key: 前缀和的值
- Value: 该前缀和出现的次数
- 边遍历边查找，看之前是否存在 `prefixSum - k`

---

## 代码实现

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        // 哈希表：key是前缀和，value是该前缀和出现的次数
        Map<Integer, Integer> prefixSumMap = new HashMap<>();
        
        // 初始化：前缀和为0出现1次（表示空数组）
        // 这很重要！处理从索引0开始的子数组
        prefixSumMap.put(0, 1);
        
        int count = 0;      // 结果计数
        int prefixSum = 0;  // 当前前缀和
        
        // 遍历数组
        for (int num : nums) {
            // 1. 计算当前前缀和
            prefixSum += num;
            
            // 2. 查找是否存在 prefixSum - k
            // 如果存在，说明有子数组的和等于k
            if (prefixSumMap.containsKey(prefixSum - k)) {
                count += prefixSumMap.get(prefixSum - k);
            }
            
            // 3. 将当前前缀和加入哈希表
            prefixSumMap.put(prefixSum, 
                prefixSumMap.getOrDefault(prefixSum, 0) + 1);
        }
        
        return count;
    }
}
```

```python
def subarraySum(nums, k):
    # 哈希表：key=前缀和，value=出现次数
    prefix_sum_map = {}
    # 初始化：前缀和0出现1次，处理从索引0开始的子数组
    prefix_sum_map[0] = 1
    
    count = 0
    prefix_sum = 0
    
    for num in nums:
        # 1. 更新当前前缀和
        prefix_sum += num
        
        # 2. 查找是否存在 prefix_sum - k
        if (prefix_sum - k) in prefix_sum_map:
            count += prefix_sum_map[prefix_sum - k]
        
        # 3. 更新哈希表中当前前缀和的次数
        prefix_sum_map[prefix_sum] = prefix_sum_map.get(prefix_sum, 0) + 1
    
    return count
```

# 复杂度分析

### 方法一：前缀和 + 哈希表

- **时间复杂度**：O(n)

- 遍历数组一次：O(n)
- 哈希表查找和插入：O(1)

- **空间复杂度**：O(n)

- 哈希表最多存储n个不同的前缀和