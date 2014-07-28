---
layout: post
title: 【DP】【String】Palindrome Partitioning II 
category: algorithm
description: Given a string s, partition s such that every substring of the partition is a palindrome. Return the minimum cuts needed for a palindrome partitioning of s.
---
##[Palindrome Partitioning II](https://oj.leetcode.com/problems/palindrome-partitioning-ii/)
{{ page.description }}

For example, given s = "aab",

Return 1 since the palindrome partitioning ["aa","b"] could be produced using 1 cut.

###思路
- f(i)= 区间 [i, n-1] 之间最小的 cut 数,n 为字符串长度,则状态转移方程为 f(i) = min{f(j +1)+1},i ≤ j < n, p[i][j] = true
- 备忘录+递归

###代码

```cpp
int minCut(string s) {
    const int n = s.size();
    vector<vector<bool> > p(n, vector<bool>(n,-1)); // whether s[i,j] is palindrome
    for (int i = n - 1; i >= 0; --i)
        for (int j = i; j < n; ++j)
            p[i][j] = s[i] == s[j] && ((j - i < 2) || p[i + 1][j - 1]); //第一步DP的核心递推式n
    vector<int> cut(n, -1);
    return minpart(s, p, cut, n-1);
    
}
int minpart(string &s, vector<vector<bool> > &p, vector<int> &cut, int n){
    int mcut = s.length();
    if(p[0][n] == true)  return 0;//一定要有Trival case, 递归算法才有起点
    for(int i = 0 ;i <= n; i++){
        if(p[i][n] == true){
            if(cut[i-1] == -1){
                cut[i-1] = minpart(s, p, cut, i-1);
            }
            int tcut = cut[i-1] + 1;
            if(tcut < mcut) mcut = tcut;
        }
    }
    return mcut;
}
```
转载请注明出处：[{{ page.title }}]({{ page.url}})



