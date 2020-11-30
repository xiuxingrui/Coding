# [LeetCode15.三数之和](https://leetcode-cn.com/problems/3sum/)
## 题目描述
给你一个包含 `n` 个整数的数组 `nums`，判断 `nums` 中是否存在三个元素 `a`，`b`，`c` ，使得 `a + b + c = 0` ？请你找出所有满足条件且不重复的三元组。

注意：答案中不可以包含重复的三元组。

### 示例
```
给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```
## 题解
### 解法一
双指针法铺垫： 先将给定 `nums` 排序，复杂度为 $O(NlogN)$。

双指针法思路： 固定 3 个指针中最左（最小）数字的指针 `k`，双指针 `i`，`j` 分设在数组索引 `[k+1,len(nums)-1]` 两端，通过双指针交替向中间移动，记录对于每个固定指针 `k` 的所有满足 `nums[k] + nums[i] + nums[j] == 0` 的 `i`,`j` 组合：
1. 当 `nums[k] > 0` 时直接`break`跳出：因为 `nums[j] >= nums[i] >= nums[k] > 0`，即 3 个数字都大于 0 ，在此固定指针 `k` 之后不可能再找到结果了。
2. 当 `k >0`且`nums[k] == nums[k - 1]`时即跳过此元素`nums[k]`：因为已经将 `nums[k - 1]`的所有组合加入到结果中，本次双指针搜索只会得到重复组合。
3. `i`，`j` 分设在数组索引 `[k+1,len(nums)-1]` 两端，当`i < j`时循环计算`s = nums[k] + nums[i] + nums[j]`，并按照以下规则执行双指针移动：
- 当`s < 0`时，`i += 1`并跳过所有重复的`nums[i]`；
- 当`s > 0`时，`j -= 1`并跳过所有重复的`nums[j]`；
- 当`s == 0`时，记录组合`[k, i, j]`至`res`，执行`i += 1`和`j -= 1`并跳过所有重复的`nums[i]`和`nums[j]`，防止记录到重复组合。

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> ans=new ArrayList<>();
        if(nums==null||nums.length<3){
            return ans;
        }
        Arrays.sort(nums);
        int len=nums.length;
        for(int i=0;i<len-2;i++){
            if(nums[i]>0){
                break;
            }
            if(i>0&&nums[i]==nums[i-1]){
                continue;
            }
            int left=i+1,right=len-1;
            while(left<right){
                int sum=nums[i]+nums[left]+nums[right];
                if(sum==0){
                    List<Integer> temp=new ArrayList<>();
                    //ans.add(new ArrayList<Integer>(Arrays.asList(nums[i], nums[left], nums[right])))
                    temp.add(nums[i]);
                    temp.add(nums[left]);
                    temp.add(nums[right]);
                    ans.add(temp);
                    while(left<right&&nums[left]==nums[left+1]){
                        left++;
                    }
                    left++;
                    while(left<right&&nums[right]==nums[right-1]){
                        right--;
                    }
                    right--;
                }else if(sum<0){
                    left++;
                }else{
                    right--;
                }
            }
        }
        return ans;
    }
}
```
### 复杂度分析
- 时间复杂度 $O(N^2)$：其中固定指针`k`循环复杂度 $O(N)$，双指针 `i`，`j` 复杂度 $O(N)$。
- 空间复杂度 $O(1)$：指针使用常数大小的额外空间。
