# 077 Combinations

> DATE: 2019.3.30 ~ 2019.3.31

## 题目描述：

> Given two integers n and k, return all possible combinations of k numbers out of 1 ... n.

Example:

```C++
Input: n = 4, k = 2
Output:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

<br/>

**类别**：Backtracking

**难度**：Medium

[NowCoder](https://leetcode.com/problems/combinations/)

<br/>

## 解题思路：

**题目翻译**：给定两个整型数组n和k，要求返由k个数组成的组合。这k个数是在n中任选k个数，由题意可得，这里的k应该小于或等于n(这个条件不要忘了做validation  check哦).

**题目分析**：这里我们先给一个例子比如n=3， k=2的条件下， 所有可能的组合如下： [1,2], [1,3], [2,3]。注意：[2,3]和[3,2]是相同的，我们只要求有其中一个就可以了。所以解题的时候，我们要避免相同的组合出现.

**解题思路**：看到这道题，首先第一想法应该是用递归来求解。如果要是用循环来求解，这个时间复杂度应该是比较恐怖了。并且，这个递归是一层一层往深处去走的，打个比方，我们一个循环，首先求得以1开始的组合，之后再求以2开始的，以此类推，所以开始是对n个数做DFS,  n-1个数做DFS...所以应该是对n*(n-1)*...*1做DFS. 在程序中，我们可以加一些剪枝条件来减少程序时间.

**时间复杂度**：在题目分析中，我们提到了对于对n,n-1,...,1做DFS，所以时间复杂度是O(n!)

<br/>

## 解题代码：

```C++
class Solution {
public:
    vector<vector<int> > combine(int n, int k) {
        vector<vector<int>> ret;
        if(n <= 0)  
            return ret;

        vector<int> curr;
        DFS(ret, curr, n, k, 1); 
        return ret;
    }

    void DFS(vector<vector<int>>& ret, vector<int> curr, int n, int k, int level) {
        if(curr.size() == k) {
            ret.push_back(curr);
            return;
        }
        if(curr.size() > k)   
            return;
        
        for(int i = level; i <= n; ++i) {
            curr.push_back(i);
            DFS(ret,curr,n,k,i+1);
            curr.pop_back();
        }
    }
};
```

<br/>

论坛优质解答欣赏：

```C++
class Solution {
public:
	vector<vector<int>> combine(int n, int k) {
		vector<vector<int>> result;
		int i = 0;
		vector<int> p(k, 0);
		while (i >= 0) {
			p[i]++;
			if (p[i] > n) --i;
			else if (i == k - 1) result.push_back(p);
			else {
			    ++i;
			    p[i] = p[i - 1];
			}
		}
		return result;
	}
};
```

<br/>
