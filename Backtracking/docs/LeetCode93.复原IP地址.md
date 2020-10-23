# [LeetCode93.复原IP地址](https://leetcode-cn.com/problems/restore-ip-addresses/)
## 题目描述
给定一个只包含数字的字符串，复原它并返回所有可能的 `IP` 地址格式。

有效的 `IP` 地址 正好由四个整数（每个整数位于 0 到 255 之间组成，且不能含有前导 0），整数之间用 `'.'` 分隔。

例如：`"0.1.2.201"` 和 `"192.168.1.1"` 是 有效的 `IP` 地址，但是 `"0.011.255.245"`、`"192.168.1.312"` 和 `"192.168@1.1"` 是 无效的 `IP` 地址。

### 示例
```
输入：s = "25525511135"
输出：["255.255.11.135","255.255.111.35"]
```
```
输入：s = "0000"
输出：["0.0.0.0"]
```
```
输入：s = "1111"
输出：["1.1.1.1"]
```
```
输入：s = "010010"
输出：["0.10.0.10","0.100.1.0"]
```
```
输入：s = "101023"
输出：["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"]
```
## 题解
由于我们需要找出所有可能复原出的 `IP` 地址，因此可以考虑使用递归的方法，对所有可能的字符串分隔方式进行搜索，并筛选出满足要求的作为答案。

设题目中给出的字符串为 `s`。我们用递归函数 `dfs(segId,segStart)` 表示我们正在从 `s[segStart]` 的位置开始，搜索 `IP` 地址中的第 `segId` 段，其中 `segId∈{0,1,2,3}`。由于 `IP` 地址的每一段必须是 `[0,255]` 中的整数，因此我们从 `segStart` 开始，从小到大依次枚举当前这一段 `IP` 地址的结束位置 `segEnd`。如果满足要求，就递归地进行下一段搜索，调用递归函数 `dfs(segId+1,segEnd+1)`。

特别地，由于 `IP` 地址的每一段不能有前导零，因此如果 `s[segStart]` 等于字符 0，那么 `IP` 地址的第 `segId` 段只能为 0，需要作为特殊情况进行考虑。

在递归搜索的过程中，如果我们已经得到了全部的 4 段 `IP` 地址（即 `segId=4`），并且遍历完了整个字符串（即 `segStart=∣s∣`，其中 `∣s∣` 表示字符串 `s`的长度），那么就复原出了一种满足题目要求的 `IP` 地址，我们将其加入答案。在其它的时刻，如果提前遍历完了整个字符串，那么我们需要结束搜索，回溯到上一步。


```java
class Solution {
    public List<String> restoreIpAddresses(String s) {
        List<String> ans = new ArrayList<String>();
        int[] segments = new int[4];
        dfs(s.toCharArray(), 0, 0,segments,ans);
        return ans;
    }
    public void dfs(char[] arr, int segId, int segStart,int[] segments,List<String> ans) {
        // 如果找到了 4 段 IP 地址并且遍历完了字符串，那么就是一种答案
        if (segId == 4) {
            if (segStart == arr.length) {
                StringBuilder ipAddr = new StringBuilder();
                for (int i = 0; i < 4; ++i) {
                    ipAddr.append(segments[i]);
                    if (i !=3) {
                        ipAddr.append('.');
                    }
                }
                ans.add(ipAddr.toString());
            }
            return;
        }

        // 如果还没有找到 4 段 IP 地址就已经遍历完了字符串，那么提前回溯
        if (segStart == arr.length) {
            return;
        }

        // 由于不能有前导零，如果当前数字为 0，那么这一段 IP 地址只能为 0
        if (arr[segStart] == '0') {
            segments[segId] = 0;
            dfs(arr, segId + 1, segStart + 1,segments,ans);
        }

        // 一般情况，枚举每一种可能性并递归
        int addr = 0;
        for (int segEnd = segStart; segEnd < arr.length; ++segEnd) {
            addr = addr * 10 + (arr[segEnd] - '0');
            if (addr > 0 && addr <= 255) {
                segments[segId] = addr;
                dfs(arr, segId + 1, segEnd + 1,segments,ans);
            } else {
                break;
            }
        }
    }
}
```
### 复杂度分析
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201023160242.png)
