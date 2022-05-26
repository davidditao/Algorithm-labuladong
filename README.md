**这是 [labuladong 的算法小抄](https://labuladong.github.io/algo/) 的刷题记录。每一节选一道代表题目，方便快速复习。**



# 第零章、核心框架汇总



# 第一章、手把手刷数据结构

## 1. 手把手刷链表算法

### 🎁合并链表：[23. 合并K个升序链表](https://leetcode.cn/problems/merge-k-sorted-lists/)

自己写的题解：[合并K个升序链表：优先队列](https://leetcode.cn/problems/merge-k-sorted-lists/solution/by-pedantic-mcleanbpp-lkxs/)



### 🎁环形链表：[142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)

自己写的题解：[环形链表：双指针](https://leetcode.cn/problems/linked-list-cycle-ii/solution/huan-xing-lian-biao-by-pedantic-mcleanbp-ljwq/)



### 🎁相交链表：[160. 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/)

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



## 3. 手把手刷图算法

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



## 4. 手把手设计数据结构

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



## 3. 用动态规划玩游戏

### 🎁魔塔游戏：[174. 地下城游戏](https://leetcode.cn/problems/dungeon-game/)

自己写的题解：[地下城游戏：DFS＋后序遍历+动态规划思想](https://leetcode.cn/problems/dungeon-game/solution/c-by-pedantic-mcleanbpp-hwv5/)



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









# 待做：

[常数时间删除/查找数组中的任意元素 :: labuladong的算法小抄](https://labuladong.github.io/algo/2/18/30/)

[单调队列结构解决滑动窗口问题 :: labuladong的算法小抄](https://labuladong.github.io/algo/2/21/61/)

[东哥带你刷二叉树（纲领篇） :: labuladong的算法小抄](https://labuladong.github.io/algo/2/19/33/)

[东哥带你刷二叉树（思路篇） :: labuladong的算法小抄](https://labuladong.github.io/algo/2/19/34/)

[东哥带你刷二叉搜索树（基操篇） :: labuladong的算法小抄](https://labuladong.github.io/algo/2/19/40/)

























