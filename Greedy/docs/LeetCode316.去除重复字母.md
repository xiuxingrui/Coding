# [LeetCode316.去除重复字母](https://leetcode-cn.com/problems/remove-duplicate-letters/)
## 题目描述
给你一个字符串 `s` ，请你去除字符串中重复的字母，使得每个字母只出现一次。需保证 返回结果的字典序最小（要求不能打乱其他字符的相对位置）。

- `1 <= s.length <= 104`
- `s` 由小写英文字母组成

### 示例
```
输入：s = "bcabc"
输出："abc"
```
```
输入：s = "cbacdcbc"
输出："acdb"
```
## 题解
用栈来存储最终返回的字符串，并维持字符串的最小字典序。每遇到一个字符，如果这个字符不存在于栈中，就需要将该字符压入栈中。但在压入之前，需要先将之后还会出现，并且字典序比当前字符小的栈顶字符移除，然后再将当前字符压入。

详情看动画：

[官方题解](https://leetcode-cn.com/problems/remove-duplicate-letters/solution/qu-chu-zhong-fu-zi-mu-by-leetcode/)
```java
class Solution {
    public String removeDuplicateLetters(String s) {
        int len=s.length();
        char[] arr=s.toCharArray();
        Deque<Character> deque=new LinkedList<>();
        HashMap<Character,Integer> map=new HashMap<>();
        HashSet<Character> set=new HashSet<>();
        for(int i=0;i<len;i++){
            map.put(arr[i],i);
        }
        for(int i=0;i<len;i++){
            if(!set.contains(arr[i])){
                while(!deque.isEmpty()&&arr[i]<deque.peekLast()&&map.get(deque.peekLast())>i){
                    set.remove(deque.pollLast());
                }
                set.add(arr[i]);
                deque.offerLast(arr[i]);
            }
        }
        StringBuilder sb=new StringBuilder();
        while(!deque.isEmpty()){
            sb.append(deque.pollFirst());
        }
        return sb.toString();
    }
}
```
### 复杂度分析
- 时间复杂度：$O(N)$。虽然外循环里面还有一个内循环，但内循环的次数受栈中剩余字符总数的限制，因此最终复杂度仍为 $O(N)$。
- 空间复杂度：$O(1)$。看上去空间复杂度像是 $O(N)$，但这不对！首先， `set` 中字符不重复，其大小受字母表大小的限制。其次，只有 `deque` 中不存在的元素才会被压入，因此 `deque` 中的元素也唯一。所以最终空间复杂度为 $O(1)$。
