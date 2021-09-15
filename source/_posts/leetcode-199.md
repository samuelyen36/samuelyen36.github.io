---
title: Leetcode 199 - Binary Tree Right Side View
date: 2021-08-19 17:28:31
tags: Leetcode
categories: 資工
description: Solution and explanation of Binary Tree Right Side View using C++
---

## Question
[https://leetcode.com/problems/binary-tree-right-side-view/](https://leetcode.com/problems/binary-tree-right-side-view/)
題目為給定一 Binary Tree ，要求各深度最右方的 Node 的數值
我解題使用的語言是 C++

## My Solution
我的解法是將其視為 DFS(depth first search) 的延伸，以一個 vector 儲存該深度最右方的 value
此方法最重要的是下方這段code

```C++
if(res.size() == depth ){
    res.push_back(ptr->val);
}
```

由於 vector 是最自動增長大小的，所以我們可以好好利用此特性
在這個 Recursive function 中，其 traversal 的路徑是往右方走到底，之後再往左邊的 node 去查找
因此，我們在該層(亦同該深度)中，第一個走到的 element 就是題目要我們求的數值 (當然，要扣掉 nullptr )
也因為每走到新的一層，我們就會 push 一個數值進 vector 之中。因此，如果看到 vector size == depth的話，就代表這個 depth的解還沒被存入 vector (depth 是 zero index的，所已是相等)

## Code

```C++
 #include <vector>
 using namespace std;
class Solution {
public:
    void traversal(TreeNode*, vector<int>&, int);
    vector<int> rightSideView(TreeNode* root) {
        vector<int> res;
        traversal(root, res, 0);
        return res;
    }
};

void Solution::traversal(TreeNode* ptr, vector<int>& res, int depth){
    if(!ptr){
        return;
    }
    else{
        if(res.size() == depth ){
            res.push_back(ptr->val);
        }
            traversal(ptr->right, res, depth+1);
            traversal(ptr->left, res, depth+1);    
    }
    return;
}
```