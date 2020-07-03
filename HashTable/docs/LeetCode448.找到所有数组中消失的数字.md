# [LeetCode448.找到所有数组中消失的数字](https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/)
## 题目描述
给定一个范围在  `1 ≤ a[i] ≤ n` ( `n` = 数组大小 ) 的 整型数组，数组中的元素一些出现了两次，另一些只出现一次。

找到所有在 `[1, n]` 范围之间没有出现在数组中的数字。

您能在不使用额外空间且时间复杂度为$O(n)$的情况下完成这个任务吗? 你可以假定返回的数组不算在额外空间内。
### 示例
```
输入:
[4,3,2,7,8,2,3,1]

输出:
[5,6]
```
## 题解
我们需要知道数组中存在的数字，由于数组的元素取值范围是 `[1, N]`，所以我们可以不使用额外的空间去解决它。

我们可以在输入数组本身以某种方式标记已访问过的数字，然后再找到缺失的数字。

### 算法
- 遍历输入数组的每个元素一次。
- 我们将把 `|nums[i]|-1` 索引位置的元素标记为负数。
- 然后遍历数组，若当前数组元素 `nums[i]` 为负数，说明我们在数组中存在数字 `i+1`。
```java
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        
        // Iterate over each of the elements in the original array
        for (int i = 0; i < nums.length; i++) {
            
            // Treat the value as the new index
            int newIndex = Math.abs(nums[i]) - 1;
            
            // Check the magnitude of value at this new index
            // If the magnitude is positive, make it negative 
            // thus indicating that the number nums[i] has 
            // appeared or has been visited.
            if (nums[newIndex] > 0) {
                nums[newIndex] *= -1;
            }
        }
        
        // Response array that would contain the missing numbers
        List<Integer> result = new LinkedList<Integer>();
        
        // Iterate over the numbers from 1 to N and add all those
        // that have positive magnitude in the array
        for (int i = 1; i <= nums.length; i++) {
            
            if (nums[i - 1] > 0) {
                result.add(i);
            }
        }
        
        return result;
    }
}
```
### 复杂度分析
- 时间复杂度：$O(N)$。
- 空间复杂度：$O(1)$，因为我们在原地修改数组，没有使用额外的空间。

