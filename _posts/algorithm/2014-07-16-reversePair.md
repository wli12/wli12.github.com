---
layout: post
title: 【Sorting】【Array】Reverse pairs
category: algorithm
description: Numbers are said to be "reverse ordered" if N[i] > N[j] for i < j. How to get the number of pairs of reverse ordered items in O(nlogn) time.
---
##[数组中的逆序对](http://ac.jobdu.com/problem.php?pid=1348)
>在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。
####输入：
每个测试案例包括两行：
第一行包含一个整数n，表示数组中的元素个数。其中1 <= n <= 10^5。
第二行包含n个整数，每个数组均为int类型。
####输出：
对应每个测试案例，输出一个整数，表示数组中的逆序对的总数。
####样例输入：
4
7 5 6 4
####样例输出：
5

###思路
分治的思想，统计两边内部的逆序对，以及左右两边之间的逆序对，编程上需要注意mid应该归前半段（23|45）

###代码
```cpp
#include <iostream>
#include <vector>
using namespace std;
long long merge2part(vector<int>& nums, int start, int mid, int end){
    vector<int> tmp;
    int p = start;
    int q = mid+1;
    long long npair = 0;
    while(p <= mid && q <= end){
        if(nums[p]<=nums[q]){
            tmp.push_back(nums[p++]);
        } else{
            tmp.push_back(nums[q++]);
            npair += mid - p + 1;//核心
        }
    }
    while(p <= mid){
        tmp.push_back(nums[p++]);
    }
    while(q <= end){
        tmp.push_back(nums[q++]);
    }
    /*
    for(int n : tmp){
        nums[i++] = n;
    }
     */
    int i = start;
    for (vector<int>::iterator it = tmp.begin() ; it != tmp.end(); ++it){
        nums[i++] = *it;
    }
    return npair;
}
long long reversePair(vector<int>& nums, int start, int end){
    if(start >= end) return 0;
    int mid = (start + end) /2;
    long long npair = 0; //根据提供的数据量，估算npair会超出int的范围
    npair += reversePair(nums, start, mid); //mid应该归前半段（23|45）
    npair += reversePair(nums, mid+1, end);
    npair += merge2part(nums, start, mid, end);
    return npair;
}
int main() {
    int a, b;
    vector<int> nums;
    while(cin >> a){
        nums.clear();//别忘了清零
        while (a-- > 0 && cin >> b){
            nums.push_back(b);
        }
        cout << reversePair(nums, 0, nums.size()-1) << endl;
    }
    return 0;
}
/**************************************************************
    Problem: 1348
    User: Silviacc
    Language: C++
    Result: Accepted
    Time:240 ms
    Memory:3064 kb
****************************************************************/
```

转载请注明出处：[{{ page.title }}]({{ page.url}})
