# [LeetCode49.字母异位词分组](https://leetcode-cn.com/problems/group-anagrams/)
## 题目描述
给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。

- 所有输入均为小写字母。
- 不考虑答案输出的顺序。
### 示例
```
输入: ["eat", "tea", "tan", "ate", "nat", "bat"]
输出:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```
## 题解
当且仅当它们的排序字符串相等时，两个字符串是字母异位词。

维护一个映射 `ans : {String -> List}`，其中每个键 `K` 是一个排序字符串，每个值是初始输入的字符串列表，排序后等于 `K`。

在 `Java` 中，我们将键存储为字符串，例如，`code`。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201005203816.png)

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        List<List<String>> ans=new ArrayList<>();
        HashMap<String,List<String>> map=new HashMap<>();
        for(String str:strs){
            char[] arr=str.toCharArray();
            Arrays.sort(arr);
            String temp=String.valueOf(arr);//不能用toString()
            if(map.containsKey(temp)){
                map.get(temp).add(str);
            }else{
                List<String> list=new ArrayList<>();
                list.add(str);
                map.put(temp,list);
            }
        }
        for(String str:map.keySet()){
            List<String> list=map.get(str);
            ans.add(list);
        }
        return ans;
    }
}
```
### 复杂度分析
- 时间复杂度：$O(NKlogK)$，其中 `N` 是 `strs` 的长度，而 `K` 是 `strs`中字符串的最大长度。当我们遍历每个字符串时，外部循环具有的复杂度为 $O(N)$。然后，我们在 $O(KlogK)$ 的时间内对每个字符串排序。
- 空间复杂度：$O(NK)$，排序存储在 `ans` 中的全部信息内容。


