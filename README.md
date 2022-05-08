# Algorithm-labuladong
**这是 [labuladong 的算法小抄](https://labuladong.github.io/algo/) 的刷题记录。每一节选一道代表题目，方便快速复习。**


## 第零章、核心框架汇总



## 第一章、手把手刷数据结构

### 手把手刷链表算法



### 手把手刷数组算法

#### 前缀和：[304. 二维区域和检索 - 矩阵不可变](https://leetcode-cn.com/problems/range-sum-query-2d-immutable/)

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



#### 差分数组：[1094. 拼车](https://leetcode-cn.com/problems/car-pooling/)

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



#### 双指针-重复元素：[26. 删除有序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

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



#### 二维数组的花式遍历：[54. 螺旋矩阵](https://leetcode-cn.com/problems/spiral-matrix/)

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



#### 滑动窗口：[76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)

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

