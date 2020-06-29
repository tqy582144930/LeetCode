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

## 二维数组中的查找

### 题目

在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

 

```c++
示例:

现有矩阵 matrix 如下：

[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
给定 target = 5，返回 true。

给定 target = 20，返回 false。
```

### 题解

```c++
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        if (matrix.empty() || matrix.size() == 0 || matrix[0].size() == 0) {
            return false;
        }
        int rows = matrix.size();
        int colums = matrix[0].size();
        int row = 0, colum = colums - 1;
        while (row < rows && colum >= 0) {
            if (matrix[row][colum] < target) {
                row++;
            } else if (matrix[row][colum] > target) {
                colum--;
            } else {
                return true;
            }
        }
        return false;
    }
};

// 时间复杂度：O(n+m) 访问到的下标的行最多增加 n 次，列最多减少 m 次，因此循环体最多执行 n + m 次。
// 空间复杂度：O(1)
```

### 总结

- 从二维数组的右上角开始查找。如果当前元素等于目标值，则返回 `true`。如果当前元素大于目标值，则移到左边一列。如果当前元素小于目标值，则移到下边一行。

## 替换空格

### 题目

请实现一个函数，把字符串 s 中的每个空格替换成"%20"。

```c++
示例 1：

输入：s = "We are happy."
输出："We%20are%20happy."
```

### 题解

```c++
class Solution {
public:
    string replaceSpace(string s) {
        if (s.empty() || s.size() < 0) {
            return string();
        }

        int indexOfOrigin = s.size() - 1;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == ' ') {
                s += "00";
            }
        }

        int indexOfNew = s.size() - 1;
        while (indexOfOrigin >= 0 && indexOfNew > indexOfOrigin) {
            if (s[indexOfOrigin] == ' ') {
                s[indexOfNew--] = '0';
                s[indexOfNew--] = '2';
                s[indexOfNew--] = '%';
            } else {
                s[indexOfNew--] = s[indexOfOrigin];
            }
            indexOfOrigin--;
        }
        return s;
    }
};
```

### 总结

- 从后往前可以减少移动时间