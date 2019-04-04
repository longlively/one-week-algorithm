# 095 Subsets II

> DATE: 2019.4.3 ~ 2019.4.4

## 题目描述：

> Given a collection of integers that might contain duplicates, **nums**, return all possible subsets (the power set).

**Note:** The solution set must not contain duplicate subsets.

Example:

```C++
Input: [1,2,2]
Output:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```

<br/>

**类别**：Backtracking

**难度**：Medium

[NowCoder](https://leetcode.com/problems/subsets-ii/)

<br/>

## 解题思路：

**题目分析**：这题跟Subsets唯一的区别在于有重复元素，但是我们得到的子集合又不能有相同的，其实做法很简单，仍然按照Subsets的处理，只是遍历子节点的时候如果发现有相等的，只遍历一个，后续跳过。

<br/>

## 解题代码：

```C++
class Solution {
public:
    vector<vector<int>> res;
    vector<vector<int>> subsetsWithDup(vector<int> &nums) {
        if(nums.empty()) {
            return res;
        }
        sort(nums.begin(), nums.end());
        res.push_back(vector<int>());
        vector<int> v;
        generate(0, v, nums);
        return res;
    }

    void generate(int start, vector<int>& v, vector<int> &nums) {
        if(start == nums.size()) {
            return;
        }

        for(int i = start; i < nums.size(); i++) {
            v.push_back(nums[i]);
            res.push_back(v);
            generate(i + 1, v, nums);
            v.pop_back();
            while(i < nums.size() - 1 && nums[i] == nums[i + 1]) {
                i++;
            }
        }
    }
};
```

<br/>

论坛优质解答欣赏：

To solve this problem, it is helpful to first think how many subsets  are there. If there is no duplicate element, the answer is simply 2^n,  where n is the number of elements. This is because you have two choices  for each element, either putting it into the subset or not. So all  subsets for this no-duplicate set can be easily constructed:
 num of subset

- (1        to 2^0) empty set is the first subset
- (2^0+1 to 2^1) add the first element into subset from (1)
- (2^1+1 to 2^2) add the second element into subset (1 to 2^1)
- (2^2+1 to 2^3) add the third element into subset (1 to 2^2)
- ....
- (2^(n-1)+1 to 2^n) add the nth element into subset(1 to 2^(n-1))

Then how many subsets are there if there are duplicate  elements? We can treat duplicate element as a spacial element. For  example, if we have duplicate elements (5, 5), instead of treating them  as two elements that are duplicate, we can treat it as one special  element 5, but this element has more than two choices: you can either  NOT put it into the subset, or put ONE 5 into the subset, or put TWO 5s  into the subset. Therefore, we are given an array (a1, a2, a3, ..., an)  with each of them appearing (k1, k2, k3, ..., kn) times, the number of  subset is (k1+1)*(k2+1)*...(kn+1). We can easily see how to write down all the subsets similar to the approach above.

```C++
class Solution {
public:
    vector<vector<int> > subsetsWithDup(vector<int> &S) {
        vector<vector<int> > totalset = {{}};
        sort(S.begin(),S.end());
        for(int i=0; i<S.size();){
            int count = 0; // num of elements are the same
            while(count + i<S.size() && S[count+i]==S[i])  count++;
            int previousN = totalset.size();
            for(int k=0; k<previousN; k++){
                vector<int> instance = totalset[k];
                for(int j=0; j<count; j++){
                    instance.push_back(S[i]);
                    totalset.push_back(instance);
                }
            }
            i += count;
        }
        return totalset;
        }
};
```

<br/>
