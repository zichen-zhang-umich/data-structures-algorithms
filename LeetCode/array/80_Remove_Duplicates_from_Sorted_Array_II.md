# 80. Remove Duplicates from Sorted Array II

## Solution 1: 通用解法

### 思路

参考：AC_OIer，https://leetcode.cn/problems/remove-duplicates-from-sorted-array-ii/solution/gong-shui-san-xie-guan-yu-shan-chu-you-x-glnq/

**我们令 k=2，假设有如下样例，[1,1,1,1,1,1,2,2,2,2,2,2,3]**
1. 首先我们先让前 2 位直接保留，得到 1,1
2. 对后面的每一位进行继续遍历，能够保留的前提是与当前位置的前面 k 个元素不同（答案中的第一个 1），因此我们会跳过剩余的 1，将第一个 2 追加，得到 1,1,2
3. 继续这个过程，这时候是和答案中的第 2 个 1 进行对比，因此可以得到 1,1,2,2
4. 这时候和答案中的第 1 个 2 比较，只有与其不同的元素能追加到答案，因此剩余的 2 被跳过，3 被追加到答案：1,1,2,2,3

### 代码
```C++
class Solution {
public:
    int work(vector<int>& nums, int k) {
        int len = 0; //len相当于慢指针，负责记录下标
        for(auto num : nums) {
            if (len < k || nums[len-k] != num) {
                nums[len++] = num;
            }
        }
        return len;
    }
    int removeDuplicates(vector<int>& nums) {
        return work(nums, 2);
    }
```




