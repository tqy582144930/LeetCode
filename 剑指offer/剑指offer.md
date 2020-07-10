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

## 矩阵中的路径

### 题目

请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3×4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用加粗标出）。

[["a","b","c","e"],
["s","f","c","s"],
["a","d","e","e"]]

但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。

 

```c++
示例 1：

输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true
示例 2：

输入：board = [["a","b"],["c","d"]], word = "abcd"
输出：false
```

### 题解

```c++
class Solution {
public:
    bool exist(vector<vector<char>>& board, string word) {
        for (int i = 0; i < board.size(); i++) {
            for (int j = 0; j < board[0].size(); j++) {
                if (dfs(board, word, i, j, 0))
                    return true;
            }
        }
        return false;
    }
private:
    bool dfs(vector<vector<char>>& board, string& word, int i, int j, int k) {
        if (i < 0 || i >= board.size() || j < 0 || j >= board[0].size() || board[i][j] != word[k]) {
            return false;
        }
        if (k == word.size() - 1) {
            return true;
        }
        char temp = board[i][j];
        board[i][j] = '/';
        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
        for (int q = 0; q < 4; q++) {
            int m = i + dx[q], n = j + dy[q];
            if (dfs(board, word, m, n, k + 1)) {
                return true;
            }
        }
        board[i][j] = temp;
        return false;
    }
};
// 时间复杂度：O(3KMN) ： 最差情况下，需要遍历矩阵中长度为 KK 字符串的所有方案，时间复杂度为 O(3^K)
// 矩阵中共有 MNMN 个起点，时间复杂度为 O(MN) 。
// 方案数计算： 设字符串长度为 KK ，搜索中每个字符有上、下、左、右四个方向可以选择，舍弃回头（上个字符）的方向，剩下 33 种选择，因此方案数的复杂度为 O(3^K)
// 空间复杂度：O(k)
```

## 机器人的运动范围

### 题目

地上有一个m行n列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

 

```c++
示例 1：

输入：m = 2, n = 3, k = 1
输出：3
示例 2：

输入：m = 3, n = 1, k = 0
输出：1
提示：

1 <= n,m <= 100
0 <= k <= 20
```

### 题解

```c++
class Solution {
private:
    int get(int number) {
        int sum = 0;
        while (number > 0) {
            sum += number % 10;
            number /= 10;
        }
        return sum;
    }

    int movingCount(int rows, int colnums, int row, int col, int k, vector<vector<bool>>& visited) {
        int count = 0;
        // 在边界范围之内，并且没有访问过
        if (row >= 0 && row < rows && col >= 0 && col < colnums && get(row) + get(col) <= k && !visited[row][col]) {
            visited[row][col] = true;
            count = 1 + movingCount(rows, colnums, row - 1, col, k, visited) 
                    + movingCount(rows, colnums, row, col - 1, k, visited)
                    + movingCount(rows, colnums, row + 1, col, k, visited)
                    + movingCount(rows, colnums, row, col + 1, k, visited);
        }
        return count;
    }
public:
    int movingCount(int m, int n, int k) {
        if (m <= 0 || n <= 0 || k < 0) {
            return 0;
        }
        vector<vector<bool>> visited(m, vector<bool>(n, false));
        int count = movingCount(m, n, 0, 0, k, visited);
        return count;
    }
};

// 时间复杂度：O(mn)
// 空间复杂度：O(mn)
```

## 剪绳子I

### 题目

给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m-1] 。请问 k[0]*k[1]*...*k[m-1] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

```c++
示例 1：

输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1
示例 2:

输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36
```

### 题解

```c++
class Solution {
public:
    int cuttingRope(int n) {
        if (n < 2) {
            return 0;
        } else if (n == 2) {
            return 1;
        } else if (n == 3) {
            return 2;
        }

        vector<int> product(n + 1);
        product[0] = 0;
        product[1] = 1;
        product[2] = 2;
        product[3] = 3;

        int max = 0;
        // 求解顺序是自下向上，因此在求f(i)之前，对每一个j而言，f(j)已经求解出来
        for (int i = 4; i <= n; i++) {
            max = 0;
            // 比较每一种可能，求出最大的可能
            for (int j = 1; j <= i / 2; j++) {
                if (max < product[j] * product[i - j]) {
                    max = product[j] * product[i - j];
                }
                product[i] = max;
            }
        }
        max = product[n];
        return max;
    }
};
```

