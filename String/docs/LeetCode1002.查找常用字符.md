# [LeetCode1002.查找常用字符](https://leetcode-cn.com/problems/find-common-characters/)
## 题解
给定仅有小写字母组成的字符串数组`A`，返回列表中的每个字符串中都显示的全部字符（包括重复字符）组成的列表。例如，如果一个字符在每个字符串中出现 3 次，但不是 4 次，则需要在最终答案中包含该字符 3 次。

你可以按任意顺序返回答案。

- `1 <= A.length <= 100`
- `1 <= A[i].length <= 100`
- `A[i][j]` 是小写字母
### 示例
```
输入：["bella","label","roller"]
输出：["e","l","l"]
```
```
输入：["cool","lock","cook"]
输出：["c","o"]
```
## 题解
根据题目的要求，如果字符 `c` 在所有字符串中均出现了 `k` 次及以上，那么最终答案中需要包含 `k` 个 `c`。因此，我们可以使用 `minfreq[c]`存储字符 `c` 在所有字符串中出现次数的最小值。

我们可以依次遍历每一个字符串。当我们遍历到字符串 `s` 时，我们使用 `freq[c]` 统计 `s` 中每一个字符 `c` 出现的次数。在统计完成之后，我们再将每一个 `minfreq[c]` 更新为其本身与 `freq[c]` 的较小值。这样一来，当我们遍历完所有字符串后，`minfreq[c]` 就存储了字符 `c` 在所有字符串中出现次数的最小值。

由于题目保证了所有的字符均为小写字母，因此我们可以用长度为 26 的数组分别表示 `minfreq`以及 `freq`。

在构造最终的答案时，我们遍历所有的小写字母 `c`，并将 `minfreq[c]` 个 `c` 添加进答案数组即可。


```java
class Solution {
    public List<String> commonChars(String[] A) {
        List<String> ans=new ArrayList<>();
        int[] cnt=new int[26];
        Arrays.fill(cnt,Integer.MAX_VALUE);
        int len=A.length;
        for(int i=0;i<len;i++){
            char[] str=A[i].toCharArray();
            int[] count=new int[26];
            for(int j=0;j<str.length;j++){
                count[str[j]-'a']++;
            }
            for(int k=0;k<26;k++){
                if(count[k]<cnt[k]){
                    cnt[k]=count[k];
                }
            }
        }
        for(int i=0;i<26;i++){
            if(cnt[i]!=0){
                for(int j=0;j<cnt[i];j++){
                    ans.add(String.valueOf((char)('a'+i)));
                }
            }
        }
        return ans;
    }
}
```
### 复杂度分析
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201014005130.png)

