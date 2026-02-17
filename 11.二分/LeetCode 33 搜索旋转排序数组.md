# 题目概述

整数数组 `nums` 按升序排列，数组中的值 **互不相同**。  
在传递给函数之前，`nums` 在预先未知的某个下标 `k`（`0 <= k < nums.length`）上进行了 **旋转**，使数组变为 `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]`。  
例如， `[0,1,2,4,5,6,7]` 在下标 3 处经旋转后可能变为 `[4,5,6,7,0,1,2]`。  
给你 **旋转后** 的数组 `nums` 和一个整数 `target`，如果 `nums` 中存在这个目标值 `target`，则返回它的下标，否则返回 `-1`。

**要求：** 算法时间复杂度必须是 `O(log n)`。

---

# 解题思路

虽然数组被旋转了，但它仍然是 **局部有序** 的。对于任意一个分割点 `mid`，数组总会被分为两部分，其中 **至少有一部分是有序的**。我们可以利用这个特性进行二分查找。

**算法步骤：**

1. 初始化 `left = 0`, `right = n - 1`。
2. 进入循环 `while (left <= right)`：

- 计算 `mid = left + (right - left) / 2`。
- 如果 `nums[mid] == target`，直接返回 `mid`。
- **判断哪半边是有序的**：

- 如果 `nums[left] <= nums[mid]`，说明 **[left, mid] 是有序的**（左半边有序）。

- 判断 `target` 是否在左半边范围内：如果 `nums[left] <= target < nums[mid]`，则说明目标在左边，移动 `right = mid - 1`。
- 否则，目标只能在右半边（无序的那部分），移动 `left = mid + 1`。

- 否则（`nums[left] > nums[mid]`），说明 **[mid, right] 是有序的**（右半边有序）。

- 判断 `target` 是否在右半边范围内：如果 `nums[mid] < target <= nums[right]`，则说明目标在右边，移动 `left = mid + 1`。
- 否则，目标只能在左半边（无序的那部分），移动 `right = mid - 1`。

3. 循环结束若没找到，返回 `-1`。

---

# Java 代码

```java
class Solution {
    public int search(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return -1;
        }

        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {
            int mid = left + (right - left) / 2;

            if (nums[mid] == target) {
                return mid;
            }

            // 判断哪一边是有序的
            // 注意：这里必须是 <=，因为当 left == mid 时（只剩两个元素），我们也认为左边是有序的
            if (nums[left] <= nums[mid]) {
                // 左半边有序 [left, mid]
                // 检查 target 是否在左半边的范围内
                if (nums[left] <= target && target < nums[mid]) {
                    right = mid - 1; // 目标在左半边
                } else {
                    left = mid + 1;  // 目标在右半边
                }
            } else {
                // 右半边有序 [mid, right]
                // 检查 target 是否在右半边的范围内
                if (nums[mid] < target && target <= nums[right]) {
                    left = mid + 1;  // 目标在右半边
                } else {
                    right = mid - 1; // 目标在左半边
                }
            }
        }

        return -1;
    }
}
```