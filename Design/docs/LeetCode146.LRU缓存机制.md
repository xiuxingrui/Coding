# [LeetCode146.LRU缓存机制](https://leetcode-cn.com/problems/lru-cache/)
## 题目描述
运用你所掌握的数据结构，设计和实现一个  `LRU` (最近最少使用) 缓存机制。它应该支持以下操作： 获取数据 `get` 和 写入数据 `put` 。

- 获取数据 `get(key)` - 如果关键字 `(key)` 存在于缓存中，则获取关键字的值（总是正数），否则返回 -1。
- 写入数据 `put(key, value)` - 如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字/值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。

进阶:

你是否可以在 $O(1)$ 时间复杂度内完成这两种操作？

### 示例
```
LRUCache cache = new LRUCache( 2 /* 缓存容量 */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // 返回  1
cache.put(3, 3);    // 该操作会使得关键字 2 作废
cache.get(2);       // 返回 -1 (未找到)
cache.put(4, 4);    // 该操作会使得关键字 1 作废
cache.get(1);       // 返回 -1 (未找到)
cache.get(3);       // 返回  3
cache.get(4);       // 返回  4
```
## 题解
### 解法一
`LRU` 缓存机制可以通过哈希表辅以双向链表实现，我们用一个哈希表和一个双向链表维护所有在缓存中的键值对。

- 双向链表按照被使用的顺序存储了这些键值对，靠近头部的键值对是最近使用的，而靠近尾部的键值对是最久未使用的。
- 哈希表即为普通的哈希映射（`HashMap`），通过缓存数据的键映射到其在双向链表中的位置。

这样以来，我们首先使用哈希表进行定位，找出缓存项在双向链表中的位置，随后将其移动到双向链表的头部，即可在 $O(1)$ 的时间内完成 `get` 或者 `put` 操作。具体的方法如下：

- 对于 `get` 操作，首先判断 `key` 是否存在：
  - 如果 `key` 不存在，则返回 −1；
  - 如果 `key` 存在，则 `key` 对应的节点是最近被使用的节点。通过哈希表定位到该节点在双向链表中的位置，并将其移动到双向链表的头部，最后返回该节点的值。

- 对于 `put` 操作，首先判断 `key` 是否存在：
  - 如果 `key` 不存在，使用 `key` 和 `value` 创建一个新的节点，在双向链表的头部添加该节点，并将 `key` 和该节点添加进哈希表中。然后判断双向链表的节点数是否超出容量，如果超出容量，则删除双向链表的尾部节点，并删除哈希表中对应的项；
  - 如果 `key` 存在，则与 `get` 操作类似，先通过哈希表定位，再将对应的节点的值更新为 `value`，并将该节点移到双向链表的头部。

上述各项操作中，访问哈希表的时间复杂度为 $O(1)$，在双向链表的头部添加节点、在双向链表的尾部删除节点的复杂度也为 $O(1)$。而将一个节点移到双向链表的头部，可以分成「删除该节点」和「在双向链表的头部添加节点」两步操作，都可以在 $O(1)$ 时间内完成。

在双向链表的实现中，使用一个伪头部（`dummyHead`）和伪尾部（`dummyTail`）标记界限，这样在添加节点和删除节点的时候就不需要检查相邻的节点是否存在。

```java
class LRUCache {
    class DLinkedNode{
        int key;
        int val;
        DLinkedNode prev;
        DLinkedNode next;
        DLinkedNode(){};
        DLinkedNode(int key,int val){
            this.key=key;
            this.val=val;
        }
    }
    private HashMap<Integer,DLinkedNode> map=new HashMap<>();
    private int capacity;
    private int size;
    private DLinkedNode dummyHead,dummyTail;
    
    public LRUCache(int capacity) {
        this.size=0;
        this.capacity=capacity;
        // 使用伪头部和伪尾部节点
        dummyHead=new DLinkedNode();
        dummyTail=new DLinkedNode();
        dummyHead.next=dummyTail;
        dummyTail.prev=dummyHead;
    }
    
    public int get(int key) {
        if(!map.containsKey(key)){
            return -1;
        }
        // 如果 key 存在，先通过哈希表定位，再移到头部
        DLinkedNode node=map.get(key);
        moveToHead(node);
        return node.val;
    }
    
    public void put(int key, int value) {
        if(map.containsKey(key)){
            // 如果 key 存在，先通过哈希表定位，再修改 value，并移到头部
            DLinkedNode node=map.get(key);
            node.val=value;
            moveToHead(node);
        }else{
            // 如果 key 不存在，创建一个新的节点
            DLinkedNode newNode=new DLinkedNode(key,value);
            // 添加进哈希表
            map.put(key,newNode);
            // 添加至双向链表的头部
            addToHead(newNode);
            size++;
            if(size>capacity){
                // 如果超出容量，删除双向链表的尾部节点
                DLinkedNode tail=removeTail();
                // 删除哈希表中对应的项
                map.remove(tail.key);
                size--;
            }
        }
    }
    private void moveToHead(DLinkedNode node){
        removeNode(node);
        addToHead(node);
    }
    private void removeNode(DLinkedNode node){
        node.prev.next=node.next;
        node.next.prev=node.prev;
    }
    private void addToHead(DLinkedNode node){
        node.next=dummyHead.next;
        node.prev=dummyHead;
        dummyHead.next.prev=node;
        dummyHead.next=node;
    }
    private DLinkedNode removeTail() {
        DLinkedNode res = dummyTail.prev;
        removeNode(res);
        return res;
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```
#### 复杂度分析
- 时间复杂度：对于 `put` 和 `get` 都是 $O(1)$。
- 空间复杂度：$O(capacity)$，因为哈希表和双向链表最多存储 `capacity+1` 个元素。
### 解法二
我们要继承 `LinkedHashMap`，然后复写 `removeEldestEntry()`函数，就能拥有我们自己的缓存策略！

