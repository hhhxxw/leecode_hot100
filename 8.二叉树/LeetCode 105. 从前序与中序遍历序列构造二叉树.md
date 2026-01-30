## 问题描述

输入：给定两个整数数组 `preorder` 和 `inorder`

- `preorder` 是二叉树的前序遍历数组（根，左，右）
- `inorder` 是二叉树的中序遍历数组（左，根，右）

输出：请构造并返回这棵二叉树，返回根结点

**示例：可以动手画一画**

```
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]

返回：
    3
   / \
  9  20
    /  \
   15   7
```

## 解题思路

根据前序序列找到根结点，然后在中序序列中找到根结点的位置，根据位置，划分左右子树。这里使用递归算法继续处理左右子树。为了快速根据根结点的值找到其所在中序序列中的位置，可以使用HashMap<元素值，中序序列元素对应的下标>

1. **前序遍历**：根 → 左子树 → 右子树

- 第一个元素总是根节点

2. **中序遍历**：左子树 → 根 → 右子树

- 根节点在中序中的位置左边是左子树，右边是右子树

3. **递归构造**：

- 从前序中取出根节点
- 在中序中找到根节点位置
- 根据位置划分左右子树
- 递归构造左右子树

**时间复杂度：** O(n)  
**空间复杂度：** O(n)（哈希表 + 递归栈）

## Java 代码实现

```java
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
    
    /**
     * 方法一：递归 + 哈希表优化
     * 使用哈希表存储中序遍历中每个元素的索引，避免重复查找
     */
    private Map<Integer, Integer> inorderMap;
    private int preorderIndex;
    
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        // 构建中序遍历的哈希表，key: 值, value: 索引
        inorderMap = new HashMap<>();
        for (int i = 0; i < inorder.length; i++) {
            inorderMap.put(inorder[i], i);
        }
        preorderIndex = 0;
        return buildTreeHelper(preorder, 0, inorder.length - 1);
    }
    private TreeNode buildTreeHelper(int[] preorder, int inStart, int inEnd) {
        // 递归终止条件
        if (inStart > inEnd) {
            return null;
        }
        
        // 前序遍历的第一个元素是根节点
        int rootVal = preorder[preorderIndex++];
        TreeNode root = new TreeNode(rootVal);
        
        // 在中序遍历中找到根节点的位置
        int inorderRootIndex = inorderMap.get(rootVal);
        
        // 递归构造左子树
        // 左子树的中序范围：[inStart, inorderRootIndex - 1]
        root.left = buildTreeHelper(preorder, inStart, inorderRootIndex - 1);
        
        // 递归构造右子树
        // 右子树的中序范围：[inorderRootIndex + 1, inEnd]
        root.right = buildTreeHelper(preorder, inorderRootIndex + 1, inEnd);
        
        return root;
    }
}
```