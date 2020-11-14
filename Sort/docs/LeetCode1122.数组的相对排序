# [LeetCode1122.数组的相对排序](https://leetcode-cn.com/problems/relative-sort-array/)
## 题目描述
给你两个数组，`arr1` 和 `arr2`，

- `arr2` 中的元素各不相同
- `arr2` 中的每个元素都出现在 `arr1` 中
对 `arr1` 中的元素进行排序，使 `arr1` 中项的相对顺序和 `arr2` 中的相对顺序相同。未在 `arr2` 中出现过的元素需要按照升序放在 `arr1` 的末尾。

- `arr1.length, arr2.length <= 1000`
- `0 <= arr1[i], arr2[i] <= 1000`
- `arr2` 中的元素 `arr2[i]` 各不相同
- `arr2` 中的每个元素 `arr2[i]` 都出现在 `arr1` 中

### 示例
```
输入：arr1 = [2,3,1,3,2,4,6,7,9,2,19], arr2 = [2,1,4,3,9,6]
输出：[2,2,2,1,4,3,3,9,6,7,19]
```
## 题解
### 解法一
一种容易想到的方法是使用排序并自定义比较函数。

由于数组 `arr2`规定了比较顺序，因此我们可以使用哈希表对该顺序进行映射：即对于数组 `arr2`中的第 `i` 个元素，我们将 `(arr2[i],i)` 这一键值对放入哈希表 中，就可以很方便地对数组 `arr1`中的元素进行比较。

比较函数的写法有很多种，例如我们可以使用最基础的比较方法，对于元素 `x` 和 `y`：

- 如果 `x` 和 `y` 都出现在哈希表中，那么比较它们对应的值 `map[x]` 和 `map[y]`；
- 如果 `x` 和 `y` 都没有出现在哈希表中，那么比较它们本身；

对于剩余的情况，出现在哈希表中的那个元素较小。

```java
class Solution {
    public int[] relativeSortArray(int[] arr1, int[] arr2) {
        Map<Integer, Integer> map = new HashMap<>();
        List<Integer> list = new ArrayList<>();
        for(int num : arr1) list.add(num);
        for(int i = 0; i < arr2.length; i++) map.put(arr2[i], i);
        Collections.sort(list, (x, y) -> {
            if(map.containsKey(x) || map.containsKey(y)) return map.getOrDefault(x, 1001) - map.getOrDefault(y, 1001);
            return x - y;
        });
        for(int i = 0; i < arr1.length; i++) arr1[i] = list.get(i);
        return arr1;
    }
}
```
自己的写法
```java
class Solution {
    public int[] relativeSortArray(int[] arr1, int[] arr2) {
        int len1=arr1.length,len2=arr2.length;
        HashMap<Integer,Integer> map=new HashMap<>();
        for(int i=0;i<len2;i++){
            map.put(arr2[i],i);
        }
        List<Integer> temp1=new ArrayList<>();
        List<Integer> temp2=new ArrayList<>();
        for(int num:arr1){
            if(map.containsKey(num)){
                temp1.add(num);
            }else{
                temp2.add(num);
            }
        }
        Collections.sort(temp2);
        Collections.sort(temp1,new Comparator<Integer>(){
            public int compare(Integer a,Integer b){
                int key1=map.get(a),key2=map.get(b);
                return key1-key2;
            }
        });
        int idx=0;
        for(int num:temp1){
            arr1[idx++]=num;
        }
        for(int num:temp2){
            arr1[idx++]=num;
        }
        return arr1;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(mlogm+n)$，其中 `m` 和 `n` 分别是数组`arr1`和 `arr2`的长度。构造哈希表 `map` 的时间复杂度为 $O(n)$，排序的时间复杂度为 $O(mlogm)$。
- 空间复杂度：$O(logm+n)$，哈希表 `map` 需要的空间为 $O(n)$，排序需要的栈空间为 $O(logm)$。

### 解法二
计数排序：

注意到本题中元素的范围为 `[0,1000]`，这个范围不是很大，我们也可以考虑不基于比较的排序，例如「计数排序」。

具体地，我们使用一个长度为 1001（下标从 0 到 1000）的数组 `frequency`，记录每一个元素在数组 `arr1`中出现的次数。随后我们遍历数组 `arr2`，当遍历到元素 `x` 时，我们将 `frequency[x]` 个 `x` 加入答案中，并将 `frequency[x]` 清零。当遍历结束后，所有在 `arr2`中出现过的元素就已经有序了。

此时还剩下没有在 `arr2`中出现过的元素，因此我们还需要对整个数组`frequency`进行一次遍历。当遍历到元素 `x` 时，如果 `frequency[x]`不为 0，我们就将 `frequency[x]` 个 `x` 加入答案中。

细节

我们可以对空间复杂度进行一个小优化。实际上，我们不需要使用长度为 1001 的数组，而是可以找出数组 `arr1`中的最大值 `upper`，使用长度为 `upper+1` 的数组即可。

```java
class Solution {
    public int[] relativeSortArray(int[] arr1, int[] arr2) {
        int upper = 0;
        for (int x : arr1) {
            upper = Math.max(upper, x);
        }
        int[] frequency = new int[upper + 1];
        for (int x : arr1) {
            ++frequency[x];
        }
        int[] ans = new int[arr1.length];
        int index = 0;
        for (int x : arr2) {
            for (int i = 0; i < frequency[x]; ++i) {
                ans[index++] = x;
            }
            frequency[x] = 0;
        }
        for (int x = 0; x <= upper; ++x) {
            for (int i = 0; i < frequency[x]; ++i) {
                ans[index++] = x;
            }
        }
        return ans;
    }
}
```
### 复杂度分析
- 时间复杂度：$O(m+n+upper)$，其中 `m` 和 `n` 分别是数组 `arr1`和 `arr2`的长度，`upper` 是数组 `arr1`中的最大值，在本题中 `upper` 不会超过 1000。遍历数组 `arr2`的时间复杂度为 $O(n)$，遍历数组 `frequency` 的时间复杂度为 $O(upper)$，而在遍历的过程中，我们一共需要 $O(m)$ 的时间得到答案数组。
- 空间复杂度：$O(upper)$，即为数组 `frequency` 需要使用的空间。注意到与方法一不同的是，方法二的代码使用了额外的空间（而不是直接覆盖数组 `arr1`存放答案，但我们一般不将存储返回答案的数组计入空间复杂度，并且在我们得到数组 `frequency` 之后，实际上也是可以将返回答案覆盖在数组 `arr1`上的。如果在面试中遇到了本题，这些细节都可以和面试官进行确认。

