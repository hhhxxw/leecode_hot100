#双指针 #链表删除
## 题目概述

给定一个**已排序**的链表的头 `head` ，删除所有重复的元素，使得每个元素只出现一次。返回同样按排序顺序排列的链表。

- **输入**：`head = [1, 1, 2]` -> **输出**：`[1, 2]`
- **输入**：`head = [1, 1, 2, 3, 3]` -> **输出**：`[1, 2, 3]`

---

## 解题思路

这里用到的是双指针思想

因为链表是有序的，我们只需要比较当前节点 `cur` 和它的下一个节点 `cur.next`：

1. **比较**：如果 `cur.val == cur.next.val`，说明遇到了重复元素。
2. **删除**：将 `cur.next` 指向 `cur.next.next`（跳过重复的节点）。
3. **移动**：如果值不相等，说明当前元素不重复，直接将 `cur` 移动到下一个节点。
4. **边界条件**：如果链表为空或只有一个节点，直接返回 `head`。

### 复杂度分析

- **时间复杂度**：![](https://cdn.nlark.com/yuque/__latex/e65a67ac353abeeff44c359310d05c02.svg)，其中 是链表的长度。我们只遍历了一次链表。
- **空间复杂度**：![](https://cdn.nlark.com/yuque/__latex/a2006f1ac61cb1902beacb3e29fff089.svg)，只使用了常数级别的额外空间（指针变量）。

---

## 代码实现

### Java 版本

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if (head == null) return null;
        
        ListNode cur = head;
        while (cur != null && cur.next != null) {
            if (cur.val == cur.next.val) {
                // 发现重复，跳过下一个节点
                cur.next = cur.next.next;
            } else {
                // 不重复，继续向后移动
                cur = cur.next;
            }
        }
        return head;
    }
}
```

### Python 版本

```python
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if not head:
            return head
        
        cur = head
        while cur and cur.next:
            if cur.val == cur.next.val:
                # 发现重复，修改 next 指针
                cur.next = cur.next.next
            else:
                # 无重复，移动 cur 指针
                cur = cur.next
        return head
```