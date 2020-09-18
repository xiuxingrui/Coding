# [LeetCode76.最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)
## 题目描述
给你一个字符串 `S`、一个字符串 `T`。请你设计一种算法，可以在 $O(n)$ 的时间复杂度内，从字符串 `S` 里面找出：包含 `T` 所有字符的最小子串。

- 如果 `S` 中不存这样的子串，则返回空字符串 `""`。
- 如果 `S` 中存在这样的子串，我们保证它是唯一的答案。

### 示例
```
输入：S = "ADOBECODEBANC", T = "ABC"
输出："BANC"
```
## 题解
滑动窗口算法的大致逻辑如下：
```c++
int left = 0, right = 0;

while (right < s.size()) {
    // 增大窗口
    window.add(s[right]);
    right++;

    while (window needs shrink) {
        // 缩小窗口
        window.remove(s[left]);
        left++;
    }
}
```
这个算法技巧的时间复杂度是 $O(N)$，比一般的字符串暴力算法要高效得多。

```c++
/* 滑动窗口算法框架 */
void slidingWindow(string s, string t) {
    unordered_map<char, int> need, window;
    for (char c : t) need[c]++;

    int left = 0, right = 0;
    int valid = 0; 
    while (right < s.size()) {
        // c 是将移入窗口的字符
        char c = s[right];
        // 右移窗口
        right++;
        // 进行窗口内数据的一系列更新
        ...

        /*** debug 输出的位置 ***/
        printf("window: [%d, %d)\n", left, right);
        /********************/

        // 判断左侧窗口是否要收缩
        while (window needs shrink) {
            // d 是将移出窗口的字符
            char d = s[left];
            // 左移窗口
            left++;
            // 进行窗口内数据的一系列更新
            ...
        }
    }
}
```

其中两处`...`表示的更新窗口数据的地方，到时候你直接往里面填就行了。

1. 我们在字符串`S`中使用双指针中的左右指针技巧，初始化`left = right = 0`，把索引左闭右开区间`[left, right)`称为一个「窗口」。
2. 我们先不断地增加`right`指针扩大窗口`[left, right)`，直到窗口中的字符串符合要求（包含了T中的所有字符）。
3. 此时，我们停止增加`right`，转而不断增加`left`指针缩小窗口`[left, right)`，直到窗口中的字符串不再符合要求（不包含T中的所有字符了）。同时，每次增加`left`，我们都要更新一轮结果。
4. 重复第 2 和第 3 步，直到`right`到达字符串`S`的尽头。

下面画图理解一下，`needs`和`window`相当于计数器，分别记录`T`中字符出现次数和「窗口」中的相应字符的出现次数。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200917141416.png)

增加`right`，直到窗口`[left, right)`包含了T中所有字符：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200917141440.png)

现在开始增加`left`，缩小窗口`[left, right)`。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200917141503.png)

直到窗口中的字符串不再符合要求，`left`不再继续移动。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200917141522.png)

之后重复上述过程，先移动`right`，再移动`left`…… 直到`right`指针到达字符串S的末端，算法结束。

首先，初始化`window`和`need`两个哈希表，记录窗口中的字符和需要凑齐的字符.

然后，使用`left`和`right`变量初始化窗口的两端，不要忘了，区间`[left, right)`是左闭右开的，所以初始情况下窗口没有包含任何元素.

其中`valid`变量表示窗口中满足`need`条件的字符个数，如果`valid`和`need.size()`的大小相同，则说明窗口已满足条件，已经完全覆盖了串`T`。

现在开始套模板，只需要思考以下四个问题：

1. 当移动`right`扩大窗口，即加入字符时，应该更新哪些数据？
2. 什么条件下，窗口应该暂停扩大，开始移动`left`缩小窗口？
3. 当移动`left`缩小窗口，即移出字符时，应该更新哪些数据？
4. 我们要的结果应该在扩大窗口时还是缩小窗口时进行更新？

如果一个字符进入窗口，应该增加`window`计数器；如果一个字符将移出窗口的时候，应该减少`window`计数器；当`valid`满足`need`时应该收缩窗口；应该在收缩窗口的时候更新最终结果。

需要注意的是，当我们发现某个字符在`window`的数量满足了`need`的需要，就要更新`valid`，表示有一个字符已经满足要求。而且，你能发现，两次对窗口内数据的更新操作是完全对称的。

