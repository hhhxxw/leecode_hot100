# 题目概述

给定一个按照升序排列的整数数组 `nums`，和一个目标值 `target`。找出给定目标值在数组中的开始位置和结束位置。  
如果数组中不存在目标值 `target`，返回 `[-1, -1]`。

**要求：** 算法的时间复杂度必须是 `O(log n)`。

---

# 解题思路

由于数组是**有序**的，且要求时间复杂度为 `O(log n)`，这明显提示我们需要使用 **二分查找 (Binary Search)**。

普通的二分查找只能找到一个等于 `target` 的位置，但这道题需要找到“第一个”和“最后一个”。我们可以通过两次二分查找来分别确定这两个边界：

1. **查找左边界 (Find First)**：

- 使用二分查找找到 `target`。
- 当 `nums[mid] == target` 时，不要停止，而是记录当前位置，并继续向 **左** 搜索 (`right = mid - 1`)，试图找到更靠前的 `target`。
- 如果 `nums[mid] < target`，说明 `target` 在右边，`left = mid + 1`。
- 如果 `nums[mid] > target`，说明 `target` 在左边，`right = mid - 1`。

2. **查找右边界 (Find Last)**：

- 同样使用二分查找。
- 当 `nums[mid] == target` 时，不要停止，而是记录当前位置，并继续向 **右** 搜索 (`left = mid + 1`)，试图找到更靠后的 `target`。

3. **综合结果**：

- 先找左边界，如果没找到（返回 -1），说明数组中不存在该元素，直接返回 `[-1, -1]`。
- 如果找到了左边界，再找右边界，最后返回 `[left_bound, right_bound]`。

---

# Java 代码

```java
public class Solution {
    public int[] searchRange(int[] nums, int target) {
        int[] result = new int[]{-1, -1};
        // 边界检查
        if (nums == null || nums.length == 0) {
            return result;
        }

        // 1. 寻找左边界
        int firstIndex = findFirst(nums, target);
        
        // 如果左边界没找到，说明数组中没有 target
        if (firstIndex == -1) {
            return result;
        }

        // 2. 寻找右边界
        int lastIndex = findLast(nums, target);

        result[0] = firstIndex;
        result[1] = lastIndex;
        return result;
    }

    // 辅助函数：寻找第一个等于 target 的索引
    private int findFirst(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        int index = -1;

        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] >= target) {
                // 如果 mid 的值大于等于 target，说明左边界在 mid 或 mid 左边
                // 核心点：即使相等，也要继续向左找
                right = mid - 1;
                if (nums[mid] == target) {
                    index = mid; 
                }
            } else {
                // nums[mid] < target
                left = mid + 1;
            }
        }
        return index;
    }

    // 辅助函数：寻找最后一个等于 target 的索引
    private int findLast(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        int index = -1;

        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] <= target) {
                // 如果 mid 的值小于等于 target，说明右边界在 mid 或 mid 右边
                // 核心点：即使相等，也要继续向右找
                left = mid + 1;
                if (nums[mid] == target) {
                    index = mid; 
                }
            } else {
                // nums[mid] > target
                right = mid - 1;
            }
        }
        return index;
    }
}
```

中间元素和目标元素，如果要找最左边，使用小于等于判断还是大于等于判断呢？

如果使用小于等于，nums[mid] <= target , 需要进一步向左探索，那么right = mid - 1，说明有边界在mid 或者mid的右边，需要继续向右找。`left = mid + 1`， 如果刚好等于target要更新index；如果用大于等于`nums[mid] >= target` 那么说明左边界在mid 或者 mid的左边，需要继续向左找。