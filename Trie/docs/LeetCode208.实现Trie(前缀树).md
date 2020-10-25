# [LeetCode208.实现Trie(前缀树)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/)
## 题目描述
实现一个 `Trie` (前缀树)，包含 `insert`, `search`, 和 `startsWith` 这三个操作。

- 你可以假设所有的输入都是由小写字母 `a-z` 构成的。
- 保证所有输入均为非空字符串。
### 示例
```
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // 返回 true
trie.search("app");     // 返回 false
trie.startsWith("app"); // 返回 true
trie.insert("app");   
trie.search("app");     // 返回 true
```
## 题解
`Trie` 树是一个有根的树，其结点具有以下字段：。

- 最多 `R` 个指向子结点的链接，其中每个链接对应字母表数据集中的一个字母。
- 本文中假定 `R` 为 26，小写拉丁字母的数量。
- 布尔字段，以指定节点是对应键的结尾还是只是键前缀。

想象以下，包含三个单词 `"sea"`,`"sells"`,`"she"` 的 `Trie` 会长啥样呢？

它的真实情况是这样的：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201024154628.png)



插入:

我们通过搜索 `Trie` 树来插入一个键。我们从根开始搜索它对应于第一个键字符的链接。有两种情况：

- 链接存在。沿着链接移动到树的下一个子层。算法继续搜索下一个键字符。
- 链接不存在。创建一个新的节点，并将它与父节点的链接相连，该链接与当前的键字符相匹配。

重复以上步骤，直到到达键的最后一个字符，然后将当前节点标记为结束节点，算法完成。

- 时间复杂度：$O(m)$，其中 `m` 为键长。在算法的每次迭代中，我们要么检查要么创建一个节点，直到到达键尾。只需要 `m` 次操作。
- 空间复杂度：$O(m)$。最坏的情况下，新插入的键和 `Trie` 树中已有的键没有公共前缀。此时需要添加 `m` 个结点，使用 $O(m)$ 空间。

查找键：

每个键在 `trie` 中表示为从根到内部节点或叶的路径。我们用第一个键字符从根开始，。检查当前节点中与键字符对应的链接。有两种情况：

- 存在链接。我们移动到该链接后面路径中的下一个节点，并继续搜索下一个键字符。
- 不存在链接。若已无键字符，且当前结点标记为 `isEnd`，则返回 `true`。否则有两种可能，均返回 `false` :
  - 还有键字符剩余，但无法跟随 `Trie` 树的键路径，找不到键。
  - 没有键字符剩余，但当前结点没有标记为 `isEnd`。也就是说，待查找键只是`Trie`树中另一个键的前缀。

- 时间复杂度 : $O(m)$。算法的每一步均搜索下一个键字符。最坏的情况下需要 `m` 次操作。
- 空间复杂度 : $O(1)$。

查找 `Trie` 树中的键前缀

该方法与在 `Trie` 树中搜索键时使用的方法非常相似。我们从根遍历 `Trie` 树，直到键前缀中没有字符，或者无法用当前的键字符继续 `Trie` 中的路径。与上面提到的“搜索键”算法唯一的区别是，到达键前缀的末尾时，总是返回 `true`。我们不需要考虑当前 `Trie` 节点是否用 `“isend”` 标记，因为我们搜索的是键的前缀，而不是整个键。

- 时间复杂度 : $O(m)$。
- 空间复杂度 : $O(1)$。

```java
class Trie {
    private boolean isEnd;
    Trie[] next;
    /** Initialize your data structure here. */
    public Trie() {
        isEnd=false;
        next=new Trie[26];
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        Trie root=this;
        for(char c:word.toCharArray()){
            if(root.next[c-'a']==null){
                root.next[c-'a']=new Trie();
            }
            root=root.next[c-'a'];
        }
        root.isEnd=true;
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        Trie root=this;
        for(char c:word.toCharArray()){
            if(root.next[c-'a']==null){
                return false;
            }
            root=root.next[c-'a'];
        }
        return root.isEnd;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        Trie root=this;
        for(char c:prefix.toCharArray()){
            if(root.next[c-'a']==null){
                return false;
            }
            root=root.next[c-'a'];
        }
        return true;
    }
}

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */
```

