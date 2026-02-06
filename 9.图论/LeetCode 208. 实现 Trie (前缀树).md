# 问题描述

需要实现一个 `Trie` 类，包含以下核心方法：

- `Trie()`: 初始化前缀树对象。
- `void insert(String word)`: 将单词 `word` 插入前缀树中。
- `boolean search(String word)`: 判断单词 `word` 是否在前缀树中（必须是完整的单词）。
- `boolean startsWith(String prefix)`: 判断是否存在以 `prefix` 为前缀的单词。

# 解题思路

Trie 的核心在于**利用字符串的公共前缀来减少查询时间**。

#### 节点设计

每一个节点不需要存储自身的字符，而是通过“子节点数组”的下标来表示字符。例如 `children[0]` 存在，则代表当前路径下有一个字符 'a'。

- `TrieNode[] children: 长度为 26 的数组，存储指向子节点的指针。
- `boolean isEnd`: 标记当前节点是否为一个单词的结尾。

#### 操作流程

1. **插入 (**`insert`**)**：

- 从根节点开始遍历单词的每个字符。
- 计算索引 `index = char - 'a'`。
- 如果子节点数组对应位置为空，则新建节点。
- 移动到子节点，继续下一个字符。
- 完成后将最后一个节点的 `isEnd` 设为 `true`。

1. **查找 (**`search`**) / 前缀查找 (**`startsWith`**)**：

- 两者的区别仅在于：`search` 要求最终停下的节点其 `isEnd` 必须为 `true`；而 `startsWith` 只要路径存在即可。

时空复杂度

- **插入操作的时间复杂度**：O(m)，其中 m 是单词的长度
- **搜索操作的时间复杂度**：O(m)，其中 m 是单词的长度
- **前缀搜索的时间复杂度**：O(k)，其中 k 是前缀的长度
- **空间复杂度**：O(n * m)，其中 n 是单词的数量，m 是单词的最大长度

# Java代码实现

```java
class TrieNode {
    // 26个小写字母
    TrieNode[] children = new TrieNode[26];
    // 标记该节点是否为单词结尾
    boolean isEnd = false;
}

class Trie {
    private TrieNode root;
    
    public Trie() {
        root = new TrieNode();
    }
    
    // 插入单词
    public void insert(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            int index = c - 'a';
            // 如果子节点不存在，创建新节点
            if (node.children[index] == null) {
                node.children[index] = new TrieNode();
            }
            node = node.children[index];
        }
        // 标记单词结尾
        node.isEnd = true;
    }
    
    // 搜索完整单词
    public boolean search(String word) {
        TrieNode node = searchNode(word);
        // 必须是单词结尾
        return node != null && node.isEnd;
    }
    
    // 搜索前缀
    public boolean startsWith(String prefix) {
        // 只需要找到节点即可，不需要是单词结尾
        return searchNode(prefix) != null;
    }
    
    // 辅助方法：搜索节点
    private TrieNode searchNode(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            int index = c - 'a';
            if (node.children[index] == null) {
                return null;
            }
            node = node.children[index];
        }
        return node;
    }
}
```