`HashMap` 底层是 数组 + 红黑树 + 链表 （不清楚也没有关系），同时其是无序的，而 `LinkedHashMap` 刚好就比 `HashMap` 多这一个功能，就是其提供 有序，并且，`LinkedHashMap`的有序可以按两种顺序排列，一种是按照插入的顺序，一种是按照读取的顺序（这个题目的示例就是告诉我们要按照读取的顺序进行排序），而其内部是靠 建立一个双向链表 来维护这个顺序的，在每次插入、删除后，都会调用一个函数来进行 双向链表的维护 ，准确的来说，是有三个函数来做这件事，这三个函数都统称为 回调函数 ，这三个函数分别是：

- `void afterNodeAccess(Node<K,V> p) { }`:其作用就是在访问元素之后，将该元素放到双向链表的尾巴处(所以这个函数只有在按照读取的顺序的时候才会执行)，之所以提这个，是建议大家去看看，如何优美的实现在双向链表中将指定元素放入链尾！
- `void afterNodeRemoval(Node<K,V> p) { }`:其作用就是在删除元素之后，将元素从双向链表中删除，还是非常建议大家去看看这个函数的，很优美的方式在双向链表中删除节点！
- `void afterNodeInsertion(boolean evict) { }`:这个才是我们题目中会用到的，在插入新元素之后，需要回调函数判断是否需要移除一直不用的某些元素！

`LinkedHashMap` 的构造函数其主要是两个构造方法，一个是继承 `HashMap`，一个是可以选择 `accessOrder` 的值(默认 `false`，代表按照插入顺序排序)来确定是按插入顺序还是读取顺序排序

```java

/**
 * //调用父类HashMap的构造方法。
 * Constructs an empty insertion-ordered <tt>LinkedHashMap</tt> instance
 * with the default initial capacity (16) and load factor (0.75).
 */
public LinkedHashMap() {
    super();
    accessOrder = false;
}
// 这里的 accessOrder 默认是为false，如果要按读取顺序排序需要将其设为 true
// initialCapacity 代表 map 的 容量，loadFactor 代表加载因子 (默认即可)
public LinkedHashMap(int initialCapacity, float loadFactor, boolean accessOrder) {
    super(initialCapacity, loadFactor);
    this.accessOrder = accessOrder;
}
```
```java
// 在插入一个新元素之后，如果是按插入顺序排序，即调用newNode()中的linkNodeLast()完成
// 如果是按照读取顺序排序，即调用afterNodeAccess()完成
// 那么这个方法是干嘛的呢，这个就是著名的 LRU 算法啦
// 在插入完成之后，需要回调函数判断是否需要移除某些元素！
// LinkedHashMap 函数部分源码

/**
 * 插入新节点才会触发该方法，因为只有插入新节点才需要内存
 * 根据 HashMap 的 putVal 方法, evict 一直是 true
 * removeEldestEntry 方法表示移除规则, 在 LinkedHashMap 里一直返回 false
 * 所以在 LinkedHashMap 里这个方法相当于什么都不做
 */
void afterNodeInsertion(boolean evict) { // possibly remove eldest
    LinkedHashMap.Entry<K,V> first;
    // 根据条件判断是否移除最近最少被访问的节点
    if (evict && (first = head) != null && removeEldestEntry(first)) {
        K key = first.key;
        removeNode(hash(key), key, null, false, true);
    }
}

// 移除最近最少被访问条件之一，通过覆盖此方法可实现不同策略的缓存
// LinkedHashMap是默认返回false的，我们可以继承LinkedHashMap然后复写该方法即可
// 例如 LeetCode 第 146 题就是采用该种方法，直接 return size() > capacity;
protected boolean removeEldestEntry(Map.Entry<K,V> eldest) {
    return false;
}
```

题解：
```java
class LRUCache extends LinkedHashMap<Integer, Integer>{
    private int capacity;
    
    public LRUCache(int capacity) {
        super(capacity, 0.75F, true);
        this.capacity = capacity;
    }

    public int get(int key) {
        return super.getOrDefault(key, -1);
    }

    public void put(int key, int value) {
        super.put(key, value);
    }

    @Override
    protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
        return size() > capacity; 
    }
}
```