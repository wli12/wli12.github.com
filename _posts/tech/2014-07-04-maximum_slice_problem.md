---
layout: post
title: MaxSliceSum & MaxDoubleSliceSum
description: Find a maximum sum of a compact subsequence of array elements and any double slice. 

category: tech
---

##MaxSliceSum

5 -7 3 5 -2 4 -1

这个数组的最大子串是3 5 -2 4
<p style="box-sizing: border-box; margin: 0px; padding: 0px; font-family: Verdana, Arial, Helvetica; font-size: 12px;">Complexity:</p>
<blockquote style="box-sizing: border-box; font-family: Verdana, Arial, Helvetica; font-size: 12px;">
<ul style="box-sizing: border-box; margin: 10px; padding: 0px;">
<li style="box-sizing: border-box;">expected worst-case time complexity is O(N);</li>
<li style="box-sizing: border-box;">expected worst-case space complexity is O(1), beyond input storage (not counting the storage required for input arguments).</li>
</ul>
</blockquote>

###思路

O(n) 的解法的关键，是把原来求最优的问题转化为一个具有最优子结构的问题，枚举最大字串的右边界，再在求解其各个子问题解的过程中取最大值。假设以第i个元素结尾的子序列的最大值是max_ending，那么以第i+1个元素结尾的子序列的最大值是max(0, max_ending+ a[i+1])

###代码

```
#include <math.h>
int solution(const vector<int> &A) {
    if(A.size() == 0) return 0;
    int max_ending_i = 0;
    int max_slice = A[0];
    for(int i : A){
        max_ending_i = max(i, max_ending_i+i);
        if(max_ending_i > max_slice) max_slice = max_ending_i;
    }
    return max_slice;
}
```

