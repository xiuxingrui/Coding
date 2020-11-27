# [LeetCode18.四数之和](https://leetcode-cn.com/problems/4sum/)
## 题目描述
给定一个包含 `n` 个整数的数组 `nums` 和一个目标值 `target`，判断 `nums` 中是否存在四个元素 `a`，`b`，`c` 和 `d` ，使得 `a + b + c + d` 的值与 `target` 相等？找出所有满足条件且不重复的四元组。

答案中不可以包含重复的四元组。

### 示例
```
给定数组 nums = [1, 0, -1, 0, -2, 2]，和 target = 0。

满足要求的四元组集合为：
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```
## 题解
### 解法一
使用两重循环分别枚举前两个数，然后在两重循环枚举到的数之后使用双指针枚举剩下的两个数。假设两重循环枚举到的前两个数分别位于下标 `i` 和 `j`，其中 `i<j`。初始时，左右指针分别指向下标 `j+1` 和下标 `n−1`。每次计算四个数的和，并进行如下操作：

- 如果和等于 `target`，则将枚举到的四个数加到答案中，然后将左指针右移直到遇到不同的数，将右指针左移直到遇到不同的数；
- 如果和小于 `target`，则将左指针右移一位；
- 如果和大于 `target`，则将右指针左移一位。
使用双指针枚举剩下的两个数的时间复杂度是 $O(n)$，因此总时间复杂度是 $O(n^3)$。

具体实现时，还可以进行一些剪枝操作：

- 在确定第一个数之后，如果 `nums[i]+nums[i+1]+nums[i+2]+nums[i+3]>target`，说明此时剩下的三个数无论取什么值，四数之和一定大于 `target`，因此退出第一重循环；
- 在确定第一个数之后，如果 `nums[i]+nums[n−3]+nums[n−2]+nums[n−1]<target`,说明此时剩下的三个数无论取什么值，四数之和一定小于 `target`，因此第一重循环直接进入下一轮，枚举 `nums[i+1]`；
- 在确定前两个数之后，如果 `nums[i]+nums[j]+nums[j+1]+nums[j+2]>target`，说明此时剩下的两个数无论取什么值，四数之和一定大于 `target`，因此退出第二重循环；
- 在确定前两个数之后，如果 `nums[i]+nums[j]+nums[n−2]+nums[n−1]<target`，说明此时剩下的两个数无论取什么值，四数之和一定小于 `target`，因此第二重循环直接进入下一轮，枚举 `nums[j+1]`。

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> ans=new ArrayList<>();
        Arrays.sort(nums);
        int len=nums.length;
        for(int i=0;i<len-3;i++){
            if(i>0&&nums[i]==nums[i-1]){
                continue;
            }
            if(nums[i]+nums[i+1]+nums[i+2]+nums[i+3]>target){
                break;
            }
            if(nums[i]+nums[len-3]+nums[len-2]+nums[len-1]<target){
                continue;
            }
            for(int j=i+1;j<len-2;j++){
                if(j>i+1&&nums[j]==nums[j-1]){
                    continue;
                }
                if(nums[i]+nums[j]+nums[j+1]+nums[j+2]>target){
                    break;
                }
                if(nums[i]+nums[j]+nums[len-2]+nums[len-1]<target){
                    continue;
                }
                int left=j+1,right=len-1;
                while(left<right){
                    int sum=nums[i]+nums[j]+nums[left]+nums[right];
                    if(sum==target){
                        List<Integer> temp=new ArrayList<>();
                        //ans.add(Arrays.asList(nums[i], nums[j], nums[left], nums[right]));
                        temp.add(nums[i]);
                        temp.add(nums[j]);
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
                    }else if(sum>target){
                        right--;
                    }else{
                        left++;
                    }
                }
            }
        }
        return ans;
    }
}
```
### 解法二
回溯+剪枝
```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        Arrays.sort(nums);
        List<List<Integer>> ans=new ArrayList<>();
        backtrack(nums,target,0,0,ans,new ArrayList<Integer>());
        return ans;
    }
    public void backtrack(int[] nums,int target,int start,int cnt,List<List<Integer>> ans,List<Integer> path){
        if(cnt==4){
            if(target==0){
                ans.add(new ArrayList<>(path));
            }
            return;
        }
        for(int i=start;i<nums.length;i++){
            if(i>start&&nums[i]==nums[i-1]){
                continue;
            }
            if(target<0&&nums[i]>=0){
                return;
            }
            path.add(nums[i]);
            backtrack(nums,target-nums[i],i+1,cnt+1,ans,path);
            path.remove(path.size()-1);
        }
    }
}
```
进一步剪枝:
```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        Arrays.sort(nums);
        List<List<Integer>> ans=new ArrayList<>();
        backtrack(nums,0,target,0,0,ans,new ArrayList<Integer>());
        return ans;
    }
    public void backtrack(int[] nums,int sum,int target,int start,int cnt,List<List<Integer>> ans,List<Integer> path){
        if(cnt==4){
            if(target==sum){
                ans.add(new ArrayList<>(path));
            }
            return;
        }
        int len=nums.length;
        for(int i=start;i<len;i++){
            if(i>start&&nums[i]==nums[i-1]){
                continue;
            }
            if(cnt==0&&i<len-3){
                if(nums[i]+nums[i+1]+nums[i+2]+nums[i+3]>target){
                    return;
                }
                if(nums[i]+nums[len-3]+nums[len-2]+nums[len-1]<target){
                    continue;
                }
            }
            if(cnt==1&&i<len-2){
                if(sum+nums[i]+nums[i+1]+nums[i+2]>target){
                    return;
                }
                if(sum+nums[i]+nums[len-2]+nums[len-1]<target){
                    continue;
                }
            }
            if(cnt==2&&i<len-1){
                if(sum+nums[i]+nums[i+1]>target){
                    return;
                }
                if(sum+nums[i]+nums[len-1]<target){
                    continue;
                }
            }
            if(cnt==3&&i<len){
                if(sum+nums[i]>target){
                    return;
                }
                if(sum+nums[i]<target||sum+nums[len-1]<target){
                    continue;
                }

            }
            path.add(nums[i]);
            backtrack(nums,sum+nums[i],target,i+1,cnt+1,ans,path);
            path.remove(path.size()-1);
        }
    }
}
```