---
layout: post
title: 【DP】【String】Decode Ways
category: algorithm
description: A message containing letters from A-Z is being encoded to numbers. Given an encoded message containing digits, determine the total number of ways to decode it.
---
##[Decode Ways](https://oj.leetcode.com/problems/decode-ways/)
<p style="box-sizing: border-box; margin: 0px 0px 10px; color: #333333; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; font-size: 14px; line-height: 30px;">A message containing letters from <code style="box-sizing: border-box; font-family: Monaco, Menlo, Consolas, 'Courier New', monospace; font-size: 13px; padding: 2px 4px; color: #c7254e; white-space: nowrap; border-top-left-radius: 4px; border-top-right-radius: 4px; border-bottom-right-radius: 4px; border-bottom-left-radius: 4px; background-color: #f9f2f4;">A-Z</code> is being encoded to numbers using the following mapping:</p>
<pre style="box-sizing: border-box; font-family: Monaco, Menlo, Consolas, 'Courier New', monospace; font-size: 13px; white-space: pre-wrap; padding: 9.5px; margin: 0px 0px 10px; line-height: 1.428571429; color: #333333; word-break: break-all; word-wrap: break-word; border: 1px solid #cccccc; border-top-left-radius: 4px; border-top-right-radius: 4px; border-bottom-right-radius: 4px; border-bottom-left-radius: 4px; background-color: #f5f5f5;">'A' -&gt; 1
'B' -&gt; 2
...
'Z' -&gt; 26
</pre>
<p style="box-sizing: border-box; margin: 0px 0px 10px; color: #333333; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; font-size: 14px; line-height: 30px;">Given an encoded message containing digits, determine the total number of ways to decode it.</p>
<p style="box-sizing: border-box; margin: 0px 0px 10px; color: #333333; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; font-size: 14px; line-height: 30px;">For example,<br style="box-sizing: border-box;" />Given encoded message <code style="box-sizing: border-box; font-family: Monaco, Menlo, Consolas, 'Courier New', monospace; font-size: 13px; padding: 2px 4px; color: #c7254e; white-space: nowrap; border-top-left-radius: 4px; border-top-right-radius: 4px; border-bottom-right-radius: 4px; border-bottom-left-radius: 4px; background-color: #f9f2f4;">"12"</code>, it could be decoded as <code style="box-sizing: border-box; font-family: Monaco, Menlo, Consolas, 'Courier New', monospace; font-size: 13px; padding: 2px 4px; color: #c7254e; white-space: nowrap; border-top-left-radius: 4px; border-top-right-radius: 4px; border-bottom-right-radius: 4px; border-bottom-left-radius: 4px; background-color: #f9f2f4;">"AB"</code> (1 2) or <code style="box-sizing: border-box; font-family: Monaco, Menlo, Consolas, 'Courier New', monospace; font-size: 13px; padding: 2px 4px; color: #c7254e; white-space: nowrap; border-top-left-radius: 4px; border-top-right-radius: 4px; border-bottom-right-radius: 4px; border-bottom-left-radius: 4px; background-color: #f9f2f4;">"L"</code> (12).</p>
<p style="box-sizing: border-box; margin: 0px 0px 10px; color: #333333; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; font-size: 14px; line-height: 30px;">The number of ways decoding <code style="box-sizing: border-box; font-family: Monaco, Menlo, Consolas, 'Courier New', monospace; font-size: 13px; padding: 2px 4px; color: #c7254e; white-space: nowrap; border-top-left-radius: 4px; border-top-right-radius: 4px; border-bottom-right-radius: 4px; border-bottom-left-radius: 4px; background-color: #f9f2f4;">"12"</code> is 2.</p>

###思路
DP入门题，比较像stairs

###代码
```cpp
int numDecodings(string s) {
    int n = s.length();
    vector<int> ways(n, -1);
    return waysEnding(s,  ways, n-1);
}
int waysEnding(string &s, vector<int>& ways, int n){
    if(n < 0) return 0;//处理“”
    if(ways[n] != -1) return ways[n]; //返回备忘录的值
    
    int nway = 0;
    char c = s[n];
    if(c <= '9' && c >= '1'){
        if(n > 0)
            nway += waysEnding(s,ways,n-1);
        else
            nway += 1;
    }
    if(n < 1) return nway;//防止下面n-2出错
    
    int i = stoi(s.substr(n-1, 2));
    if(i <= 26 && i >= 10){
        if(n > 1)
            nway += waysEnding(s,ways,n-2);
        else
            nway += 1;
    }
    ways[n]= nway;//别忘了给备忘录设值，不然会TLE
    return nway;
}
```

转载请注明出处：[{{ page.title }}]({{ page.url}})



