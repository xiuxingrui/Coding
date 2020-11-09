# [LeetCode347.前K个高频元素](https://leetcode-cn.com/problems/top-k-frequent-elements/)
## 题目描述
给定一个非空的整数数组，返回其中出现频率前 `k` 高的元素。

- 你可以假设给定的 `k` 总是合理的，且 `1 ≤ k ≤ 数组中不相同的元素的个数`。
- 你的算法的时间复杂度必须优于 $O(nlogn)$, `n` 是数组的大小。
- 题目数据保证答案唯一，换句话说，数组中前 `k` 个高频元素的集合是唯一的。
- 你可以按任意顺序返回答案。

### 示例
```
输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]
```
```
输入: nums = [1], k = 1
输出: [1]
```
## 题解
### 解法一
堆/优先级队列

首先遍历整个数组，并使用哈希表记录每个数字出现的次数，并形成一个「出现次数数组」。找出原数组的前 `k` 个高频元素，就相当于找出「出现次数数组」的前 `k` 大的值。

最简单的做法是给「出现次数数组」排序。但由于可能有 $O(N)$ 个不同的出现次数（其中 `N` 为原数组长度），故总的算法复杂度会达到 $O(NlogN)$，不满足题目的要求。

在这里，我们可以利用堆的思想：建立一个小顶堆，然后遍历「出现次数数组」：

- 如果堆的元素个数小于 `k`，就可以直接插入堆中。
- 如果堆的元素个数等于 `k`，则检查堆顶与当前出现次数的大小。如果堆顶更大，说明至少有 `k` 个数字的出现次数比当前值大，故舍弃当前值；否则，就弹出堆顶，并将当前值插入堆中。

遍历完成后，堆中的元素就代表了「出现次数数组」中前 `k` 大的值。

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> occurrences = new HashMap<Integer, Integer>();
        for (int num : nums) {
            occurrences.put(num, occurrences.getOrDefault(num, 0) + 1);
        }

        // int[] 的第一个元素代表数组的值，第二个元素代表了该值出现的次数
        PriorityQueue<int[]> queue = new PriorityQueue<int[]>(new Comparator<int[]>() {
            public int compare(int[] m, int[] n) {
                return m[1] - n[1];
            }
        });
        for (Map.Entry<Integer, Integer> entry : occurrences.entrySet()) {
            int num = entry.getKey(), count = entry.getValue();
            if (queue.size() == k) {
                if (queue.peek()[1] < count) {
                    queue.poll();
                    queue.offer(new int[]{num, count});
                }
            } else {
                queue.offer(new int[]{num, count});
            }
        }
        int[] ret = new int[k];
        for (int i = 0; i < k; ++i) {
            ret[i] = queue.poll()[0];
        }
        return ret;
    }
}
```

自己写法(大顶堆):
```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        HashMap<Integer,Integer> map=new HashMap<>();
        for(int num:nums){
            map.put(num,map.getOrDefault(num,0)+1);
        }
        PriorityQueue<int[]> pqueue=new PriorityQueue<>(new Comparator<int[]>(){
            public int compare(int[] a,int[] b){
                if(a[1]!=b[1]){
                    return b[1]-a[1];
                }else{
                    return a[0]-b[0];
                }
            }
        });
        for(int key:map.keySet()){
            int value=map.get(key);
            pqueue.offer(new int[]{key,value});
        }
        int[] ans=new int[k];
        int count=0;
        while(k>0){
            ans[count++]=pqueue.poll()[0];
            k--;
        }
        return ans;
    }
}
```
#### 复杂度分析

- 时间复杂度：$O(Nlogk)$，其中 `N` 为数组的长度。我们首先遍历原数组，并使用哈希表记录出现次数，每个元素需要 $O(1)$ 的时间，共需 $O(N)$ 的时间。随后，我们遍历「出现次数数组」，由于堆的大小至多为 `k`，因此每次堆操作需要 $O(logk)$ 的时间，共需 $O(Nlogk)$ 的时间。二者之和为 $O(Nlogk)$。
- 空间复杂度：$O(N)$。哈希表的大小为 $O(N)$，而堆的大小为 $O(k)$(大顶堆大小为`N`)，共计为$O(N)$。

### 解法二
桶排序

首先依旧使用哈希表统计频率，统计完成后，创建一个数组，将频率作为数组下标，对于出现频率不同的数字集合，存入对应的数组下标即可。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200907191702.png)

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        // 统计每个数字出现的次数
        Map<Integer, Integer> counterMap = IntStream.of(nums).boxed().collect(Collectors.toMap(e -> e, e -> 1, Integer::sum));
        // 一个数字最多出现 nums.length 次，因此定义一个长度为 nums.length + 1 的数组，freqList[i] 中存储出现次数为 i 的所有数字。
        List<Integer>[] freqList = new List[nums.length + 1];
        for (int i = 0; i < freqList.length; i++) {
            freqList[i] = new ArrayList<>();
        }
        counterMap.forEach((num, freq) -> {
            freqList[freq].add(num);
        });
        // 按照出现频次，从大到小遍历频次数组，构造返回结果。
        int[] res = new int[k];
        int idx = 0;
        for (int freq = freqList.length - 1; freq > 0; freq--) {
            for (int num: freqList[freq]) {
                res[idx++] = num;
                if (idx == k) {
                    return res;
                }
            }
        }
        return res;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n)$，`n` 表示数组的长度。首先，遍历一遍数组统计元素的频率，这一系列操作的时间复杂度是 $O(n)$；桶的数量为 `n+1`，所以桶排序的时间复杂度为 $O(n)$；因此，总的时间复杂度是 $O(n)$。
- 空间复杂度：很明显为 $O(n)$