当`valid == need.size()`时，说明`T`中所有字符已经被覆盖，已经得到一个可行的覆盖子串，现在应该开始收缩窗口了，以便得到「最小覆盖子串」。

移动`left`收缩窗口时，窗口内的字符都是可行解，所以应该在收缩窗口的阶段进行最小覆盖子串的更新，以便从可行解中找到长度最短的最终结果。

解释二：

本问题要求我们返回字符串 `s` 中包含字符串 `t` 的全部字符的最小窗口。我们称包含 `t` 的全部字母的窗口为「可行」窗口。

我们可以用滑动窗口的思想解决这个问题，在滑动窗口类型的问题中都会有两个指针。一个用于「延伸」现有窗口的 `r` 指针，和一个用于「收缩」窗口的 `l` 指针。在任意时刻，只有一个指针运动，而另一个保持静止。我们在 `s` 上滑动窗口，通过移动 `r` 指针不断扩张窗口。当窗口包含 `t` 全部所需的字符后，如果能收缩，我们就收缩窗口直到得到最小窗口。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200918120020.gif)

如何判断当前的窗口包含所有 `t` 所需的字符呢？我们可以用一个哈希表表示 `t` 中所有的字符以及它们的个数，用一个哈希表动态维护窗口中所有的字符以及它们的个数，如果这个动态表中包含 `t` 的哈希表中的所有字符，并且对应的个数都不小于 `t` 的哈希表中各个字符的个数，那么当前的窗口是「可行」的。

注意：这里 `t` 中可能出现重复的字符，所以我们要记录字符的个数。

1. 不断增加`j`使滑动窗口增大，直到窗口包含了`T`的所有元素
2. 不断增加`i`使滑动窗口缩小，因为是要求最小字串，所以将不必要的元素排除在外，使长度减小，直到碰到一个必须包含的元素，这个时候不能再扔了，再扔就不满足条件了，记录此时滑动窗口的长度，并保存最小值
3. 让`i`再增加一个位置，这个时候滑动窗口肯定不满足条件了，那么继续从步骤一开始执行，寻找新的满足条件的滑动窗口，如此反复，直到`j`超出了字符串`S`范围。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200918120451.png)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200918120805.png)

```java
class Solution {
    public String minWindow(String s, String t) {
        HashMap<Character,Integer> window=new HashMap<>();
        HashMap<Character,Integer> need=new HashMap<>();
        char[] chs=s.toCharArray();
        char[] cht=t.toCharArray();
        for(char c:cht){
            need.put(c,need.getOrDefault(c,0)+1);
        }
        int left=0,right=0,start=0;
        int len=Integer.MAX_VALUE,valid=0;
        while(right<s.length()){
            char r=chs[right];
            right++;
            if(need.containsKey(r)){
                window.put(r,window.getOrDefault(r,0)+1);
                //用==最后一个用例过不去，两个很长的字符串，Integer是对象，Integer会缓存频繁使用的数值，数值范围为-128到127，在此范围内直接返回缓存值。超过该范围就会new 一个对象,用等号会出错。
                if(window.get(r).equals(need.get(r))){
                    valid++;
                }
            }
            while(valid==need.size()){
                if(right-left<len){
                    len=right-left;
                    start=left;
                }
                char l=chs[left];
                left++;
                if(need.containsKey(l)){
                    //用==最后一个用例过不去，两个很长的字符串，Integer是对象，Integer会缓存频繁使用的数值，数值范围为-128到127，在此范围内直接返回缓存值。超过该范围就会new 一个对象,用等号会出错。
                    if(window.get(l).equals(need.get(l))){
                        valid--;
                    } 
                    window.put(l,window.get(l)-1);
                }
            }
        }
        if(len==Integer.MAX_VALUE){
            return "";
        }else{
            return s.substring(start,start+len);
        }
    }
}
```
### 复杂度分析
- 时间复杂度：最坏情况下左右指针对 `s` 的每个元素各遍历一遍，哈希表中对 `s` 中的每个元素各插入、删除一次，对 `t` 中的元素各插入一次。每次检查是否可行会遍历整个 `t` 的哈希表，哈希表的大小与字符集的大小有关，设字符集大小为 `C`，则渐进时间复杂度为 $O(C⋅∣s∣+∣t∣)$。
- 空间复杂度：这里用了两张哈希表作为辅助空间，每张哈希表最多不会存放超过字符集大小的键值对，我们设字符集大小为 `C` ，则渐进空间复杂度为 $O(C)$。
