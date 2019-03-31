# 078 Subsets

> DATE: 2019.3.30 ~ 2019.3.31

## 题目描述：

> Given a set of **distinct** integers, *nums*, return all possible subsets (the power set).
>
> **Note:** The solution set must not contain duplicate subsets.

Example:

```C++
Input: nums = [1,2,3]
Output:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```

<br/>

**类别**：Backtracking

**难度**：Medium

[NowCoder](https://leetcode.com/problems/subsets/)

<br/>

## 解题思路：

**题目翻译**：给定一组不同的整数集合，列出集合里面的所有子集合，同时要求子集合元素需要升序排列，不包含重复子集。

**解题思路**：首先我们需要对集合排序，对于一个n元素的集合，首先我们取第一个元素，加入子集合中，后面的n  - 1个元素可以认为是第一个元素的子节点，我们依次遍历，譬如遍历到第二个元素的时候，后续的n -  2个元素又是第二个元素的子节点，再依次遍历处理，直到最后一个元素，然后回溯，继续处理。处理完第一个元素之后，我们按照同样的方式处理第二个元素。

譬如上面的[1, 2, 3]，首先取出1，加入子集合，后面的2和3就是1的子节点，先取出2，把[1, 2]加入子集合，后面的3就是2的子节点，取出3，把[1, 2, 3]加入子集合。然后回溯，取出3，将[1, 3]加入子集合。

1处理完成之后，我们可以同样方式处理2，以及3。

<br/>

## 解题代码：

```C++
class Solution {
public:
    vector<vector<int> > res;
    vector<vector<int>> subsets(vector<int>& nums) {
        if(nums.empty()) {
            return res;
        }
        sort(nums.begin(), nums.end());
        //别忘了空集合
        res.push_back(vector<int>());
        vector<int> vec;
        generate(0, vec, nums);
        return res;
    }

    void generate(int start, vector<int>& vec, vector<int> &nums) {
        if(start == nums.size()) {
            return;
        }
        for(int i = start; i < nums.size(); i++) {
            vec.push_back(nums[i]);
            res.push_back(vec);
            generate(i + 1, vec, nums);
            vec.pop_back();
        }
    }
};
```

<br/>

论坛优质解答欣赏：

**Bit Manipulation**
To give all the possible subsets, we just need to exhaust all the possible combinations of the numbers. And each number has only two possibilities: either in or not in a subset. And this can be represented using a bit.

Using [1, 2, 3] as an example, 1 appears once in every two consecutive subsets, 2 appears twice in every four consecutive subsets, and 3 appears four times in every eight subsets (initially all subsets are empty):

[], [ ], [ ], [    ], [ ], [    ], [    ], [       ]
[], [1], [ ], [1   ], [ ], [1   ], [    ], [1      ]
[], [1], [2], [1, 2], [ ], [1   ], [2   ], [1, 2   ]
[], [1], [2], [1, 2], [3], [1, 3], [2, 3], [1, 2, 3]

```C++
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        int n = nums.size(), p = 1 << n;
        vector<vector<int>> subs(p);
        for (int i = 0; i < p; i++) {
            for (int j = 0; j < n; j++) {
                if ((i >> j) & 1) {
                    subs[i].push_back(nums[j]);
                }
            }
        }
        return subs;
    }
};
```

<br/>
