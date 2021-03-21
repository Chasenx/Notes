命名规则 (谷歌代码规范)：
-------
- 文件名小写，可以加入下划线分开 `hello_test.cpp`
- 变量名全部小写，可以加入下划线 `teble_name`
- 类和结构体命名使用 “驼峰变量名”， `UrlTableTester`
- 函数命名和类类似，`StartRpc()`
- 常量命名，用个小 `k` 开头，`kDaysInAWeek`

动态规划问题：
----------
- 核心问题是一个选与不选的问题
- 一般是处理成递归的逻辑
- 可以使用空间换时间的方法来减小时间复杂度，计算 dp 数组


矩形和圆重叠问题：
-------
- 利用向量来判断 [大神的解析](https://www.zhihu.com/question/24251545 "跳转知乎链接")
- 实现的C++代码
```c++
bool checkOverlap(int radius, int x_center, int y_center, int x1, int y1, int x2, int y2) {
    // (x1, y1)是矩形的左下角坐标，(x2, y2)是矩形的右上角坐标
    
    // c 是矩形的中心坐标
    double c_x = (x2 + x1) / 2.0;
    double c_y = (y2 + y1) / 2.0;
    
    // v = p - c 
    // v为矩形中心指向圆心的向量,同时用绝对值把圆映射到第一象限
    double v_x = abs(x_center - c_x);
    double v_y = abs(y_center - c_y);
    
    // h 是矩形中心指向右上角的向量
    double h_x = x2 - c_x;
    double h_y = y2 - c_y;

    // u = v - h
    // u 用来判断相交
    double u_x = max(0, v_x - h_x);
    double u_y = max(0, v_y - h_y);
    
    return u_x * u_x + u_y * u_y <= radius * radius;
    }
```

单调栈的应用：
----------
- 求循环数组中每个元素的下一个更大元素，利用自栈底到栈顶单调递减的栈来实现
- 栈空或者元素比栈顶元素小的时候，入栈
- 元素比栈顶元素大的时候，逐个出栈，并且标记数组的最大值为此元素，直到比栈顶元素小
- 这个问题有点像汉诺塔问题，总是保持
```c++
vector<int> nextGreaterElements(vector<int>& nums) {
    int n = nums.size();
    stack<int> s;
    vector<int> res(n, -1);

    for (int i = 0; i < 2 * n; i++) {
        while (!s.empty() && nums[s.top()] < nums[i % n]) {
            res[s.top()] = nums[i % n];
            s.pop();
        }
        s.push(i % n);
    }
    return res;
    }
```

链表归并排序：
-----------
- 使用merge函数合并两个有序链表
- 使用cut函数将链表切成两段，并返回后面一段的指针
```c++
ListNode* sortList(ListNode* head) {
    ListNode* dummy_head = new ListNode(0);
    dummy_head->next = head;
    ListNode* p = head;
    // 计算链表长度
    int length = 0;
    while (p != nullptr) {
        length++;
        p = p->next;
    }

    for (int size = 1; size < length; size <<= 1) {
        ListNode* cur = dummy_head->next;
        ListNode* tail = dummy_head;

        while (cur != nullptr) {
            ListNode* left = cur;
            ListNode* right = cut(left, size);
            cur = cut(right, size);

            tail->next = merge(left, right);
            while (tail->next != nullptr) {
                tail = tail->next;
            }
        }
    }
    return dummy_head->next;
}

ListNode* cut(ListNode* head, int n) {
    ListNode* p = head;
    while(--n && p) {
        p = p->next;
    }
    if (p == nullptr) {
        return p;
    }
    ListNode* next = p->next;
    p->next = nullptr;
    return next;
}

ListNode* merge(ListNode* l1, ListNode* l2) {
    ListNode* dummy_head = new ListNode;
    ListNode* p = dummy_head;
    while (l1 && l2) {
        if (l1->val < l2->val) {
            p->next = l1;
            p = l1;
            l1 = l1->next;
        } else {
            p->next = l2;
            p = l2;
            l2 = l2->next;
        }
    }
    p->next = l1 ? l1 : l2;
    return dummy_head->next;
}
```

将x减到0的最小操作数：
-----------------
- [1, 1, 4, 2, 3] 每次从数组的左边或者右边删除一个元素，使删除元素之和为x，求出最少的元素个数
- 问题转化为求数组的一个子数组，使得改子数组之和为 sum - x，求该最大子数组
- 利用移动窗口法，用两个指针 left和right 来定位子串
```c++
int minOperations(vector<int>& nums, int x) {
    // 问题转换为求数值为 sum - x 的最大子串
    int length = nums.size();
    int sum = 0;
    for (auto n : nums) {
        sum += n;
    }
    int target = sum - x;
    if (target < 0) {
        return -1;
    }
    int left = 0; 
    int right = 0;
    int max_step = -1; // 记录最小的步数
    while (right < length) {
        target -= nums[right++];
        while (target < 0) {
            target +=nums[left++];
        }
        if (target == 0) {
            max_step = max(max_step, right - left);
        }
    }
    return max_step == -1 ? -1 : length - max_step;
}
```

利用BFS对树进行层次遍历：
--------------------
- 树的层次遍历需要使用到一个队列 queue
- 当层次遍历需要记录每一层的元素个数的时候，在每一层开始前先记录下这一层的节点数量
```c++
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> res;
    if (root == nullptr) {
        return res;
    }
    queue<TreeNode*> q;
    q.push(root);
    int level = 0;  // level 用来记录所在层数

    while (!q.empty()) {
        int n = q.size();
        vector<int> temp;
        res.push_back(temp);

        for (int i = 0; i < n; i++) {
            TreeNode* p = q.front();
            q.pop();
            res[level].push_back(p->val);
            if (p->left != nullptr) {
                q.push(p->left);
            }
            if (p->right != nullptr) {
                q.push(p->right);
            }
        }
        level++;
    }
    return res;
}
```