## 二进制中1的个数

### 题目

请实现一个函数，输入一个整数，输出该数二进制表示中 1 的个数。例如，把 9 表示成二进制是 1001，有 2 位是 1。因此，如果输入 9，则该函数输出 2。

```c++
示例 1：

输入：00000000000000000000000000001011
输出：3
解释：输入的二进制串 00000000000000000000000000001011 中，共有三位为 '1'
```

### 题解

```c++
// 为了避免循环，我们可以不右移输入的数字n。可以左移与n做&运算的1
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int count = 0;
        unsigned int flag = 1;
        while (flag) {
            if (n & flag) {
                count++;
            }
            flag = flag << 1;
        } 
        return count;
    }
};

// 把一个整数减去1，再和原整数做与运算，会把该整数最右边的1置为0。那么一个整数的二进制有
// 多少个1就会进行多少次这个操作
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int count = 0;
        while (n) {
            count++;
            n = (n - 1) & n;
        } 
        return count;
    }
};
```

## 数值的整次方

### 题目

实现函数double Power(double base, int exponent)，求base的exponent次方。不得使用库函数，同时不需要考虑大数问题。

```c++
示例 1:

输入: 2.00000, 10
输出: 1024.00000
```

### 题解

```c++
class Solution {
public:
    double myPow(double x, int n) {
        // 如果底数为0，它的任意次方都为0
        if (x == 0) return 0;
        long b = n;
        double res = 1.0;
        // 考虑次方小于0，先把底数取倒数，再把次方取正数
        // (5)-1 = 1/(5)1
        if (b < 0) {
            x = 1 / x;
            b = (-b);
        }
        // 如果是偶次方：(a)^n = a^(n/2) * a^(n/2)
        // 如果是奇次方：(a)^n = a^[(n - 1)/2] * a^[(n - 1)/2] * a
        while (b > 0) { 
            // 判断次方是奇数还是偶数
            if ((b & 1) == 1) {
                res *= x;
            }
            x *= x;
            // 把次方除以二
            b >>= 1;
        }
        return res;
    }
};
```

## 打印从1到最大的n位数

### 题目

输入数字 n，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。

```c++
示例 1:

输入: n = 1
输出: [1,2,3,4,5,6,7,8,9]
```

### 题解

```c++
class Solution {
public:
    vector<int> res;
    vector<int> printNumbers(int n) {
        if (n <= 0) {
            return vector<int>();
        }
        vector<char> number(n, '0');
        while (!increment(number)) {
            printNumber(number);
        }
        return res;
    }

    bool increment(vector<char>& number) {
        bool isOverflow = false;
        int carray = 0;
        int length = number.size();
        for (int i = length - 1; i >= 0; i--) {
            int sum = number[i] - '0' + carray;
            if (i == length - 1) {
                sum++;
            }
            if (sum >= 10) {
                if (i == 0) {
                    isOverflow = true;
                } else {
                    carray = 1;
                    number[i] = sum - 10 + '0';
                }
            } else {
                number[i] = sum + '0';
                break;
            }
        }
        return isOverflow;
    }

    void printNumber(vector<char>& number) {
        string s = "";
        bool isBegin0 = true;
        for (char x : number) {
            if (isBegin0 && x != '0') {
                isBegin0 = false;
            }
            if (!isBegin0) {
                s += x;
            }
        }
        int num = stoi(s);
        res.push_back(num);
    }
};
```

## 表示数值的字符串

### 题目

请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100"、"5e2"、"-123"、"3.1416"、"0123"都表示数值，但"12e"、"1a3.14"、"1.2.3"、"+-5"、"-1E-16"及"12e+5.4"都不是。

### 题解

