---
layout: post
title: 【DFS】【Matrix】Word Search
category: algorithm
description: Given a 2D board and a word, find if the word exists in the grid.
---
[Word Search](https://oj.leetcode.com/problems/word-search/)

{{ page.description }}

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

For example,

Given board =

```
[
  ["ABCE"],
  ["SFCS"],
  ["ADEE"]
]
```
word = "ABCCED", -> returns true,

word = "SEE", -> returns true,

word = "ABCB", -> returns false.

####思路
- 没有什么算法在里头，深搜+递归（回溯），15min写完
- 如果是mXn的矩阵，长为l的字串，那么find那一步的最坏情况是一个高为l的满四叉树（其他分支都在最后一步fail），1+4+4^2+...+4^(l-1)=(4^l-1)/3
![](http://upload.wikimedia.org/math/6/1/6/61678cc844f354ce4114e6d4fd1e782b.png)不过平均情况应该会比这个好很多

####代码
```cpp
bool exist(vector<vector<char> > &board, string word) {
    int lx = board.size();
    if(lx == 0) return false;
    int ly = board[0].size();
    if(ly == 0) return false;
    for(int x = 0; x < lx; x++){
        for(int y = 0; y < ly; y++){
            if(find(board, x, y, word.c_str()))
                return true;
        }
    }
    return false;
}

bool find(vector<vector<char> > &board, int x, int y, const char *p){
    if(*p == '\0') //不是*p == NULL
        return true;
    if(x >= board.size() || y >= board[0].size() || board[x][y] != *p++)
        return false;
    board[x][y] = ~board[x][y];//忘掉这个会WA Input:["aa"], "aaa"
    int dx[] = {1, -1, 0, 0};
    int dy[] = {0, 0, 1, -1};
    for(int i = 0; i< 4; i++){
        if(find(board, x+dx[i], y+dy[i], p))
            return true;
    }
    board[x][y] = ~board[x][y];
    return false;
}
```

转载请注明出处：[{{ page.title }}]({{ page.url}})



