# [LeetCode205.同构字符串](https://leetcode-cn.com/problems/isomorphic-strings/)
## 题目描述
给定两个字符串 `s` 和 `t`，判断它们是否是同构的。

如果 `s` 中的字符可以被替换得到 `t` ，那么这两个字符串是同构的。

所有出现的字符都必须用另一个字符替换，同时保留字符的顺序。两个字符不能映射到同一个字符上，但字符可以映射自己本身。

你可以假设 `s` 和 `t` 具有相同的长度。
### 示例
```
输入: s = "egg", t = "add"
输出: true
```
```
输入: s = "foo", t = "bar"
输出: false
```
```
输入: s = "paper", t = "title"
输出: true
```
## 题解
两个字符串同构的含义就是字符串 `s` 可以唯一的映射到 `t` ，同时 `t` 也可以唯一的映射到 `s` 。

我们只需要验证 `s - > t` 和 `t -> s` 两个方向即可。如果验证一个方向，是不可以的。

举个例子，`s = ab`, `t = cc`，如果单看 `s -> t` ，那么 `a -> c`, `b -> c` 是没有问题的。

必须再验证 `t -> s`，此时，`c -> a`,`c -> b`，一个字母对应了多个字母，所以不是同构的。

```java
class Solution {
    public boolean isIsomorphic(String s, String t) {
        if(s.length()!=t.length()){
            return false;
        }
        int len=s.length();
        HashMap<Character,Character> map1=new HashMap<>();
        HashMap<Character,Character> map2=new HashMap<>();
        char[] arr1=s.toCharArray();
        char[] arr2=t.toCharArray();
        for(int i=0;i<len;i++){
            if(map1.containsKey(arr1[i])){
                if(map1.get(arr1[i])!=arr2[i]){
                    return false;
                }
            }else{
                map1.put(arr1[i],arr2[i]);
            }
            if(map2.containsKey(arr2[i])){
                if(map2.get(arr2[i])!=arr1[i]){
                    return false;
                }
            }else{
                map2.put(arr2[i],arr1[i]);
            }
        }
        return true;
    }
}
```
另一种写法：
```java
class Solution {
    public boolean isIsomorphic(String s, String t) {
        if(s.length()!=t.length()){
            return false;
        }
        int len=s.length();
        HashMap<Character,Character> map=new HashMap<>();
        char[] arr1=s.toCharArray();
        char[] arr2=t.toCharArray();
        for(int i=0;i<len;i++){
            if(map.containsKey(arr1[i])){
                if(map.get(arr1[i])!=arr2[i]){
                    return false;
                }
            }else{
                if(map.containsValue(arr2[i])){
                    return false;
                }
                map.put(arr1[i],arr2[i]);
            }
        }
        return true;
    }
}
```