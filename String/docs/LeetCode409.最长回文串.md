# [LeetCode409.最长回文串](https://leetcode-cn.com/problems/longest-palindrome/)
## 题解
给定一个包含大写字母和小写字母的字符串，找到通过这些字母构造成的最长的回文串。

在构造过程中，请注意区分大小写。比如 `"Aa"` 不能当做一个回文字符串。

注意:
假设字符串的长度不会超过 1010。

### 示例
```
输入:
"abccccdd"

输出:
7

解释:
我们可以构造的最长的回文串是"dccaccd", 它的长度是 7。
```
## 题解
1. 如果某字母有偶数个，因为偶数有对称性，可以把它全部用来构造回文串；但如果是奇数个的话，并不是完全不可以用来构建，也不是只能选最长的那个，而是只要砍掉1个，剩下的变成偶数就可以全部计入了
2. 但奇数字母里，可以保留1个不砍，把它作为回文串的中心，所以最后还要再加回一个1
3. 但是！如果压根没有奇数的情况，这个1也不能随便加，所以还要分情况讨论
```java
class Solution {
    public int longestPalindrome(String s) {
      int[] cnt = new int[58];
      for (char c : s.toCharArray()) {
        cnt[c - 'A'] += 1;
      }

      int ans = 0;
      for (int x: cnt) {
        // 字符出现的次数最多用偶数次。
        ans += x - (x & 1);
      }
      // 如果最终的长度小于原字符串的长度，说明里面某个字符出现了奇数次，那么那个字符可以放在回文串的中间，所以额外再加一。
      return ans < s.length() ? ans + 1 : ans;  
    }
}
```
自己写法：
```java
class Solution {
    public int longestPalindrome(String s) {
        HashMap<Character,Integer> map=new HashMap<>();
        char[] charArray=s.toCharArray();
        for(char c:charArray){
            map.put(c,map.getOrDefault(c,0)+1);
        }
        int ans=0;
        boolean flag=false;
        for(Character key:map.keySet()){
            int value=map.get(key);
            if(value%2==0){
                ans+=value;
            }else{
                flag=true;
                ans+=value-1;
            }
        }
        if(flag==true){
            ans++;
        }
        return ans;
    }
}
```
