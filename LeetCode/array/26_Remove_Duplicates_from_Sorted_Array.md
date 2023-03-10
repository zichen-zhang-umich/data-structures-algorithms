# 26. Remove Duplicates from Sorted Array

```C++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.size() < 2) return nums.size();
        int i = 0, j = 1;
        while (j < nums.size()) {
            if (nums[i] != nums[j]) {
                if (j - i > 1) {
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