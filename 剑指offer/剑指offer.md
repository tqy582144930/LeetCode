[toc]

## 数组中重复元素

### 题目

找出数组中重复的数字。


在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

```c++
示例 1：

输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 
```

### 题解

```c++
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        if (nums.empty()) {
            return -1;
        }
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] < 0 || nums[i] > nums.size() - 1) {
                return -1;
            }
        }

        for (int i = 0; i < nums.size(); i++) {
            while (nums[i] != i) {
                if (nums[i] == nums[nums[i]]) {
                    return nums[i];
                }
                int temp = nums[i];
                nums[i] = nums[temp];
                nums[temp] = temp;
            }
        }
        return -1;
    }
};

// 时间复杂度：O(n)
// 空间复杂度：O(1)
```

### 总结

- 数组中的数字都在0-n-1的范围内，如果数组没有重复的元素，那么当数组排序后i将出现在下标为i的位置；如果数组中有重复的元素，有些位置可能存在多个元素，有些位置可能没有元素
- 扫描数组中的每个元素，当扫描到下标为i的元素，比较这个元素m是否与i相等，如果是接着扫描下一个；如果不是，则拿它和第m个数字进行比较
  - 如果它和第m个数字相等，就返回
  - 否则，就把第i个数字和第m个数字交换，把m放置到属于它的位置

