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

## 从尾到头打印链表

### 题目

输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

**示例 1：**

```c++
输入：head = [1,3,2]
输出：[2,3,1]
```

### 题解

```c++
class Solution {
public:
    vector<int> reversePrint(ListNode* head) {
        if (head == NULL) {
            return vector<int>();
        }
        stack<int> st;
        ListNode *ptr = head;
        while (ptr != NULL) {
            st.push(ptr->val);
            ptr = ptr->next;
        }
        vector<int> ve;
        while (!st.empty()) {
            int val = st.top();
            st.pop();
            ve.push_back(val);
        }
        return ve;
    }
};

// 时间复杂度 O(n)
// 空间复杂度 O(n)
```

## 重建二叉树

### 题目

输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

例如，给出

```c++
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
```


返回如下的二叉树：

```c++
    3
   / \
  9  20
    /  \
   15   7
```
### 题解

```c++
class Solution {
public:
    vector<int> pre;
    vector<int> is;
    int index;

    int findIs(int val) {
        int pos = -1;
        for (int i = 0; i < is.size(); i++) {
            if (is[i] == val) {
                pos = i;
                break;
            }
        }
        return pos;
    }

    TreeNode *creatPI(int left, int right) {
        if (left > right) {
            return NULL;
        }
        TreeNode *node = new TreeNode(pre[index]);
        int pos = findIs(pre[index]);
        index++;
        node->left = creatPI(left, pos - 1);
        node->right = creatPI(pos + 1, right);
        return node;
    }

    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        pre = preorder;
        is = inorder;
        index = 0;
        if (preorder.empty() || inorder.empty()) {
            return NULL;
        } else {
            return creatPI(0, inorder.size() - 1);
        }
    }
};
```

### 总结

- 前序：根节点+左节点+右节点
- 中序：左节点+根节点+右节点

## 用两个栈实现队列

### 题目

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )

```c++
示例 1：

输入：
["CQueue","appendTail","deleteHead","deleteHead"]
[[],[3],[],[]]
输出：[null,null,3,-1]
示例 2：

输入：
["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
[[],[],[5],[2],[],[]]
输出：[null,-1,null,null,5,2]
```

### 题解

```c++
class CQueue {
public:
    stack<int> stack1, stack2;
    CQueue() {
        while (!stack1.empty()) {
            stack1.pop();
        }
        while (!stack2.empty()) {
            stack2.pop();
        }
    }
    // 使用stack1用来接受新添加的元素
    void appendTail(int value) {
        stack1.push(value);
    }
    
    // 使用stack2使得stack1里面的元素倒叙，再从stack2中弹出就满足先进先出
    int deleteHead() {
        if (stack2.empty()) {
            while (!stack1.empty()) {
                int val = stack1.top();
                stack1.pop();
                stack2.push(val);
            }
        }
        if (stack2.empty()) {
            return -1;
        } else {
            int val = stack2.top();
            stack2.pop();
            return val;
        }
    }
};

// 时间复杂度：O(1)
// 空间复杂度：O(n)：需要用两个栈来存储元素
```

## 斐波那契数列

### 题目

写一个函数，输入 n ，求斐波那契（Fibonacci）数列的第 n 项。斐波那契数列的定义如下：

F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。 

```c++
示例 1：

输入：n = 2
输出：1
示例 2：

输入：n = 5
输出：5
```

### 题解

```c++
class Solution {
public:
    int fib(int n) {
        if (n < 0) {
            return -1;
        } else if (n == 0) {
            return 0;
        } else if (n == 1) {
            return 1;
        }
        
        int a = 0, b = 1;
        int res = 0;
        for (int i = 2; i <= n; i++) {
            res = (a + b) % 1000000007;
            a = b;
            b = res;
        }
        return res;
    }
};

// 时间复杂；O(n)
// 空间复杂：O(1)
```

## 青蛙跳台阶

### 题目

一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个 n 级的台阶总共有多少种跳法。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

```c++
示例 1：

输入：n = 2
输出：2
示例 2：

输入：n = 7
输出：21
提示：

0 <= n <= 100
```

### 题解

```c++
class Solution {
public:
    int numWays(int n) {
        if (n < 0) {
            return -1;
        } else if (n == 0 || n == 1) {
            return 1;
        } else if (n == 2) {
            return 2;
        }
        
        int a = 1, b = 2;
        int res = 0;
        for (int i = 3; i <= n; i++) {
            res = (a + b) % 1000000007;
            a = b;
            b = res;
        }
        return res;
    }
};
```

## 旋转数组的最小元素

### 题目

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 的一个旋转，该数组的最小值为1。  

```c++
示例 1：

输入：[3,4,5,1,2]
输出：1
示例 2：

输入：[2,2,2,0,1]
输出：0
```

### 题解

```c++
class Solution {
public:
    int minInOrder(vector<int>& numbers) {
        int res = numbers[0];
        for (int i = 1; i < numbers.size(); i++) {
            if (res > numbers[i]) {
                res = numbers[i];
            }
        }
        return res;
    }

    int minArray(vector<int>& numbers) {
        if (numbers.empty()) {
            return -1;
        }
        int i = 0, j = numbers.size() - 1;
        // 如果旋转数组本身就是排序数组，数组中的第一个数就是最小的数，可以直接返回。所以我们把mid初始化为i
        int mid = i;
        while (numbers[i] >= numbers[j]) {
            if (j - i == 1) {
                mid = j;
                break;
            }

            mid = (i + j) / 2;
            // 如果下标i、j和mid都相等，则只能顺序查找
            if (numbers[i] == numbers[j] && numbers[i] == numbers[mid]) {
                return minInOrder(numbers);
            }
            if (numbers[i] <= numbers[mid]) {
                i = mid;
            } else if (numbers[j] >= numbers[mid]) {
                j = mid;
            }
        }
        return numbers[mid];
    }
};
```

