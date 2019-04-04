# 047 Permutations II

> DATE: 2019.4.4 ~ 2019.4.5

## 题目描述：

> Given a collection of numbers that might contain duplicates, return all possible unique permutations.

Example:

```C++
Input: [1,1,2]
Output:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```

<br/>

**类别**：Backtracking

**难度**：Medium

[NowCoder](https://leetcode.com/problems/permutations-ii/)

<br/>

## 解题思路：

**题目翻译**：给定一个整形数组，这个数组中可能会包含重复的数字，要求我们返回的是这个数组不同的Permutations，也就是说每一种可能的permutation在最后的答案中只能出现一次。例子能清晰的告诉不同的地方。

**题目分析**： 对于这道题。也是要求permutation，大体上的解题思路和Permutations是相同的，但是不同点在哪里呢？ 不同点为：

1. 这个给定的数组中可能会含有相同的数字.
2. 最后答案不接受重复相同的答案组.

对于这两点要求，Permutations的解法是无法解决的，所以我们就要考虑怎样满足以上两个要求.   我们可以对整个input数组进行排序，在求解答案的时候，只要一个元素的permutation求出来了，在这个元素后面和这个元素相同的元素，我们完全都可以pass掉，其实这个方法在sum和combination里面已经是屡试不爽了。

**解题思路**：除了加上对于重复答案的处理外，剩下思路同Permutation一模一样。

**时间复杂度**：O(n!)。

<br/>

## 解题代码：

```C++
class Solution {
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        vector<vector<int>> permutations;
        if(nums.size() == 0)
            return permutations;
        vector<int> curr;
        vector<bool> isVisited(nums.size(), false);
        sort(nums.begin(),nums.end());
        DFS(permutations,curr,nums,isVisited);
        return permutations;
    }

    void DFS(vector<vector<int>>& ret, vector<int> curr, vector<int> nums, vector<bool> isVisited) {
        if(curr.size() == nums.size()) {
            ret.push_back(curr);
            return;
        }

        for(int i = 0; i < nums.size(); ++i) {
            if(isVisited[i] == false) {
                isVisited[i] = true;
                curr.push_back(nums[i]);
                DFS(ret,curr,nums,isVisited);
                isVisited[i] = false;
                curr.pop_back();
                while(i < nums.size()-1 && nums[i] == nums[i+1])
                    ++i;
            }
        }
    }
};
```

<br/>

论坛优质解答欣赏：

```C++
class Solution {
public:
    void recursion(vector<int> num, int i, int j, vector<vector<int> > &res) {
        if (i == j-1) {
            res.push_back(num);
            return;
        }
        for (int k = i; k < j; k++) {
            if (i != k && num[i] == num[k]) continue;
            swap(num[i], num[k]);
            recursion(num, i+1, j, res);
        }
    }
    vector<vector<int> > permuteUnique(vector<int> &num) {
        sort(num.begin(), num.end());
        vector<vector<int> >res;
        recursion(num, 0, num.size(), res);
        return res;
    }
};
```

<br/>
