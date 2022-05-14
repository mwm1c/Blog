---
title: 将有序数组转换为二叉树
cover: https://i.imgtg.com/2022/05/14/zXphF.png
tags: leetcode
date: 2021-09-21
---

> 题目链接： https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/
### 解题思路
我做这道题感觉比较难一点的应该就是要警惕数组访问越界的问题了。
大致可以分为两种情况。
一种情况是数组内有2个及以上的值，这种情况内要分两种小情况：
    1.数组内有2个以上的值，如果这样就对`root`节点的左右子树进行递归;
    2.数组内只有2个值，如果这样就只需要对`root`节点的左子树进行递归(因为数组是排好序的)。
还有一种情况是数组内只有一个值了，如果是这样给`root`节点的数据域赋完值就直接返回节点

### 代码

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */
struct TreeNode* sortedArrayToBST(int* nums, int numsSize)
{
    // 传进来的是数组，但返回出去的是一棵 平衡BST
    struct TreeNode* root = (struct TreeNode*)malloc(sizeof(struct TreeNode));
    root->val = nums[numsSize / 2];
    root->right = NULL;
    root->left = NULL;
    if (numsSize / 2 != 0)
    {
        int* pleft = nums;
        root->left = sortedArrayToBST(pleft, numsSize / 2);
        if (numsSize != 2)
        {
            int* pright = &nums[numsSize / 2 + 1];
            root->right = sortedArrayToBST(pright, numsSize - 1 - numsSize / 2);
        }
    }
    return root;
}
```
