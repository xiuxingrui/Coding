# [LeetCode126.单词接龙II](https://leetcode-cn.com/problems/word-ladder-ii/)
## 题目描述
给定两个单词（`beginWord` 和 `endWord`）和一个字典 `wordList`，找出所有从 `beginWord` 到 `endWord` 的最短转换序列。转换需遵循如下规则：

- 每次转换只能改变一个字母。
- 转换后得到的单词必须是字典中的单词。
- 
说明:

- 如果不存在这样的转换序列，返回一个空列表。
- 所有单词具有相同的长度。
- 所有单词只由小写字母组成。
- 字典中不存在重复的单词。
- 你可以假设 `beginWord` 和 `endWord` 是非空的，且二者不相同。

### 示例
```
输入:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

输出:
[
  ["hit","hot","dot","dog","cog"],
  ["hit","hot","lot","log","cog"]
]
```
```
输入:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

输出: []

解释: endWord "cog" 不在字典中，所以不存在符合要求的转换序列。
```
## 题解
### 解法一
`BFS`超时：
```java
class Solution {
    public List<List<String>> findLadders(String beginWord, String endWord, List<String> wordList) {
        // 结果集
        List<List<String>> res = new ArrayList<>();
        Set<String> distSet = new HashSet<>(wordList);
        // 字典中不包含目标单词
        if (!wordList.contains(endWord)) {
            return res;
        }
        // 已经访问过的单词集合：只找最短路径，所以之前出现过的单词不用出现在下一层
       Set<String> visited = new HashSet<>();
        // 累积每一层的结果队列
        Queue<List<String>> queue= new LinkedList<>();
        List<String> list = new ArrayList<>();
        list.add(beginWord);
        queue.add(list);
        visited.add(beginWord);
        // 是否到达符合条件的层：如果该层添加的某一单词符合目标单词，则说明截止该层的所有解为最短路径，停止循环
        boolean flag = false;
        while (!queue.isEmpty() && !flag) {
            // 上一层的结果队列
            int size = queue.size();
            // 该层添加的所有元素：每层必须在所有结果都添加完新的单词之后，再将这些单词统一添加到已使用单词集合
            // 如果直接添加到 visited 中，会导致该层本次结果添加之后的相同添加行为失败
            // 如：该层遇到目标单词，有两条路径都可以遇到，但是先到达的将该单词添加进 visited 中，会导致第二条路径无法添加
            HashSet<String> set=new HashSet<>();
            for (int i = 0; i < size; i++) {
                List<String> path = queue.poll();
                // 获取该路径上一层的单词
                String word = path.get(path.size() - 1);
                for(int j=0;j<wordList.size();j++){
                    String temp=wordList.get(j);
                    if(!visited.contains(temp)&&check(temp,word)){
                        List<String> pathList=new ArrayList(path);
                        pathList.add(temp);
                        set.add(temp);
                        if(temp.equals(endWord)){
                            flag=true;
                            res.add(pathList);
                        }
                        queue.offer(pathList);
                    }
                }
            }
            visited.addAll(set);
        }
        return res;
    }
    public boolean check(String s,String p){
        char[] arrS=s.toCharArray();
        char[] arrP=p.toCharArray();
        int len=s.length();
        int cnt=0;
        for(int i=0;i<len;i++){
            if(arrP[i]!=arrS[i]){
                cnt++;
                if(cnt>1){
                    return false;
                }
            }
        }
        return cnt==1;
    }
}
```
优化：

思路说明：在到达最短路径所在的层时，记录并输出所有符合条件的路径。

1. 在单词接龙的基础上，需要将找到的最短路径存储下来；
2. 之前的队列只用来存储每层的元素，那么现在就得存储每层添加元素之后的结果：`"ab"`,`"if"`,`{"cd","af","ib","if"}`；
   1. 第一层：`{"ab"}`
   2. 第二层：`{"ab","af"}`、`{"ab","ib"}`
   3. 第三层：`{"ab","af","if"}`、`{"ab","ib","if"}`
3. 如果该层添加的某一个单词符合目标单词，则该路径为最短路径，该层为最短路径所在的层，但此时不能直接返回结果，必须将该层遍历完，将该层所有符合的结果都添加进结果集；
4. 每层添加单词的时候，不能直接添加到总的已访问单词集合中，需要每层有一个单独的该层访问的单词集，该层结束之后，再会合到总的已访问单词集合中，原因就是因为3.

