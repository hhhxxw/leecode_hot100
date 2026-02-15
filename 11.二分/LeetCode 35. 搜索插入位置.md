## 题目理解

给定一个排序数组和一个目标值，找出目标值在数组中的索引。如果目标值存在，返回其索引；如果不存在，返回它应该被插入的位置。

**示例：**

- 输入: `nums = [1,3,5,6], target = 5` → 输出: `2`
- 输入: `nums = [1,3,5,6], target = 2` → 输出: `1`
- 输入: `nums = [1,3,5,6], target = 7` → 输出: `4`

## 解题思路

这是一个经典的**二分查找**问题。由于数组已排序，可以用二分查找在 O(log n) 时间内解决。

**关键点：**

- 使用左右指针逼近目标
- 当 `left > right` 时，`left` 的位置就是插入位置
- 如果找到目标值，直接返回其索引

时空复杂度

- **时间复杂度：** O(log n) - 二分查找
- **空间复杂度：** O(1) - 只使用常数额外空间

## Java 代码实现

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            
            if (nums[mid] == target) {
                return mid;  // 找到目标值
            } else if (nums[mid] < target) {
                left = mid + 1;  // 目标在右侧
            } else {
                right = mid - 1;  // 目标在左侧
            }
        }
        
        // 循环结束时，left 就是插入位置
        return left;
    }
}
```