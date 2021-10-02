# 1707. Maximum XOR With an Element From Array

## Question link
> https://leetcode.com/problems/maximum-xor-with-an-element-from-array/

## Question Description
You are given an array nums consisting of non-negative integers. You are also given a queries array, where queries[i] = [xi, mi].

The answer to the ith query is the maximum bitwise XOR value of xi and any element of nums that does not exceed mi. In other words, the answer is max(nums[j] XOR xi) for all j such that nums[j] <= mi. If all elements in nums are larger than mi, then the answer is -1.

Return an integer array answer where answer.length == queries.length and answer[i] is the answer to the ith query.

> Example 1:
> Input: nums = [0,1,2,3,4], queries = [[3,1],[1,3],[5,6]]
> Output: [3,3,7]
> Explanation:
> 1) 0 and 1 are the only two integers not greater than 1. 0 XOR 3 = 3 and 1 XOR 3 = 2. The larger of the two is 3.
> 2) 1 XOR 2 = 3.
> 3) 5 XOR 2 = 7.

> Example 2:
> Input: nums = [5,2,4,6,6,3], queries = [[12,4],[8,1],[6,3]]
> Output: [15,-1,5]

Constraints:
- 1 <= nums.length, queries.length <= 10<sup>5</sup>
- queries[i].length == 2
- 0 <= nums[j], xi, mi <= 10<sup>9</sup>

## 分类 && 解题思路
tries
bitwise

## Code Implementation
```c++
class TrieNode {
public:
    vector<TrieNode*> children;
    TrieNode(){
        children.resize(2, NULL);
    }

    void addNum(TrieNode* root, int num){
        TrieNode* cur = root;
        for(int i = 31; i >= 0; i--){
            int curBit = (num >> i) & 1;
            if(cur->children[curBit] == NULL){
                cur->children[curBit] = new TrieNode();
            }
            cur = cur->children[curBit];
        }
    }

    int findMaxXor(TrieNode* root, int num, int l){
        int sum = 0;
        TrieNode* cur = root;
        string ss;
        for(int i = 31; i >= 0; i--){
            int curBit = (num >> i) & 1;
            int otherChoice = curBit == 1 ? 0 : 1;
            if(cur->children[otherChoice] == NULL){
                cur = cur->children[curBit];
            }else{
                sum += (1 << i);
                cur = cur->children[otherChoice];
            }
        }

        return sum;
    }
};

class Solution {
public:
    vector<int> maximizeXor(vector<int>& nums, vector<vector<int>>& queries) {
        vector<int> res;
        TrieNode * root = new TrieNode();
        for(int num : nums){
            root->addNum(root, num);
        }

        for(int i = 0; i < queries.size(); i++){            
            res.push_back(root->findMaxXor(root, queries[i][0], queries[i][1] ));
        }

        return res;
    }
};
```

## Time Complexity Analysis