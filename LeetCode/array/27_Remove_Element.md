# Remove Element

## Solution 1：通用解法

### 代码
```C++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int amount = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] != val) {
                nums[amount++] = nums[i];
            }
        }
        return amount;
    }
};
```

```C++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int idx = 0;
        for(auto x : nums)
            if(x != val)nums[idx++] = x;
        return idx;
    }
};

// 作者：AC_OIer
// 链接：https://leetcode.cn/problems/remove-element/solution/shua-chuan-lc-shuang-bai-shuang-zhi-zhen-mzt8/
```

## Solution 2：双指针

### 思路
根据题意，我们可以将数组分成*前后*两段：
1. 前半段是有效部分，存储的是**不**等于 val 的元素。
2. 后半段是无效部分，存储的是等于 val 的元素。
最终答案返回有效部分的结尾下标

作者：AC_OIer
链接：https://leetcode.cn/problems/remove-element/solution/shua-chuan-lc-shuang-bai-shuang-zhi-zhen-mzt8/

### 代码
```C++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int j = nums.size() - 1;
        for (int i = 0; i <= j; i++) {
            if (nums[i] == val) {
                swap(nums[i--], nums[j--]);
            }
        }
        return j + 1;
    }
};
```