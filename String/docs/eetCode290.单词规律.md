# [LeetCode290.单词规律](https://leetcode-cn.com/problems/word-pattern/)
## 题目描述
给定一种规律 `pattern` 和一个字符串 `str` ，判断 `str` 是否遵循相同的规律。

这里的 遵循 指完全匹配，例如， `pattern` 里的每个字母和字符串 `str` 中的每个非空单词之间存在着双向连接的对应规律。

你可以假设 `pattern` 只包含小写字母， `str` 包含了由单个空格分隔的小写字母。    
### 示例
```
输入: pattern = "abba", str = "dog cat cat dog"
输出: true
```
```
输入:pattern = "abba", str = "dog cat cat fish"
输出: false
```
```
输入: pattern = "aaaa", str = "dog cat cat dog"
输出: false
```
```
输入: pattern = "abba", str = "dog dog dog dog"
输出: false
```
## 题解
自己的写法：
```java
class Solution {
    public boolean wordPattern(String pattern, String s) {
        char[] arr=pattern.toCharArray();
        String[] ss=s.split(" ");
        int len=pattern.length();
        if(len!=ss.length){
            return false;
        }
        HashMap<Character,String> map1=new HashMap<>();
        HashMap<String,Character> map2=new HashMap<>();
        for(int i=0;i<len;i++){
            if(map1.containsKey(arr[i])){
                if(!map1.get(arr[i]).equals(ss[i])){
                    return false;
                }
            }else{
                map1.put(arr[i],ss[i]);
            }
            if(map2.containsKey(ss[i])){
                if(map2.get(ss[i])!=arr[i]){
                    return false;
                }
            }else{
                map2.put(ss[i],arr[i]);
            }
        }
        return true;
    }
}
```
不用库函数：
```java
class Solution {
    public boolean wordPattern(String pattern, String s) {
        char[] arr1=pattern.toCharArray();
        char[] arr2=s.toCharArray();
        int len1=pattern.length();
        int len2=s.length();
        HashMap<Character,String> map1=new HashMap<>();
        HashMap<String,Character> map2=new HashMap<>();
        int j=0;
        for(int i=0;i<len1;i++){
            StringBuilder sb=new StringBuilder();
            while(j<len2&&arr2[j]!=' '){
                sb.append(arr2[j]);
                j++;
            }
            j++;
            if(j>=len2&&i<len1-1){
                return false;
            }
            String temp=sb.toString();
            if(map1.containsKey(arr1[i])){
                if(!map1.get(arr1[i]).equals(temp)){
                    return false;
                }
            }else{
                map1.put(arr1[i],temp);
            }
            if(map2.containsKey(temp)){
                if(map2.get(temp)!=arr1[i]){
                    return false;
                }
            }else{
                map2.put(temp,arr1[i]);
            }
        }
        if(j<len2-1){
            return false;
        }
        return true;
    }
}
```
别人的写法
```java
class Solution {
    public boolean wordPattern(String pattern, String s) {
        String[] ss = s.split(" "); //以空格分隔str
        if(ss.length != pattern.length()) return false; //如果没有全部成对的映射则返回false
        Map<Character, String> map = new HashMap<>(); //存放映射
        char[] arr=pattern.toCharArray();
        for(int i = 0; i < pattern.length(); i++){
            if(!map.containsKey(arr[i])){ //1. 没有映射时执行
                if(map.containsValue(ss[i])) return false; //2. 没有映射的情况下s[i]已被使用，则不匹配返回false
                map.put(arr[i], ss[i]); //3. 构建映射
            }else{
                if(!map.get(arr[i]).equals(ss[i])) return false; //当前字符串与映射不匹配,返回false
            }
        }
        return true;
    }
}
```