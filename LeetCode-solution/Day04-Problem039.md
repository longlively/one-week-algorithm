# 039 Combination Sum

> DATE: 2019.4.2 ~ 2019.4.3

## 题目描述：

> Given a set of candidate numbers (candidates) (without duplicates) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.

> The same repeated number may be chosen from candidates unlimited number of times.

**Note**:
- All numbers (including target) will be positive integers.
- The solution set must not contain duplicate combinations.

Example 1:

```C++
Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]
```

Example 2:

```C++
Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```

<br/>

**类别**：Backtracking

**难度**：Medium

[NowCoder](https://leetcode.com/problems/combination-sum/)

<br/>

## 解题思路：

**题目翻译**：给一个数组candidates和一个目标值target，找出所有的满足条件的组合：使得组合里面的数字之和等于target，并且一些数字可以从candidates中重复选择。

**题目分析**：这道题的大体思路和combinations是相同的，不同的地方在于一个数字可以使用多次，这也造成了我们进行实现function的时候要注意的问题，也就是说，传入递归的参数不同于combinations。

**时间复杂度**：没什么好说的，和combinations的时间复杂度是相同的O(n!)。

<br/>

## 解题代码：

```C++
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int> &candidates, int target) {
        vector<vector<int>> ret;
        if(candidates.size() == 0 || target < 0)
            return ret;
        vector<int> curr;
        sort(candidates.begin(),candidates.end());  
        BackTracking(ret, curr, candidates, target, 0);
        return ret;
    }
    void BackTracking(vector<vector<int>>& ret, vector<int> curr, vector<int> candidates, int target, int level) {
        if(target == 0) {
            ret.push_back(curr);
            return;
        }
        else if(target < 0) 
            return;
        for(int i = level; i < candidates.size(); ++i) {
            target -= candidates[i];
            curr.push_back(candidates[i]);
            BackTracking(ret, curr, candidates, target, i);
            curr.pop_back();
            target += candidates[i];
        }
    }
};
```

<br/>

论坛优质解答欣赏：

```C++
class Solution {
public:
    vector<vector<int> > combinationSum(vector<int> &candidates, int target) {
        sort(candidates.begin(), candidates.end());
        vector<vector<int> > res;
        vector<int> combination;
        combinationSum(candidates, target, res, combination, 0);
        return res;
    }
private:
    void combinationSum(vector<int> &candidates, int target, vector<vector<int> > &res, vector<int> &combination, int begin) {
        if (!target) {
            res.push_back(combination);
            return;
        }
        for (int i = begin; i != candidates.size() && target >= candidates[i]; ++i) {
            combination.push_back(candidates[i]);
            combinationSum(candidates, target - candidates[i], res, combination, i);
            combination.pop_back();
        }
    }
};
```

<br/>