```c++
class Solution {
private:
    bool scanInt(string& s, int& index) {
        if (s[index] == '+' || s[index] == '-') {
            index++;
        }
        return scanUnsignedInt(s, index);
    }

    bool scanUnsignedInt(string& s, int& index) {
        bool flag = false;
        while (s[index] >= '0' && s[index] <= '9' && index < s.size()) {
            index++;
            flag = true;
        }
        return flag;
    }

public:
    bool isNumber(string s) {
        // 去除首尾空格
        int len = 0;
        for (int i = s.size() - 1; i >= 0; i--) {
            if (s[i] == ' ') {
                len++;
            } else {
                break;
            }
        }
        s = s.substr(0, s.size() - len);
        len = 0;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == ' ') {
                len++;
            } else {
                break;
            }
        }
        s = s.substr(len);

        if (s.size() == 0) {
            return false;
        }
        int index = 0;
        
        // 判断整数部分是否 是可带符号数字
        bool isNum = scanInt(s, index);
        // 判断小数部分
        if (s[index] == '.') {
            index++;
            // 小数点前面有带符号数字 或者 小数点后面有数字 即可
            isNum = isNum | scanUnsignedInt(s, index);
        }
        // 判断指数部分
        if (s[index] == 'e' || s[index] == 'E') {
            index++;
            // e或者E的前面 得是符合要求数字，并且后面 得是可带符号的数字
            isNum = isNum & scanInt(s, index);
        }
        return isNum & (index == s.size());
    }
};
```

## 调整数组顺序使奇数位于偶数前面

### 题目

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。

```c++
示例：

输入：nums = [1,2,3,4]
输出：[1,3,2,4] 
注：[3,1,2,4] 也是正确的答案之一。
```

### 题解

```c++
class Solution {
public:
    vector<int> exchange(vector<int>& nums) {
        if (nums.empty()) {
            return vector<int>();
        }
        int i = 0, j = nums.size() - 1;
        while (i < j) {
            while ((i < j) && nums[i] % 2 == 1) {
                i++;
            }
            while ((i < j) && nums[j] % 2 == 0) {
                j--;
            }
            if (i < j) {
                int temp = nums[i];
            	nums[i] = nums[j];
            	nums[j] = temp;
            }
        }
        return nums;
    }
};
```

## 树的字结构

### 题目

输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。

例如:
给定的树 A:

```c++
     3
    / \
   4   5
  / \
 1   2
 
   4 
  /
 1
返回 true，因为 B 与 A 的一个子树拥有相同的结构和节点值。
```
```c++
示例 1：

输入：A = [1,2,3], B = [3,1]
输出：false
```

### 题解

```c++
class Solution {
public:
    bool isSubStructure(TreeNode* A, TreeNode* B) {
        if (A == NULL || B == NULL) {
            return false;
        }
        // 先判断A和B是否相等，接着匹配A->left和B  A->right和B
        return  (DoesTreeAHaveTreeB(A, B) || isSubStructure(A->left, B) ||
                isSubStructure(A->right, B));
    }
	
    // 这个函数用来判断两个树对应的节点是否相等
    bool DoesTreeAHaveTreeB(TreeNode *A, TreeNode *B) {
        if (B == NULL) {
            return true;
        } 
        if (A == NULL || A->val != B->val) {
            return false;
        }
        return DoesTreeAHaveTreeB(A->left, B->left) && DoesTreeAHaveTreeB(A->right, B->right);
    }
};
```

## 镜像二叉树

### 题目

请完成一个函数，输入一个二叉树，该函数输出它的镜像。

例如输入：

```c++
 	  4
    /   \
  2     7
 / \   / \
1   3 6   9
镜像输出：
```
```c++
 	4
  /   \
  7     2
 / \   / \
9   6 3   1

示例 1：
输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]
```
### 题解

```c++
class Solution {
public:
    TreeNode* mirrorTree(TreeNode* root) {
        if (root == NULL) {
            return NULL;
        }
        // 通过前序遍历来改变左右节点
        TreeNode *temp = root->left;
        root->left = mirrorTree(root->right);
        root->right = mirrorTree(temp);
        return root;
    }
};
```

## 对称的二叉树

### 题目

请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。

```c++
	1
   / \
  2   2
 / \ / \
3  4 4  3但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:
```
```c++
	1
   / \
  2   2
   \   \
   3    3
```
### 题解

```c++
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        return root == NULL ? true : recur(root->left, root->right);
    }

    bool recur(TreeNode *left, TreeNode *right) {
        if (left == NULL && right == NULL) {
            return true;
        }
        if (left == NULL || right == NULL || left->val != right->val) {
            return false;
        }
        return recur(left->left, right->right) && recur(left->right, right->left);
    }
};
```

## 顺时针打印矩阵

### 题目

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

```c++
示例 1：

输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
示例 2：

输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
输出：[1,2,3,4,8,12,11,10,9,5,6,7]

### 
```

### 题解

