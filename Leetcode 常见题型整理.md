# Leetcode 常见题型整理

一些个人刷leetcode的题型总结

希望开了这个repo能push自己好好刷leetcode（：

---

[TOC]

---

## Linklist

### 暴力使用for/while 遍历链表

#### [160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

思路: 从例子我们可以观察发现，由于分叉在前面，因此更长的链表多出来的node一定不会重合，因此考虑先遍历两个链表找到长度之差，再通过相减，得到长度开始的node，也就是可能开始重合的node，然后一个个走下去找到交叉的node

---

个人感觉这道题比较无聊，没有什么特别的技巧在里面，遍历两遍实在有些丑陋但也满足了复杂度要求x

Solution:

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* A_next = headA->next;
        ListNode* B_next = headB->next;
        int A_length = 1;
        int B_length = 1;
        while(A_next != nullptr){
            A_length ++;
            A_next = A_next->next;
        }
        while(B_next != nullptr){
            B_length ++;
            B_next = B_next->next;
        }
        int delta_length = A_length - B_length;
        ListNode* l_list = headA;
        ListNode* s_list = headB;
        if(A_length < B_length){
            delta_length = -delta_length;
            l_list = headB;
            s_list = headA;
        }
        for(int i = 0;i<delta_length;i++){
            l_list = l_list->next;
        }
        while(l_list != nullptr){
            if(l_list == s_list) return l_list;
            l_list = l_list->next;
            s_list = s_list->next;
        }
        return nullptr;
    }
};
```



## Tree

### 通过递归tree来求解

#### [236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

思路：一开始在想bfs或者dfs求解，但是后面试了一下发现不太行（（，后面才发现可以使用最经典的递归求解，感觉难度在于递归了以后怎么找到办法来记录那个是LCA，这里选择了通过判断result什么时候从1变为2来解决，同时要注意分为3种情况讨论一下

- 两个需要的node是并列的：2=0+1+1
- 一个是另一个的子node：2 = 1+0+1
- 一个在parent的左边，一个在右边：2 = 0+1+1

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
    TreeNode * lca = nullptr;
    int lca_counter(TreeNode* root, TreeNode* p, TreeNode* q){
        if(root == nullptr) return 0;
        if(root == p || root == q ){
            int result = 1 + lca_counter(root->left, p, q) + lca_counter(root->right, p, q);
            if(result == 2) lca = root;
            return result;
        }
        int left = lca_counter(root->left, p, q);
        int right = lca_counter(root->right, p, q);
        if(left == 1 && right ==1) lca = root;
        return left + right;
    }
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        lca_counter(root, p, q);
        return lca;
    }
};
```

## Dynamic Programming 

### 经典迷宫小问题

经典203例题，简单无需多言

### 买卖股票问题

需要用到270里FSM的思想

### Knapsack问题