```java
class Solution {
    public List<List<String>> findLadders(String beginWord, String endWord, List<String> wordList) {
        // 结果集
        List<List<String>> res = new ArrayList<>();
        Set<String> distSet = new HashSet<>(wordList);
        // 字典中不包含目标单词
        if (!distSet.contains(endWord)) {
            return res;
        }
        // 已经访问过的单词集合：只找最短路径，所以之前出现过的单词不用出现在下一层
        Set<String> visited = new HashSet<>();
        // 累积每一层的结果队列
        Queue<List<String>> queue= new LinkedList<>();
        List<String> list = new ArrayList<>(Arrays.asList(beginWord));
        queue.add(list);
        visited.add(beginWord);
        // 是否到达符合条件的层：如果该层添加的某一单词符合目标单词，则说明截止该层的所有解为最短路径，停止循环
        boolean flag = false;
        while (!queue.isEmpty() && !flag) {
            // 上一层的结果队列
            int size = queue.size();
            // 该层添加的所有元素：每层必须在所有结果都添加完新的单词之后，再将这些单词统一添加到已使用单词集合
            // 如果直接添加到 visited 中，会导致该层本次结果添加之后的相同添加行为失败
            // 如：该层遇到目标单词，有两条路径都可以遇到，但是先到达的将该单词添加进 visited 中，会导致第二条路径无法添加
            Set<String> subVisited = new HashSet<>();
            for (int i = 0; i < size; i++) {
                List<String> path = queue.poll();
                // 获取该路径上一层的单词
                String word = path.get(path.size() - 1);
                char[] chars = word.toCharArray();
                // 寻找该单词的下一个符合条件的单词
                for (int j = 0; j < chars.length; j++) {
                    char temp = chars[j];
                    for (char ch = 'a'; ch <= 'z'; ch++) {
                        chars[j] = ch;
                        if (temp == ch) {
                            continue;
                        }
                        String str = new String(chars);
                        // 符合条件：在 wordList 中 && 之前的层没有使用过
                        if (distSet.contains(str) && !visited.contains(str)) {
                            // 生成新的路径
                            List<String> pathList = new ArrayList<>(path);
                            pathList.add(str);
                            // 如果该单词是目标单词：将该路径添加到结果集中，查询截止到该层
                            if (str.equals(endWord)) {
                                flag = true;
                                res.add(pathList);
                            }
                            // 将该路径添加到该层队列中
                            queue.add(pathList); 
                            // 将该单词添加到该层已访问的单词集合中
                            subVisited.add(str);
                        }
                    }
                    chars[j] = temp;
                }
            }
            // 将该层所有访问的单词添加到总的已访问集合中
            visited.addAll(subVisited);
        }
        return res;
    }
}
```
### 解法二
回溯，超时
```java
class Solution {
    int ans=Integer.MAX_VALUE;
    public List<List<String>> findLadders(String beginWord, String endWord, List<String> wordList) {
        List<List<String>> list=new ArrayList<>();
        if(!wordList.contains(endWord)){
            return list;
        }
        boolean[] visited=new boolean[wordList.size()];
        List<String> path=new ArrayList<>();
        path.add(beginWord);
        backtracking(beginWord,beginWord,endWord,wordList,1,visited,path,list);
        return list;
    }
    public void backtracking(String s,String beginWord,String endWord,List<String> wordList,int cnt,boolean[] visited,List<String> path,List<List<String>> list){
        if(s.equals(endWord)){
            if(cnt==ans){
                list.add(new ArrayList(path));
            }
            if(cnt<ans){
                ans=cnt;
                list.clear();
                list.add(new ArrayList(path));
            }
            return;
        }
        for(int i=0;i<wordList.size();i++){
            if(visited[i]||!check(s,wordList.get(i))||wordList.get(i).equals(beginWord)){
                continue;
            }
            visited[i]=true;
            String temp=wordList.get(i);
            path.add(temp);
            backtracking(temp,beginWord,endWord,wordList,cnt+1,visited,path,list);
            visited[i]=false;
            path.remove(path.size()-1);
        }
    }
    public boolean check(String s,String p){
        char[] arrS=s.toCharArray();
        char[] arrP=p.toCharArray();
        int len=s.length();
        int cnt=0;
        for(int i=0;i<len;i++){
            if(arrP[i]!=arrS[i]){
                cnt++;
            }
        }
        return cnt==1;
    }
}
```