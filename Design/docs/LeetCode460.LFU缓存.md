# [LeetCode460.LFU缓存](https://leetcode-cn.com/problems/lfu-cache/)
## 题目描述
请你为 最不经常使用（`LFU`）缓存算法设计并实现数据结构。

实现 `LFUCache` 类：

- `LFUCache(int capacity)` - 用数据结构的容量 `capacity` 初始化对象
- `int get(int key)` - 如果键存在于缓存中，则获取键的值，否则返回 -1。
- `void put(int key, int value)` - 如果键已存在，则变更其值；如果键不存在，请插入键值对。当缓存达到其容量时，则应该在插入新项之前，使最不经常使用的项无效。在此问题中，当存在平局（即两个或更多个键具有相同使用频率）时，应该去除 最久未使用 的键。

注意「项的使用次数」就是自插入该项以来对其调用 `get` 和 `put` 函数的次数之和。使用次数会在对应项被移除后置为 0 。

是否可以在 $O(1)$ 时间复杂度内执行两项操作？
### 示例
```
输入：
["LFUCache", "put", "put", "get", "put", "get", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [3], [4, 4], [1], [3], [4]]
输出：
[null, null, null, 1, null, -1, 3, null, -1, 3, 4]

解释：
LFUCache lFUCache = new LFUCache(2);
lFUCache.put(1, 1);
lFUCache.put(2, 2);
lFUCache.get(1);      // 返回 1
lFUCache.put(3, 3);   // 去除键 2
lFUCache.get(2);      // 返回 -1（未找到）
lFUCache.get(3);      // 返回 3
lFUCache.put(4, 4);   // 去除键 1
lFUCache.get(1);      // 返回 -1（未找到）
lFUCache.get(3);      // 返回 3
lFUCache.get(4);      // 返回 4
```
## 题解
我们定义两个哈希表，第一个 `freqMap` 以频率 `freq` 为索引，每个索引存放一个双向链表，这个链表里存放所有使用频率为 `freq` 的缓存，缓存里存放三个信息，分别为键 `key`，值 `value`，以及使用频率 `freq`。第二个 `keyMap` 以键值 `key` 为索引，每个索引存放对应缓存在 `freqMap` 中链表里的内存地址，这样我们就能利用两个哈希表来使得两个操作的时间复杂度均为 $O(1)$。同时需要记录一个当前缓存最少使用的频率 `minFreq`，这是为了删除操作服务的。

对于 `get(key)` 操作，我们能通过索引 `key` 在 `keyMap` 中找到缓存在 `freqMap` 中的链表的内存地址，如果不存在直接返回 -1，否则我们能获取到对应缓存的相关信息，这样我们就能知道缓存的键值还有使用频率，直接返回 `key` 对应的值即可。

但是我们注意到 `get` 操作后这个缓存的使用频率加一了，所以我们需要更新缓存在哈希表 `freqMap` 中的位置。已知这个缓存的键 `key`，值 `value`，以及使用频率 `freq`，那么该缓存应该存放到 `freqMap` 中 `freq + 1` 索引下的链表中。所以我们在当前链表中 $O(1)$ 删除该缓存对应的节点，根据情况更新 `minFreq` 值，然后将其$O(1)$ 插入到 `freq + 1` 索引下的链表头完成更新。这其中的操作复杂度均为 $O(1)$。你可能会疑惑更新的时候为什么是插入到链表头，这其实是为了保证缓存在当前链表中从链表头到链表尾的插入时间是有序的，为下面的删除操作服务。

对于 `put(key, value)`操作，我们先通过索引 `key`在 `keyMap` 中查看是否有对应的缓存，如果有的话，其实操作等价于 `get(key)` 操作，唯一的区别就是我们需要将当前的缓存里的值更新为 `value`。如果没有的话，相当于是新加入的缓存，如果缓存已经到达容量，需要先删除最近最少使用的缓存，再进行插入。

先考虑插入，由于是新插入的，所以缓存的使用频率一定是 1，所以我们将缓存的信息插入到 `freqMap` 中 1 索引下的列表头即可，同时更新 `keyMap[key]` 的信息，以及更新 `minFreq = 1`。

那么剩下的就是删除操作了，由于我们实时维护了 `minFreq`，所以我们能够知道 `freqMap` 里目前最少使用频率的索引，同时因为我们保证了链表中从链表头到链表尾的插入时间是有序的，所以 `freqMap[minFreq]` 的链表中链表尾的节点即为使用频率最小且插入时间最早的节点，我们删除它同时根据情况更新 `minFreq` ，整个时间复杂度均为 $O(1)$。

```java
class LFUCache {
    class Node{
        int key,val,freq;
        public Node(int key,int val,int freq){
            this.key=key;
            this.val=val;
            this.freq=freq;
        }
    }
    private int minFreq,size,capacity;
    private HashMap<Integer,Node> keyMap;
    private HashMap<Integer,LinkedList<Node>> freqMap;
    public LFUCache(int capacity) {
        this.size=0;
        this.capacity=capacity;
        keyMap=new HashMap<>();
        freqMap=new HashMap<>();
    }
    
    public int get(int key) {
        if(!keyMap.containsKey(key)){
            return -1;
        }else{
            Node node=keyMap.get(key);
            int val=node.val,freq=node.freq;
            freqMap.get(freq).remove(node);
            if(freqMap.get(freq).size()==0){
                freqMap.remove(freq);
                if(freq==minFreq){
                    minFreq+=1;
                }
            }
            LinkedList<Node> list=freqMap.getOrDefault(freq+1,new LinkedList<Node>());
            Node newNode=new Node(key,val,freq+1);
            list.addFirst(newNode);
            freqMap.put(freq+1,list);
            keyMap.put(key,newNode);
            return val;
        }
    }
    
    public void put(int key, int value) {
        if(capacity==0){
            return;
        }
        if(!keyMap.containsKey(key)){
            if(size==capacity){
                Node node=freqMap.get(minFreq).getLast();
                keyMap.remove(node.key);
                freqMap.get(minFreq).removeLast();
                if(freqMap.get(minFreq).size()==0){
                    freqMap.remove(minFreq);
                }
                size--;
            }
            size++;
            minFreq=1;
            LinkedList<Node> list=freqMap.getOrDefault(1,new LinkedList<Node>());
            Node newNode=new Node(key,value,1);
            list.addFirst(newNode);
            freqMap.put(1,list);
            keyMap.put(key,newNode);
        }else{
            // 与 get 操作基本一致，除了需要更新缓存的值
            Node node = keyMap.get(key);
            int freq = node.freq;
            freqMap.get(freq).remove(node);
            if (freqMap.get(freq).size() == 0) {
                freqMap.remove(freq);
                if (minFreq == freq) {
                    minFreq += 1;
                }
            }
            LinkedList<Node> list = freqMap.getOrDefault(freq + 1, new LinkedList<Node>());
            Node newNode=new Node(key,value,freq+1);
            list.addFirst(newNode);
            freqMap.put(freq+1,list);
            keyMap.put(key,newNode);
        }
    }
}

/**
 * Your LFUCache object will be instantiated and called as such:
 * LFUCache obj = new LFUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```
### 复杂度分析
- 时间复杂度：`get` 时间复杂度 $O(1)$，`put` 时间复杂度 $O(1)$。由于两个操作从头至尾都只利用了哈希表的插入删除还有链表的插入删除，且它们的时间复杂度均为 $O(1)$，所以保证了两个操作的时间复杂度均为 $O(1)$。
- 空间复杂度：$O(capacity)$，其中 `capacity` 为 `LFU` 的缓存容量。哈希表中不会存放超过缓存容量的键值对。


