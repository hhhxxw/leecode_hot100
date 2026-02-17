# 题目概述

已知一个长度为 `n` 的数组，预先按照升序排列，经由 `1` 到 `n` 次 **旋转** 后，得到输入数组。例如，原数组 `nums = [0,1,2,4,5,6,7]` 在变化后可能得到：若旋转 4 次，则可以得到 `[4,5,6,7,0,1,2]`。  
请你找出并返回数组中的 **最小元素**。

**注意：** 数组中 **不存在重复元素**。  
**要求：** 算法的时间复杂度必须是 `O(log n)`。

---

# 解题思路

看这个时间复杂度的要求可以推断出这道题的核心也是利用 **二分查找**，但这次我们不需要找某个特定的 `target`，而是要找那个“突变点”（即最小值所在的位置）。

旋转后的数组可以看作是由 **两段有序的子数组** 拼接而成的，且 **右半段的所有元素都小于左半段的所有元素**（除非数组没有旋转，或者旋转了 n 次回到原样）。

**关键点：** 我们需要比较 `nums[mid]` 和 `nums[right]` 的关系。

1. **如果** `nums[mid] < nums[right]`：

- 说明 `mid` 到 `right`这一段是有序的（递增）。
- 最小值肯定 **不在** `mid` 的右边（不包括 `mid`）。
- 但是 `mid` **本身可能是最小值**，也可能在 `mid` 的左边。
- 所以我们将搜索区间缩小为 `[left, mid]`，即 `right = mid`。

2. **如果** `nums[mid] > nums[right]`：

- 说明 `mid` 在左半段（较大的那一段），而最小值在右半段。
- 说明最小值肯定在 `mid` 的右边（且不包括 `mid`）。
- 所以我们将搜索区间缩小为 `[mid + 1, right]`，即 `left = mid + 1`。

3. **循环终止条件**：

- 当 `left == right` 时，循环结束，此时 `nums[left]` 就是最小值。

**为什么比较** `nums[right]` **而不是** `nums[left]`**？**  
因为如果是旋转数组（如 `[4,5,6,7,0,1,2]`），`nums[left]` 可能会大于 `nums[mid]`（当 `mid` 在右半段时），也可能小于 `nums[mid]`（当 `mid` 在左半段时），但这两种情况都无法确定最小值在哪一边（特别是当数组没有旋转，是完全升序的时候 `[0,1,2,3]`，`nums[left] < nums[mid]` 依然成立，但这不能说明最小值在右边）。而比较 `nums[right]` 则能明确区分：

- 若 `nums[mid] < nums[right]`，说明右边正常升序，最小值只可能在左边或就是 `mid`。
- 若 `nums[mid] > nums[right]`，说明这里发生了“断崖”，最小值一定在右边的低谷里。

---

# Java 代码

```java
class Solution {
    public int findMin(int[] nums) {
        int left = 0;
        int right = nums.length - 1;

        // 注意这里是 left < right，而不是 left <= right
        // 因为当 left == right 时，我们就找到了最小值，不需要再进循环判断了
        while (left < right) {
            int mid = left + (right - left) / 2;

            // 比较中间值和右边界值
            if (nums[mid] < nums[right]) {
                // mid 右边是有序的，说明最小值在 [left, mid] 之间
                // 注意：mid 本身可能是最小值，所以 right = mid
                right = mid;
            } else {
                // nums[mid] > nums[right]
                // 说明 mid 在左半段较大的部分，最小值在 mid 右边
                // 注意：mid 肯定不是最小值，所以 left = mid + 1
                left = mid + 1;
            }
        }
        
        // 循环结束时，left == right，指向最小值
        return nums[left];
    }
}
```