## 问题描述

给定一个二叉树，将其展开为一个单链表。展开后的单链表应该同样用二叉树节点表示，其中 `right` 子指针指向链表中的下一个节点，而 `left` 子指针始终为 `null`。

**示例：**

```
输入：
    1
   / \
  2   5
 / \   \
3   4   6

输出：
1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6
```

flatten 的递归解法属于后序遍历思想，因为它必须先展开左右子树，再拼接到当前节点下，符合后序“左右根”的处理顺序，是后序思想的典型应用场景。

## 解题思路

考虑到这道题目的解题场景， 就是要先处理好左右子树之后，在进行拼接。所以借助后序遍历的思想去进行递归。具体来说，对于每一个结点，我先去展开其左右子树，将右子树连接到左子树的最右结点，然后将左子树移动到右边，左子树置空

- 对于每个节点，先递归展开左右子树
- 将右子树连接到左子树的最右节点
- 将左子树移到右边，左子树置为 null

## 时空复杂度

**时间复杂度：** O(n)**空间复杂度：** O(h)，其中 h 是树的高度（递归栈深度）

## Java 代码实现

```Java
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode() {}
    TreeNode(int val) { this.val = val; }
    TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

public class Solution {
    public void flatten(TreeNode root) {
        if (root == null) return;
        
        // 递归展开左子树
        flatten(root.left);
        // 递归展开右子树
        flatten(root.right);
        
        // 保存左右子树
        TreeNode left = root.left;
        TreeNode right = root.right;
        
        // 将左子树移到右边
        root.right = left;
        root.left = null;
        
        // 找到新右子树的最右节点
        TreeNode current = root;
        while (current.right != null) {
            current = current.right;
        }
        
        // 连接原右子树
        current.right = right;
    }
}
```