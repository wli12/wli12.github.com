---
layout: post
title: 【Sorting】【Linked List】Merge k Sorted Lists
category: algorithm
description: Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.
---
[Merge k Sorted Lists](https://oj.leetcode.com/problems/merge-k-sorted-lists/)
>{{ page.description }}

####思路
用一个priority_queue（最小堆）维护k个链表中的最小值，然后剩下的事情就太简单了，出乎意料两次过。。。
####代码
```cpp
class mycomp{
    public:
    bool operator()(ListNode * lhs, ListNode * rhs) const {
        return (lhs->val > rhs->val);
    }
};
class Solution {
    public:
    ListNode *mergeKLists(vector<ListNode *> &lists) {
        if(lists.size() == 0) return NULL;
        if(lists.size() == 1) return lists[0];        
        priority_queue<ListNode *, vector<ListNode *>, mycomp> smallq;
        for(ListNode * plist : lists){
            if(plist != NULL)
                smallq.push(plist);
        }
        if(smallq.empty()) return NULL;//忽略这点会过不了[{},{}]
        ListNode * newHead = smallq.top();
        ListNode * newTail = newHead;
        while(!smallq.empty()){
            ListNode * small = smallq.top();
            smallq.pop();
            if(small->next != NULL)
                smallq.push(small->next);
            newTail->next = small;
            newTail = small;
        }
        newTail->next = NULL;
        return newHead;
    }  
};
```

转载请注明出处：[{{ page.title }}]({{ page.url}})
