---
layout: post
title: 【DP】【String】Palindrome Partitioning
category: algorithm
description: Given a string s, partition s such that every substring of the partition is a palindrome. 
---
##[Palindrome Partitioning](https://oj.leetcode.com/problems/palindrome-partitioning/)

{{ page.description }}

Return all possible palindrome partitioning of s.

For example, given s = "aab",

Return

```
[
  ["aa","b"],
  ["a","a","b"]
]
```

###思路
有两步DP：

1. whether s[i,j] is palindrome 
2. sub palindromes of s[i, n-1]

###代码
```cpp
vector<vector<string> > partition(string s) {
    const int n = s.size();
    bool p[n][n]; // 1. whether s[i,j] is palindrome 
    fill_n(&p[0][0], n * n, false);
    for (int i = n - 1; i >= 0; --i)
        for (int j = i; j < n; ++j)
            p[i][j] = s[i] == s[j] && ((j - i < 2) || p[i + 1][j - 1]); //第一步DP的核心递推式
    vector<vector<string> > sub_palins[n]; // 2. 要存下所有的结果，sub_palins[i]:sub palindromes of s[i, n-1]
    for (int i = n - 1; i >= 0; --i) {
        for (int j = i; j < n; ++j)
            if (p[i][j]) {//s[i,j] is palindrome
                const string palindrome = s.substr(i, j - i + 1);//substr(pos, len)
                if (j + 1 < n) {
                    for (auto v : sub_palins[j + 1]) { //在sub_palins[j + 1]中元素加上palindrome，生成sub_palins[i]中的元素
                        v.insert(v.begin(), palindrome);//第二步DP的核心递推式
                        sub_palins[i].push_back(v);
                    }
                } else {
                    sub_palins[i].push_back(vector<string> { palindrome });//C++11大法好！
                }
            }
    }
    return sub_palins[0];
}
```

转载请注明出处：[{{ page.title }}]({{ page.url}})



