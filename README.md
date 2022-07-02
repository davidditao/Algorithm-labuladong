**这是 [labuladong 的算法小抄](https://labuladong.github.io/algo/) 的刷题记录。每一节选一道代表题目，方便快速复习。**

# LeetCode上的 Debug 小技巧

补充一个在LeetCode网页上刷题的时候的 Debug 技巧。因为非会员在 LeetCode 上没有调试功能，所以我们只能 `print` 打印一些关键变量的值。

**但是如果遇到递归算法的时候，直接打印变量会十分混乱。** 这里提供一个十分好用的方法。

首先我们需要定义一个函数 `debug` 和一个全局变量 `_count`，然后在递归函数开始时执行`debug(_count++)` 在递归函数结束前执行`debug(--_count)`。然后在中间我们就可以正常打印我们需要的关键变量。`debug`函数如下：

```C++
// 调试
int _count = 0;
void debug(int count){
    // 打印 count 个缩进
    for(int i = 0; i < count; i++){
        printf("    ");
    }
    // 这句可以不写
    printf("[%d] ", count);
}
```

**我们以一个较为复杂的题目 [236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/) 为例**，递归版本的算法如下：

```C++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        return dfs(root, p, q);
    }
private:
    TreeNode* dfs(TreeNode *root, TreeNode *p, TreeNode *q){
        if(root == nullptr){
            return nullptr;
        }
        if(root == p || root == q){
            return root;
        }
        TreeNode *left = dfs(root->left, p, q);
        TreeNode *right = dfs(root->right, p, q);

        if(left != nullptr && right != nullptr){
            return root;
        } 
        return left == nullptr ? right : left;
    }
};
```

我们进行 debug 打印关键变量的信息：

```C++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        return dfs(root, p, q);
    }
private:
    // 调试
    int _count = 0;
    void debug(int count){
        // 打印 count 个缩进
        for(int i = 0; i < count; i++){
            printf("    ");
        }
        // 这句可以不写
        printf("[%d] ", count);
    }

    TreeNode* dfs(TreeNode *root, TreeNode *p, TreeNode *q){
        // 函数开始时调用debug，并打印当前节点的信息
        debug(_count++);
        cout<<"node: ";
        root == nullptr ? cout<<"nullptr"<<endl : cout<<root->val<<endl;

        if(root == nullptr){
            // 函数结束时调用debug，并打印返回值的信息
            debug(--_count);
            cout<<"retrun nullptr"<<endl;

            return nullptr;
        }
        if(root == p || root == q){
            // 函数结束时调用debug，并打印返回值的信息
            debug(--_count);
            cout<<"return "<<root->val<<endl;

            return root;
        }
        TreeNode *left = dfs(root->left, p, q);
        TreeNode *right = dfs(root->right, p, q);

        if(left != nullptr && right != nullptr){
            // 函数结束时调用debug，并打印返回值的信息
            debug(--_count);
            cout<<"return ";
            root == nullptr ? cout<<"nullptr"<<endl : cout<<root->val<<endl;

            return root;
        } 

        // 函数结束时调用debug，并打印返回值的信息
        debug(--_count);
        cout<<"return ";
        TreeNode *node =  left == nullptr ? right : left;
        node == nullptr ? cout<<"nullptr"<<endl : cout<<node->val<<endl;

        return left == nullptr ? right : left;
    }
};
```

打印结果如下：

```C++
/**
* 输入：
* [3,5,1,6,2,0,8,null,null,7,4]
* 5
* 8
*/
[0] node: 3
    [1] node: 5
    [1] return 5
    [1] node: 1
        [2] node: 0
            [3] node: nullptr
            [3] retrun nullptr
            [3] node: nullptr
            [3] retrun nullptr
        [2] return nullptr
        [2] node: 8
        [2] return 8
    [1] return 8
[0] return 3
```

可以看到这种方法可以很直观地看出递归的过程，我们很容易就能分析出这个算法的执行流程。



# 第零章、核心框架汇总



# 第一章、手把手刷数据结构

## 1. 手把手刷链表算法

### 🎁合并链表：[23. 合并K个升序链表](https://leetcode.cn/problems/merge-k-sorted-lists/)

[剑指 Offer II 078. 合并排序链表](https://leetcode.cn/problems/vvXgSW/)

自己写的题解：[合并K个升序链表：优先队列](https://leetcode.cn/problems/merge-k-sorted-lists/solution/by-pedantic-mcleanbpp-lkxs/)



### 🎁环形链表：[142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)

[剑指 Offer II 022. 链表中环的入口节点](https://leetcode.cn/problems/c32eOV/)

自己写的题解：[环形链表：双指针](https://leetcode.cn/problems/linked-list-cycle-ii/solution/huan-xing-lian-biao-by-pedantic-mcleanbp-ljwq/)



### 🎁相交链表：[160. 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/)

[剑指 Offer II 023. 两个链表的第一个重合节点](https://leetcode.cn/problems/3u1WK4/)

自己写的题解：[相交链表：双指针](https://leetcode.cn/problems/intersection-of-two-linked-lists/solution/xiang-jiao-lian-biao-shuang-zhi-zhen-by-8it64/)



### 🎁反转链表：[92. 反转链表 II](https://leetcode.cn/problems/reverse-linked-list-ii/)

自己写的题解：[反转链表：递归 - 反转链表 II ](https://leetcode.cn/problems/reverse-linked-list-ii/solution/fan-zhuan-lian-biao-by-pedantic-mcleanbp-x4bl/)



### 🎁K个一组反转链表：[25. K 个一组翻转链表](https://leetcode.cn/problems/reverse-nodes-in-k-group/)

自己写的题解：[K个一组反转链表](https://leetcode.cn/problems/reverse-nodes-in-k-group/solution/fan-zhuan-by-pedantic-mcleanbpp-ryr8/)



### 🎁回文链表：[234. 回文链表](https://leetcode.cn/problems/palindrome-linked-list/)

自己写的题解：[回文链表：反转链表 + 链表中间结点](https://leetcode.cn/problems/palindrome-linked-list/solution/hui-wen-lian-biao-fan-zhuan-lian-biao-by-irf3/)



## 2. 手把手刷数组算法

### 🎁前缀和：[304. 二维区域和检索 - 矩阵不可变](https://leetcode-cn.com/problems/range-sum-query-2d-immutable/)

**难度简单**

```C++
class NumMatrix {
public:
    NumMatrix(vector<vector<int>>& matrix) {
        arr.resize(matrix.size() + 1, vector<int>(matrix[0].size()+1, 0)); 
        for(int i = 1; i < arr.size(); i++){
            for(int j = 1; j < arr[0].size(); j++){
                arr[i][j] = matrix[i-1][j-1] + arr[i-1][j] + arr[i][j-1] - arr[i-1][j-1];
            }
        }
    }
    
    int sumRegion(int row1, int col1, int row2, int col2) {
        return arr[row2+1][col2+1] - arr[row2+1][col1] - arr[row1][col2+1] + arr[row1][col1];
    }
private:
    vector<vector<int>> arr;
};

/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix* obj = new NumMatrix(matrix);
 * int param_1 = obj->sumRegion(row1,col1,row2,col2);
 */
```



### 🎁差分数组：[1094. 拼车](https://leetcode-cn.com/problems/car-pooling/)

**难度==中等==**

```C++
class Difference{
private:
    vector<int> diff;
public:
    Difference(int n){
        diff.resize(n, 0);
    }

    void increment(int i, int j, int val){
        diff[i] += val;
        if(j + 1 < diff.size()){
            diff[j + 1] -= val;
        }
    }

    vector<int> result(){
        vector<int> res(diff.size(), 0);
        res[0] = diff[0];
        for(int i = 1; i < res.size(); i++){
            res[i] = res[i-1] + diff[i];
        }
        return res;
    }

};

class Solution {
public:
    bool carPooling(vector<vector<int>>& trips, int capacity) {
        Difference diff(1001);
        for(int i = 0; i < trips.size(); i++){
            int val = trips[i][0];
            int start = trips[i][1];
            int end = trips[i][2]-1;

            diff.increment(start, end, val);
        }
        vector<int> res = diff.result();
        for(int i = 0; i < res.size(); i++){
            if(res[i] > capacity){
                return false;
            }
        }
        return true;
    }
};
```



### 🎁双指针-重复元素：[26. 删除有序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

**难度简单**

```C++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int slow = 0, fast = 0;
        while(fast < nums.size()){
            if(/* fast指针找到满足条件的元素 */){
                // 将元素放在slow指针处
                slow++;
            }
            fast++;
        }
        return slow+1;
    }
}
```



### 🎁二维数组的花式遍历：[54. 螺旋矩阵](https://leetcode-cn.com/problems/spiral-matrix/)

**难度==中等==**

```C++
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        int upper = 0, lower = m - 1, left = 0, right = n - 1;
        vector<int> res;
        while(res.size() < m*n){
            if(upper <= lower){
                for(int j = left; j <= right; j++){
                    res.push_back(matrix[upper][j]);
                }
                upper++;
            }

            if(left <= right){
                for(int i = upper; i <= lower ; i++){
                    res.push_back(matrix[i][right]);
                }
                right--;
            }

            if(upper <= lower){
                for(int j = right; j >= left; j--){
                    res.push_back(matrix[lower][j]);
                }
                lower--;
            }

            if(left <= right){
                for(int i = lower; i >= upper; i--){
                    res.push_back(matrix[i][left]);
                }
                left++;
            }
        }
        return res;
    }
};
```



### 🎁滑动窗口：[76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)

**难度==困难==**

```C++
class Solution {
public:
    string minWindow(string s, string t) {
        unordered_map<char, int> need, window;

        for(char c : t){
            need[c]++;
        }
        
        int left = 0, right = 0;
        int valid = 0;
        int start = 0, len = INT_MAX;
        while(right < s.size()){
            char c = s[right];
            right++;
            if(need.count(c)){
                window[c]++;
                if(window[c] == need[c]){
                    valid++;
                }
            }

            while(valid == need.size()){
                if(right - left < len){
                    start = left;
                    len = right - left;
                }

                char d = s[left];
                left++;

                if(need.count(d)){
                    if(window[d] == need[d]){
                        valid--;
                    }
                    window[d]--;
                }
            }
        } 

        return len == INT_MAX ? "" : s.substr(start, len);
    }
};
```



### 🎁二分查找：[704. 二分查找](https://leetcode.cn/problems/binary-search/)

自己写的题解：[二分查找模版总结](https://leetcode.cn/problems/binary-search/solution/er-fen-cha-zhao-by-pedantic-mcleanbpp-w419/)



### 🎁带权重的随机选择算法：[528. 按权重随机选择](https://leetcode.cn/problems/random-pick-with-weight/)

**难度==中等==**

```C++
class Solution {
public:
    Solution(vector<int>& w) {
        presum.resize(w.size() + 1, 0);
        for(int i = 1; i < presum.size(); i++){
            presum[i] = presum[i - 1] + w[i - 1];
        }
    }
    
    int pickIndex() {
        int n = presum.size();
        int target = rand() % presum[n - 1] + 1; 
        return left_bound(presum, target) - 1;    
    }
private:
    vector<int> presum;
    int left_bound(vector<int> &presum, int target){
        int left = 0, right = presum.size() - 1;
        while(left <= right){
            int mid = (right - left)/2 + left;
            if(presum[mid] < target){
                left = mid + 1;
            } else if(presum[mid] > target){
                right = mid - 1;
            } else {
                right = mid - 1;
            }
        }
        return left;
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(w);
 * int param_1 = obj->pickIndex();
 */
```



### 🎁二分搜索应用：[875. 爱吃香蕉的珂珂](https://leetcode.cn/problems/koko-eating-bananas/)

**难度==中等==**

```c++
class Solution {
public:
    int minEatingSpeed(vector<int>& piles, int h) {
        int left = 1, right = *max_element(piles.begin(), piles.end());
        while(left <= right){
            int mid = (right - left)/2 + left;
            if(finishtime(piles, mid) > h){
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return left;
    }

private:
    int finishtime(vector<int> &piles, int speed){
        int sum = 0;
        for(int i = 0; i < piles.size(); i++){
            sum += (piles[i] + speed - 1) / speed;
        }
        return sum;
    }
};
```



### 🎁田忌赛马：[870. 优势洗牌](https://leetcode.cn/problems/advantage-shuffle/)

**难度==中等==**

```C++
class Solution {
public:
    vector<int> advantageCount(vector<int>& nums1, vector<int>& nums2) {
        sort(nums1.begin(), nums1.end());
        priority_queue<pair<int, int>, vector<pair<int, int>>, cmp> q;

        for(int i = 0; i < nums2.size(); i++){
            q.push(make_pair(nums2[i], i));
        }
        
        int n = nums1.size();
        int left = 0, right = n - 1; 
        vector<int> res(n, 0);
        while(!q.empty()){
            int maxval = q.top().first;
            int index = q.top().second;
            q.pop();
            if(maxval < nums1[right]){
                res[index] = nums1[right];
                right--;
            } else {
                res[index] = nums1[left];
                left++;
            }
        }
        return res;
    }
private:
    struct cmp{
        bool operator()(pair<int, int> p1, pair<int, int> p2){
            return p1.first < p2.first;
        }
    };
};
```



### 🎁常数时间删除/查找数组中的任意元素：[380. O(1) 时间插入、删除和获取随机元素](https://leetcode.cn/problems/insert-delete-getrandom-o1/) + [710. 黑名单中的随机数](https://leetcode.cn/problems/random-pick-with-blacklist/)

自己写的题解：[O(1) 时间插入、删除和获取随机元素：变长数组+哈希表](https://leetcode.cn/problems/insert-delete-getrandom-o1/solution/380-o1-shi-jian-cha-ru-shan-chu-he-huo-q-8cik/) + [黑名单中的随机数：哈希表 - 黑名单中的随机数 - 力扣（LeetCode）](https://leetcode.cn/problems/random-pick-with-blacklist/solution/hei-ming-dan-zhong-de-sui-ji-shu-by-peda-lmpe/)



### 🎁单调栈：[316. 去除重复字母](https://leetcode.cn/problems/remove-duplicate-letters/)

[由浅入深，单调栈思路去除重复字符 - 去除重复字母 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-duplicate-letters/solution/you-qian-ru-shen-dan-diao-zhan-si-lu-qu-chu-zhong-/)

改进：可以直接将 res 做为单调栈，不用另开辟空间

```C++
class Solution {
public:
    string removeDuplicateLetters(string s) {
        vector<bool> inStack(256, false);
        vector<int> count(256, 0);
        string res;
        
        for(char c : s){
            count[c]++;
        }

        for(char c : s){
            count[c]--;
            if(inStack[c]){
                continue;
            }            
            while(!res.empty() && res[res.size() - 1] > c){
                if(count[res[res.size() - 1]] == 0){
                    break;
                }
                inStack[res[res.size() - 1]] = false;
                res.pop_back();
            }
            res += c;
            inStack[c] = true;
        }

        return res;
    }
};
```



## 3. 手把手刷二叉树算法

### 🎁后序遍历解决二叉树深度问题：[543. 二叉树的直径](https://leetcode.cn/problems/diameter-of-binary-tree/)

自己写的题解：[二叉树的直径：后序遍历解决二叉树深度问题 ](https://leetcode.cn/problems/diameter-of-binary-tree/solution/er-cha-shu-de-zhi-jing-by-pedantic-mclea-ckf8/)



### 🎁后序遍历应用：[114. 二叉树展开为链表](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/)

自己写的题解：[二叉树展开为链表：后序遍历](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/solution/er-y-by-pedantic-mcleanbpp-za58/)



### 🎁前序遍历应用：[116. 填充每个节点的下一个右侧节点指针](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/)

自己写的题解：[填充每个节点的下一个右侧节点指针：DFS](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/solution/tian-chong-mei-ge-jie-dian-de-xia-yi-ge-dkm3r/)



### 🎁构造二叉树：[105. 从前序与中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

自己写的题解：[二叉树的构造问题解题方法 ](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/solution/gou-zao-er-cha-shu-by-pedantic-mcleanbpp-7ij7/)



### 🎁二叉树的序列化与反序列化：[297. 二叉树的序列化与反序列化](https://leetcode.cn/problems/serialize-and-deserialize-binary-tree/)

自己写的题解：[二叉树的序列化与反序列化：DFS+BFS - 二叉树的序列化与反序列化 - 力扣（LeetCode）](https://leetcode.cn/problems/serialize-and-deserialize-binary-tree/solution/er-cha-shu-de-xu-lie-hua-yu-fan-xu-lie-h-htxc/)



### 🎁后序遍历+序列化：[652. 寻找重复的子树](https://leetcode.cn/problems/find-duplicate-subtrees/)

自己写的题解：[寻找重复的子树：后序遍历+序列化二叉树 - 寻找重复的子树 - 力扣（LeetCode）](https://leetcode.cn/problems/find-duplicate-subtrees/solution/xun-zhao-zhong-fu-de-zi-shu-hou-xuh-by-p-oeaw/)



### 🎁排序算法：[912. 排序数组](https://leetcode.cn/problems/sort-an-array/)

自己写的题解：[排序算法](https://leetcode.cn/problems/sort-an-array/solution/pai-xu-suan-fa-by-pedantic-mcleanbpp-jsin/)



### 🎁归并排序解决逆序对问题：[剑指 Offer 51. 数组中的逆序对](https://leetcode.cn/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)

自己写的题解：[数组中的逆序对：归并排序](https://leetcode.cn/problems/shu-zu-zhong-de-ni-xu-dui-lcof/solution/shu-zu-zhong-de-by-pedantic-mcleanbpp-600u/)



### 🎁二叉搜索树的性质：[538. 把二叉搜索树转换为累加树](https://leetcode.cn/problems/convert-bst-to-greater-tree/)

自己写的题解：[把二叉搜索树转换为累加树：中序遍历 ](https://leetcode.cn/problems/convert-bst-to-greater-tree/solution/ba-er-cha-sou-suo-shu-zhuan-huan-wei-lei-vl8k/)



### 🎁二叉搜索树的增删改查操作：[700. 二叉搜索树中的搜索](https://leetcode.cn/problems/search-in-a-binary-search-tree/)

自己写的题解：[二叉搜索树的增删改查操作](https://leetcode.cn/problems/search-in-a-binary-search-tree/solution/er-cha-sou-suo-shu-de-by-pedantic-mclean-omuw/)



### 🎁构造二叉搜索树：[96. 不同的二叉搜索树](https://leetcode.cn/problems/unique-binary-search-trees/)

自己写的题解：[不同的二叉搜索树：暴力搜索 + 优化 = 动态规划](https://leetcode.cn/problems/unique-binary-search-trees/solution/by-pedantic-mcleanbpp-x4f8/)



### 🎁快速选择算法：[215. 数组中的第K个最大元素](https://leetcode.cn/problems/kth-largest-element-in-an-array/)

自己写的题解：[数组中的第k个最大元素：优先队列 + 快速选择算法](https://leetcode.cn/problems/kth-largest-element-in-an-array/solution/shu-zu-zhong-by-pedantic-mcleanbpp-m2us/)



### 🎁二叉树的最近公共节点：[236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)

自己写的题解：[二叉树的最近公共祖先大集合：后序遍历](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/solution/er-cha-shu-de-gong-gong-zu-xian-by-pedan-00fs/)



### 🎁完全二叉树的节点个数：[222. 完全二叉树的节点个数](https://leetcode.cn/problems/count-complete-tree-nodes/)

自己写的题解：[完全二叉树的节点个数：普通二叉树 + 满二叉树](https://leetcode.cn/problems/count-complete-tree-nodes/solution/by-pedantic-mcleanbpp-ebl6/)



## 4. 手把手刷图算法

### 🎁图论基础：[797. 所有可能的路径](https://leetcode.cn/problems/all-paths-from-source-to-target/)

自己写的题解：[797. 所有可能的路径：图的回溯](https://leetcode.cn/problems/all-paths-from-source-to-target/solution/by-pedantic-mcleanbpp-9xu5/)



### 🎁二分图：[785. 判断二分图](https://leetcode.cn/problems/is-graph-bipartite/)

自己写的题解：[判断二分图：DFS](https://leetcode.cn/problems/is-graph-bipartite/)



### 🎁判断图是否有环：[207. 课程表](https://leetcode.cn/problems/course-schedule/)

自己写的题解：[判断图是否有环：图的回溯](https://leetcode.cn/problems/course-schedule/solution/by-pedantic-mcleanbpp-qyo6/)



### 🎁拓扑排序：[210. 课程表 II](https://leetcode.cn/problems/course-schedule-ii/)

自己写的题解：[图的拓扑排序：判断图是否有环+图的后序遍历](https://leetcode.cn/problems/course-schedule-ii/solution/by-pedantic-mcleanbpp-b6ke/)



### 🎁岛屿问题：[130. 被围绕的区域](https://leetcode.cn/problems/surrounded-regions/)

自己写的题解：[被围绕的区域：DFS + 并查集 ](https://leetcode.cn/problems/surrounded-regions/solution/bei-wei-rao-de-qu-yu-by-pedantic-mcleanb-wzmr/)



### 🎁并查集：[130. 被围绕的区域 ](https://leetcode.cn/problems/surrounded-regions/)

自己写的题解：[被围绕的区域：DFS + 并查集 ](https://leetcode.cn/problems/surrounded-regions/solution/bei-wei-rao-de-qu-yu-by-pedantic-mcleanb-wzmr/)



### 🎁最小生成树：[1584. 连接所有点的最小费用](https://leetcode.cn/problems/min-cost-to-connect-all-points/)

自己写的题解：[最小生成树：Kruskal + Prim - 连接所有点的最小费用 - 力扣（LeetCode）](https://leetcode.cn/problems/min-cost-to-connect-all-points/solution/lian-jie-suo-you-dian-de-zui-xiao-fei-yo-3o3u/)



### 🎁最短路径：





## 5. 手把手设计数据结构

### 🎁单调栈： [496. 下一个更大元素 I](https://leetcode.cn/problems/next-greater-element-i/)

**难度简单**

```C++
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        stack<int> stk;
        vector<int> nge(nums2.size(), 0);
        unordered_map<int, int> map;
        for(int i = 0; i < nums2.size(); i++){
            map[nums2[i]] = i; 
        }
        for(int i = nums2.size() - 1; i >= 0; i--){
            while(!stk.empty() && stk.top() <= nums2[i]){
                stk.pop();
            }
            nge[i] = stk.empty() ? -1 : stk.top();
            stk.push(nums2[i]);
        }
        vector<int> res;
        for(int n : nums1){
            int i = map[n];
            res.push_back(nge[i]);
        }

        return res;
    }
};
```



### 🎁LRU 算法：[146. LRU 缓存](https://leetcode.cn/problems/lru-cache/)

自己写的题解：[LRU缓存：哈希表+双向链表](https://leetcode.cn/problems/lru-cache/solution/l-by-pedantic-mcleanbpp-wnjx/)



### 🎁单调队列结构解决滑动窗口问题：[239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)

自己写的题解：[滑动窗口最大值：堆（优先队列）、单调队列 - 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/solution/hua-dong-chuang-kou-zui-da-zhi-by-pedant-ljr5/)



### 🎁设计朋友圈时间线功能：[355. 设计推特](https://leetcode.cn/problems/design-twitter/)

自己写的题解：[设计推特：哈希表+链表+优先队列 ](https://leetcode.cn/problems/design-twitter/solution/she-ji-tui-te-by-pedantic-mcleanbpp-6gn8/)



# 第二章、手把手刷动态规划

## 1. 动态规划基本技巧

### 🎁动态规划核心原理：[322. 零钱兑换](https://leetcode.cn/problems/coin-change/)

自己写的题解：[零钱兑换：动态规划 ](https://leetcode.cn/problems/coin-change/solution/dstd-by-pedantic-mcleanbpp-4cil/)



### 🎁动态规划设计方法：[300. 最长递增子序列](https://leetcode.cn/problems/longest-increasing-subsequence/)

自己写的题解：[最长递增子序列：动态规划+二分查找 ](https://leetcode.cn/problems/longest-increasing-subsequence/solution/zui-chang-di-by-pedantic-mcleanbpp-q3zk/)



### 🎁base case 的初始值如何设置：[931. 下降路径最小和](https://leetcode.cn/problems/minimum-falling-path-sum/)

自己写的题解：[下降路径最小和：动态规划 - 下降路径最小和 - 力扣（LeetCode）](https://leetcode.cn/problems/minimum-falling-path-sum/solution/xia-jiang-lu-jing-zui-xiao-he-dong-tai-g-nv58/)



## 2. 子序列类型问题

### 🎁编辑距离：[72. 编辑距离](https://leetcode.cn/problems/edit-distance/)

自己写的题解：[编辑距离：动态规划](https://leetcode.cn/problems/edit-distance/solution/bian-ji-ju-chi-by-pedantic-mcleanbpp-7v9e/)



## 3. 背包问题

### 🎁0-1背包问题：

**问题描述：** 给你一个可装载重量为 `W` 的背包和 `N` 个物品，每个物品有重量和价值两个属性。其中第 `i` 个物品的重量为 `wt[i]`，价值为 `val[i]`，现在让你用这个背包装物品，最多能装的价值是多少？

```C++
int knapsack(int w, int n, vector<int> &wt, vector<int> &val) {
    vector<vector<int>> dp(n + 1, vector<int>(w + 1, 0));
//         for(int i = 0; i < dp.size(); i++){
//             dp[i][0] = 0;
//         }
    for(int i = 1; i < dp.size(); i++){
        for(int j = 1; j < dp[0].size(); j++){
            if(j - wt[i-1][0] < 0){
                dp[i][j] = dp[i - 1][j];
            } else {
                dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - wt[i-1][0]] + val[i-1][1]);
            }
        }
    }
    return dp[n][w];
}
```



### 🎁子集背包问题：[416. 分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/)

自己写的题解：[分割等和的子集：0-1 背包问题 ](https://leetcode.cn/problems/partition-equal-subset-sum/solution/0-1-by-pedantic-mcleanbpp-ub8q/)



### 🎁完全背包问题：[518. 零钱兑换 II](https://leetcode.cn/problems/coin-change-2/)



### 🎁动态规划与回溯：[494. 目标和](https://leetcode.cn/problems/target-sum/)



## 4. 用动态规划玩游戏

### 🎁最小路径和：[64. 最小路径和](https://leetcode.cn/problems/minimum-path-sum/)

自己写的题解：[最小路径和：从记忆化递归到动态规划](https://leetcode.cn/problems/minimum-path-sum/solution/zui-xiao-lu-jing-he-by-pedantic-mcleanbp-y1vi/)



### 🎁魔塔游戏：[174. 地下城游戏](https://leetcode.cn/problems/dungeon-game/)

自己写的题解：[地下城游戏：DFS＋后序遍历+动态规划思想](https://leetcode.cn/problems/dungeon-game/solution/c-by-pedantic-mcleanbpp-hwv5/)



### 🎁正则表达式匹配：[10. 正则表达式匹配](https://leetcode.cn/problems/regular-expression-matching/)

自己写的题解：[正则表达式匹配：递归 + 记忆化 = 动态规划](https://leetcode.cn/problems/regular-expression-matching/solution/zheng-ze-biao-da-shi-by-pedantic-mcleanb-suav/)



### 🎁打家劫舍问题：[198. 打家劫舍](https://leetcode.cn/problems/house-robber/)

自己写的题解：[打家劫舍：经典动态规划问题 ](https://leetcode.cn/problems/house-robber/solution/da-jia-jp-by-pedantic-mcleanbpp-i14r/)



### 🎁股票问题：[121. 买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)

自己写的题解：[状态机解决所有股票问题！！！ - 买卖股票的最佳时机 - 力扣（LeetCode）](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/solution/gu-piao-wen-ti-by-pedantic-mcleanbpp-pta9/)



## 5. 贪心类型问题

### 🎁区间调度问题：[435. 无重叠区间](https://leetcode.cn/problems/non-overlapping-intervals/)

自己写的题解：[无重叠区间：贪心算法 ](https://leetcode.cn/problems/non-overlapping-intervals/solution/wu-zhong-die-qu-jian-tan-xin-suan-fa-by-95odm/)



# 第三章、必知必会算法技巧

## 1. 暴力搜索算法

### 🎁回溯算法：[51. N 皇后](https://leetcode.cn/problems/n-queens/)

自己写的题解：[N皇后：回溯](https://leetcode.cn/problems/n-queens/solution/nhuang-hou-hui-su-by-pedantic-mcleanbpp-dy29/)



### 🎁回溯算法解决排列组合和子集问题：[46. 全排列](https://leetcode.cn/problems/permutations/)

自己写的题解：[回溯问题大合集：子集、组合、排列 ](https://leetcode.cn/problems/permutations/solution/hui-su-wen-ti-by-pedantic-mcleanbpp-qal3/)



### 🎁回溯算法解决集合划分问题：[698. 划分为k个相等的子集 - 力扣（LeetCode）](https://leetcode.cn/problems/partition-to-k-equal-sum-subsets/)

自己写的题解：[集合划分问题：回溯 - 划分为k个相等的子集](https://leetcode.cn/problems/partition-to-k-equal-sum-subsets/solution/hua-fen-zi-ji-wen-ti-by-pedantic-mcleanb-vi1h/)



### 🎁BFS 算法：[752. 打开转盘锁](https://leetcode.cn/problems/open-the-lock/)

自己写的题解：[最短路径：广度优先搜索 - 打开转盘锁](https://leetcode.cn/problems/open-the-lock/solution/by-pedantic-mcleanbpp-e5lb/)



## 2. 经典面试题

### 🎁分治算法：为运算符表达式设计优先级

自己写的题解：[为运算符表达式设计优先级：分治算法 - 为运算表达式设计优先级](https://leetcode.cn/problems/different-ways-to-add-parentheses/solution/wei-yun-suan-fu-biao-da-shi-by-pedantic-icfl5/)



### 🎁接雨水：[42. 接雨水](https://leetcode.cn/problems/trapping-rain-water/)

自己写的题解：[接雨水：动态规划+双指针+单调栈](https://leetcode.cn/problems/trapping-rain-water/solution/jie-yu-shui-by-pedantic-mcleanbpp-jpqm/)

















