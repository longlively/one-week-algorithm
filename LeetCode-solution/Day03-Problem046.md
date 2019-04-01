# 046 Permutations

> DATE: 2019.4.1 ~ 2019.4.2

## 题目描述：

> Given a collection of **distinct** integers, return all possible permutations.

Example:

```C++
Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

<br/>

**类别**：Backtracking

**难度**：Medium

[NowCoder](https://leetcode.com/problems/permutations/)

<br/>

## 解题思路：

**题目翻译**：给定一个整形数组，要求求出这个数组的所有变形体。

**题目分析**：046Permutations的解题方法和077Combinations几乎是相同的，唯一需要注意的是，Permutation需要加一个bool类型的数组来进行记录哪个元素访问了，哪个没有，这样才不会导致重复出现，并且不同于Combinations的一点是，Permutations不需要排序。

比如上面的[1, 2, 3]，首先取出1，加入子集合，后面的2和3就是1的子节点，先取出2，把[1, 2]加入子集合，后面的3就是2的子节点，取出3，把[1, 2, 3]加入子集合。然后回溯，取出3，将[1, 3]加入子集合。

1处理完成之后，我们可以同样方式处理2，以及3。

**解题思路**: 遇到这种问题，很显然，第一个想法我们首先想到DFS，递归求解，对于数组中的每一个元素，找到以它为首节点的Permutations，这就要求在递归中，每次都要从数组的第一个元素开始遍历，这样，就引入了另外一个问题，我们会对于同一元素访问多次，这就不是我们想要的答案了，所以我们引入了一个bool类型的数组，用来记录哪个元素被遍历了（通过下标找出对应）。在对于每一个Permutation进行求解中，如果访问了这个元素，我们将它对应下表的bool数组中的值置为true，访问结束后，我们再置为false。

**时间复杂度分析**: 这道题同Combination，所以对于这道题的解答，时间复杂度同样是O(n!)。

<br/>

## 解题代码：

```C++
class Solution {
public:
    vector<vector<int>> permute(vector<int> &nums) {
        vector<vector<int>> permutations;
        if(nums.size() == 0) 
            return permutations;
        vector<int> curr;
        vector<bool> isVisited(nums.size(), false); 
        backTracking(permutations, curr, isVisited, nums);
        return permutations;
    }
    
    void backTracking(vector<vector<int>>& ret, vector<int> curr, vector<bool> isVisited, vector<int> nums) {
        if(curr.size() == nums.size()) {
            ret.push_back(curr);
            return;
        }

        for(int i = 0; i < nums.size(); ++i) {
            if(isVisited[i] == false) {
                isVisited[i] = true;
                curr.push_back(nums[i]);
                backTracking(ret, curr, isVisited, nums);
                isVisited[i] = false;
                curr.pop_back();
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
    vector<vector<int> > permute(vector<int> &num) {
	    vector<vector<int> > result;
	    
	    permuteRecursive(num, 0, result);
	    return result;
    }
    
    // permute num[begin..end]
    // invariant: num[0..begin-1] have been fixed/permuted
	void permuteRecursive(vector<int> &num, int begin, vector<vector<int> > &result) {
		if (begin >= num.size()) {
		    // one permutation instance
		    result.push_back(num);
		    return;
		}
		
		for (int i = begin; i < num.size(); i++) {
		    swap(num[begin], num[i]);
		    permuteRecursive(num, begin + 1, result);
		    // reset
		    swap(num[begin], num[i]);
		}
    }
};
```

<br/>