```c++
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        if (matrix.size() == 0 || matrix[0].size() == 0) {
            return {};
        }

        int rows = matrix.size(), columns = matrix[0].size();
        vector<int> res;
        int top = 0, left = 0, bottom = rows - 1, right = columns - 1;
        while (left <= right && top <= bottom) {
            for (int i = left; i <= right; i++) {
                res.push_back(matrix[top][i]);
            }
            for (int i = top + 1; i <= bottom; i++) {
                res.push_back(matrix[i][right]);
            }
            if (left < right && top < bottom) {
                for (int i = right - 1; i > left; i--) {
                    res.push_back(matrix[bottom][i]);
                }
                for (int i = bottom; i > top; i--) {
                    res.push_back(matrix[i][left]);
                }
            }
            left++;
            right--;
            top++;
            bottom--;
        }
        return res;
    }
};

// 时间复杂度：O(mn)
// 空间复杂度：O(1)
```

## 包含min函数的栈

### 题目

定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。

### 题解

```c++
class MinStack {
public:
    stack<int> m_data, m_min;
    MinStack() {
        while(!m_data.empty()) {
            m_data.pop();
        }
        while(!m_min.empty()) {
            m_min.pop();
        }
    }
    
    void push(int x) {
        m_data.push(x);
        if (m_min.size() == 0 || x < m_min.top()) {
            m_min.push(x);
        } else {
            m_min.push(m_min.top());
        }
    }
    
    void pop() {
        assert(m_data.size() > 0 && m_min.size() > 0);

        m_data.pop();
        m_min.pop();
    }
    
    int top() {
        return m_data.top();
    }
    
    int min() {
        assert(m_min.size() > 0);
        return m_min.top();
    }
};
```

## 栈的压入、弹出序列

### 题目

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。

```c++
示例 1：

输入：pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
输出：true
解释：我们可以按以下顺序执行：
push(1), push(2), push(3), push(4), pop() -> 4,
push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1
```

### 题解

```c++
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
        stack<int> stack;
        int i = 0;
        for (int num : pushed) {
            stack.push(num);
            // 如果不为空，并且辅助栈顶元素和弹出序列元素是否相等
            while (!stack.empty() && stack.top() == popped[i]) {
                stack.pop();
                i++;
            }
        }
        return stack.empty();
    }
};
```

## 之子型打印二叉树

### 题目

请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。

例如:
给定二叉树: [3,9,20,null,null,15,7],

    	3
       / \
      9  20
        /  \
       15   7
    返回其层次遍历结果：
    
    [
      [3],
      [20,9],
      [15,7]
    ]
### 题解

```c++
// 使用双端队列
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        if (root == NULL) {
            return vector<vector<int>>();
        }
        deque<TreeNode *> deq;
        deq.push_front(root);
        bool leftOrRight = true;
        vector<vector<int>> res;
        while (!deq.empty()) {
            int size = deq.size();
            vector<int> temp;
            if (leftOrRight) {
                for (int i = 0; i < size; i++) {
                    TreeNode *ptr = deq.front();
                    deq.pop_front();
                    temp.push_back(ptr->val);
                    if (ptr->left != NULL) {
                        deq.push_back(ptr->left);
                    }
                    if (ptr->right != NULL) {
                        deq.push_back(ptr->right);
                    }
                }
            } else {
                for (int i = 0; i < size; i++) {
                    TreeNode *ptr = deq.back();
                    deq.pop_back();
                    temp.push_back(ptr->val);
                    if (ptr->right != NULL) {
                        deq.push_front(ptr->right);
                    }
                    if (ptr->left != NULL) {
                        deq.push_front(ptr->left);
                    }
                }
            }
            res.push_back(temp);
            leftOrRight = !leftOrRight;
        }
        return res;
    }
};
// 时间复杂度：O(N)
// 空间复杂度：O(N):最差情况下，即当树为满二叉树时，最多有 N/2N/2 个树节点 同时 在 deque 中，使用 O(N)O(N) 大小的额外空间。

// 使用两个栈
void PrintByZ(BtNode *ptr)
{
	if (ptr == NULL) {
		return;
	}
	stack<BtNode *> st1;
	stack<BtNode *> st2;
	bool leftOrRight = true;
	st1.push(ptr);
	while (!st1.empty() || !st2.empty()) {
		if (leftOrRight) {
			while (!st1.empty()) {
				BtNode * node = st1.top();
				st1.pop();
				cout << node->data << " ";
				if (node->leftChild != NULL) {
					st2.push(node->leftChild);
				}
				if (node->rightChild != NULL) {
					st2.push(node->rightChild);
				}
			}
		}
		else {
			while (!st2.empty()) {
				BtNode * node = st2.top();
				st2.pop();
				cout << node->data << " ";
				if (node->rightChild != NULL) {
					st1.push(node->rightChild);
				}
				if (node->leftChild != NULL) {
					st1.push(node->leftChild);
				}
			}
		}
		leftOrRight = !leftOrRight;
	}
}
```

