# 26. Remove Duplicates from Sorted Array

## Solution 1: 双指针A

### 思路

参考：max-LFszNScOfE，https://leetcode.cn/problems/remove-duplicates-from-sorted-array/solution/shuang-zhi-zhen-shan-chu-zhong-fu-xiang-dai-you-hu/

首先注意数组是**有序**的，那么重复的元素一定会相邻。要求删除重复元素，实际上就是将不重复的元素移到数组的左侧。考虑用 2 个指针，一个在前记作 p，一个在后记作 q，算法流程如下：

1. 比较 p 和 q 位置的元素是否相等。
2. 如果相等，q 后移 1 位
3. 如果不相等，将 q 位置的元素复制到 p+1 位置上，p 后移一位，q 后移 1 位

重复上述过程，直到 q 等于数组长度。

**最后**：返回 p + 1，即为新数组长度。

### 代码
```C++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.size() < 2) return nums.size();
        int i = 0; //前指针
        int j = 1; //后指针
        while (j < nums.size()) {
            if (nums[i] != nums[j]) {
                if (j - i > 1) { //如果p和q是前后相邻，那么就没必要在q的位置上重复赋值
                    nums[i + 1] = nums[j];
                }
                i++;
            }
            j++;
        }
        return i + 1;
    }
};
```

## Solution 2: 通用解法

### 代码
```C++
class Solution {
public:
    // k表示保留重复数目的个数，在这道题里 k=1
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
        return work(nums, 1);
    }
};
```