##MaxDoubleSliceSum
<p style="box-sizing: border-box; margin: 0px; padding: 0px; font-family: Verdana, Arial, Helvetica; font-size: 12px;">A non-empty zero-indexed array A consisting of N integers is given.</p>
<p id="brinza-task-description" style="box-sizing: border-box; font-family: Verdana, Arial, Helvetica; font-size: 12px;">
<p style="box-sizing: border-box; margin: 0px; padding: 0px;">A triplet (X, Y, Z), such that 0 ≤ X &lt; Y &lt; Z &lt; N, is called a <em style="box-sizing: border-box;">double slice</em>.</p>
<p style="box-sizing: border-box; margin: 0px; padding: 0px;">The <em style="box-sizing: border-box;">sum</em> of double slice (X, Y, Z) is the total of A[X + 1] + A[X + 2] + ... + A[Y − 1] + A[Y + 1] + A[Y + 2] + ... + A[Z − 1].</p>
<p style="box-sizing: border-box; margin: 0px; padding: 0px;">For example, array A such that:</p>
<p style="box-sizing: border-box; margin: 0px; padding: 0px;"> </p>
<pre style="box-sizing: border-box; font-family: monospace, serif; font-size: 1em; white-space: pre-wrap;"><tt style="box-sizing: border-box;"> A[0] = 3 A[1] = 2 A[2] = 6 A[3] = -1 A[4] = 4 A[5] = 5 A[6] = -1 A[7] = 2</tt></pre>
<p style="box-sizing: border-box; margin: 0px; padding: 0px;">contains the following example double slices:</p>
<blockquote style="box-sizing: border-box;">
<ul style="box-sizing: border-box; margin: 10px; padding: 0px;">
<li style="box-sizing: border-box;">double slice (0, 3, 6), sum is 2 + 6 + 4 + 5 = 17,</li>
<li style="box-sizing: border-box;">double slice (0, 3, 7), sum is 2 + 6 + 4 + 5 − 1 = 16,</li>
<li style="box-sizing: border-box;">double slice (3, 4, 5), sum is 0.</li>
</ul>
</blockquote>
<p style="box-sizing: border-box; margin: 0px; padding: 0px;">The goal is to find the maximal sum of any double slice.</p>
<p style="box-sizing: border-box; margin: 0px; padding: 0px;">Write a function:</p>
<blockquote style="box-sizing: border-box;">
<p class="lang-c" style="box-sizing: border-box; margin: 0px; padding: 0px; font-family: monospace; font-size: 9pt;"><tt style="box-sizing: border-box;">int solution(int A[], int N);</tt></p>
</blockquote>
<p style="box-sizing: border-box; margin: 0px; padding: 0px;">that, given a non-empty zero-indexed array A consisting of N integers, returns the maximal sum of any double slice.</p>
<p style="box-sizing: border-box; margin: 0px; padding: 0px;">For example, given:</p>
<p style="box-sizing: border-box; margin: 0px; padding: 0px;"> </p>
<pre style="box-sizing: border-box; font-family: monospace, serif; font-size: 1em; white-space: pre-wrap;"><tt style="box-sizing: border-box;"> A[0] = 3 A[1] = 2 A[2] = 6 A[3] = -1 A[4] = 4 A[5] = 5 A[6] = -1 A[7] = 2</tt></pre>
<p style="box-sizing: border-box; margin: 0px; padding: 0px;">the function should return 17, because no double slice of array A has a sum of greater than 17.</p>
<p style="box-sizing: border-box; margin: 0px; padding: 0px;">Assume that:</p>
<blockquote style="box-sizing: border-box;">
<ul style="box-sizing: border-box; margin: 10px; padding: 0px;">
<li style="box-sizing: border-box;">N is an integer within the range [<span class="number" style="box-sizing: border-box;">3</span>..<span class="number" style="box-sizing: border-box;">100,000</span>];</li>
<li style="box-sizing: border-box;">each element of array A is an integer within the range [<span class="number" style="box-sizing: border-box;">−10,000</span>..<span class="number" style="box-sizing: border-box;">10,000</span>].</li>
</ul>
</blockquote>
<p style="box-sizing: border-box; margin: 0px; padding: 0px;">Complexity:</p>
<blockquote style="box-sizing: border-box;">
<ul style="box-sizing: border-box; margin: 10px; padding: 0px;">
<li style="box-sizing: border-box;">expected worst-case time complexity is O(N);</li>
<li style="box-sizing: border-box;">expected worst-case space complexity is O(N), beyond input storage (not counting the storage required for input arguments).</li>
</ul>
</blockquote>
<p style="box-sizing: border-box; margin: 0px; padding: 0px;">Elements of input arrays can be modified.</p>

###思路
1. 乍一看这个要比最大子段和高级，似乎要枚举最大字串的右边界i和中间点center，但其实只用枚举右边界i，右边界从i-1变成i之后，中间点只可能是center或者i-1中的一个，中间点为k的时候左边界不变最优，中间点为i-1的时候需要求最优左边界（这就是个内嵌的MaxSliceSum问题）；还有一种情况就是只保留A[i-1]一个元素。整体复杂度仍然是时间 O(N),空间 O(1)。
2. 还有一种思路就是只枚举中间点k，分别算以k-1结尾和k+1开头的最大字段和，然后再把两者拼起来，[这种方法](http://blog.csdn.net/caopengcs/article/details/17491395)整体复杂度是时间 O(N),空间 O(N)。

###代码
```
#include <math.h>
int max(int a,int b,int c);
int solution(vector<int> &A) {
    if(A.size() <= 3) return 0;
    int max_left = 0;//中间点为i-1右边界为i时左段最大值
    int max_ending = 0;//右边界为i时两段最大值
    int center = 1;//中间点
    int max_slice = 0;
    for(int i = 3; i< A.size(); i++){
        max_left = max(max_left+A[i-2], A[i-2]);//MaxSliceSum问题
        max_ending = max(max_ending+A[i-1], A[i-1], max_left);//MaxDoubleSliceSum问题
        if(max_ending == A[i-1]) center = i-2;
        else if(max_ending == max_left) center = i-1;
        if(max_ending > max_slice) max_slice = max_ending;
    }
    return max_slice;
}

inline int max(int a, int b, int c){
   if(b>a) a=b;
   if(c>a) a=c;
   return a;
}
```