## 二叉搜索树的后续遍历序列

### 题目

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 true，否则返回 false。假设输入的数组的任意两个数字都互不相同。

参考以下这颗二叉搜索树：

```c++
	 5
	/ \
   2   6
  / \
 1   3
示例 1：

输入: [1,6,3,2,5]
输出: false
示例 2：

输入: [1,3,2,6,5]
输出: true
```
### 题解

```c++
class Solution {
public:
    bool verifyPostorder(vector<int>& postorder) {
        if (postorder.empty()) {
            return true;
        }
        return recur(postorder, 0, postorder.size() - 1);
    }

    bool recur(vector<int>& postorder, int left, int right) {
        if (left >= right) {
            return true;
        }
        int p = left;
        while (postorder[p] < postorder[right]) {
            p++;
        }
        int m = p;
        while (postorder[p] > postorder[right]) {
            p++;
        }
        // p=j ： 判断 此树 是否正确。
		// recur(i, m - 1)： 判断 此树的左子树 是否正确。
		// recur(m, j - 1)： 判断 此树的右子树 是否正确。
        return p == right && recur(postorder, left, m - 1) && recur(postorder, m, right - 1);
    }
};
```

## 二叉树中和为某一个值的路径

### 题目

输入一棵二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶节点所经过的节点形成一条路径。

 

示例:
给定如下二叉树，以及目标和 sum = 22，

```c++
          5
         / \
        4   8
       /   / \
      11  13  4
     /  \    / \
    7    2  5   1
    
返回:
[
   [5,4,11,2],
   [5,8,4,5]
]
```
### 题解

```c++
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;
    vector<vector<int>> pathSum(TreeNode* root, int sum) {
        recur(root, sum);
        return res;
    }

    void recur(TreeNode *root, int sum) {
        if (root == NULL) {
            return;
        }
        path.push_back(root->val);
        sum -= root->val;
        // 如果是叶子结点并且sum为0，就添加这个path
        if (sum == 0 && root->left == NULL && root->right == NULL) {
            res.push_back(path);
        }
        // 接着遍历左节点
        // 右节点
        recur(root->left, sum);
        recur(root->right, sum);
        path.pop_back();
    }
};
```

## 二叉搜索树与双向链表

### 题目

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的循环双向链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。

### 题解

```c++

```

## 序列化二叉树

### 题目

请实现两个函数，分别用来序列化和反序列化二叉树。

示例: 

你可以将以下二叉树：

```c++
	1
   / \
  2   3
     / \
    4   5

序列化为 "[1,2,3,null,null,4,5]"
```
### 题解

```c++
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        string res;
        dfs(root, res);
        return res;
    }

    void dfs(TreeNode *root, string &res) {
        if (root == NULL) {
            res += "null ";
            return;
        }
        res += to_string(root->val) + ' ';
        dfs(root->left, res);
        dfs(root->right, res);
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        int index = 0;
        return dfs_deserialize(data, index);
    }

    TreeNode *dfs_deserialize(string &data, int &index) {
        if (index > data.size()) {
            return NULL;
        }
        // 如果字符为null，返回null
        if (data[index] == 'n') {
            index += 5;
            return NULL;
        }
        int val = 0, sign = 1;
        // 如果是负数，需要跳过负号
        if (data[index] == '-') {
            sign = -1;
            index++;
        }
        // 把字符转成数字
        while (data[index] != ' ') {
            val = val * 10 + data[index] - '0';
            index++;
        }
        val *= sign;
        index++;

        // 构建二叉树
        TreeNode *root = new TreeNode(val);
        root->left = dfs_deserialize(data, index);
        root->right = dfs_deserialize(data, index);
        return root;
    }
};
```

