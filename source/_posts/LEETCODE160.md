---
title: 相交链表
cover: https://i.imgtg.com/2022/05/14/zXxax.png
tags: leetcode
date: 2021-09-21
---
> 题目链接：https://leetcode.cn/problems/intersection-of-two-linked-lists/

### 解题思路

由于题干要求返回出去后链表必须保持其原始结构，我就想到能不能用一个指针数组来实现这种操作：将两条链表每个节点的地址分别压入两个栈中，然后循环比较两个栈的栈顶元素，同时记录上一位栈顶元素。当遇到第一个不同的节点时，结束循环，上一位栈顶元素即是答案。调bug调了快两个小时终于是解决了。。。还是太菜了

![Snipaste_2021-09-21_17-27-55.png](https://pic.leetcode-cn.com/1632216486-dpjFvY-Snipaste_2021-09-21_17-27-55.png)

### 代码

```c
typedef struct Stack
{
    struct ListNode** array;
    int capacity;
    int top;
} Stack;

Stack* newStack(int capacity)
{
    Stack* stack = NULL;
    stack = (Stack*)calloc(sizeof(Stack), 1);
    stack->array = (struct ListNode**)malloc(sizeof(struct ListNode*) * capacity);
    stack->capacity = capacity;
    return stack;
}

struct ListNode* pushStack(Stack* stack, struct ListNode* address_data)
{
    if (stack->top >= stack->capacity)
    {
        exit(EXIT_FAILURE);
    }
    stack->array[stack->top++] = address_data;

    return address_data;
}

struct ListNode* popStack(Stack* stack)
{
    struct ListNode* data = 0;
    if (stack->top < 1)
    {
        exit(EXIT_FAILURE);
    }
    data = stack->array[--stack->top];
    return data;
}

int emptyStack(Stack* stack)
{
    return stack->top < 1;
}

struct ListNode* peekStack(Stack* stack)
{
    if (stack->top < 1)
    {
        exit(EXIT_FAILURE);
    }

    return stack->array[stack->top - 1];
}

void delStack(Stack** stack)
{
    free(stack[0]->array);
    free(stack[0]);
    stack[0] = NULL;
}

struct ListNode* getIntersectionNode(struct ListNode* headA, struct ListNode* headB)
{
    Stack* stackA = NULL;
    Stack* stackB = NULL;
    stackA = newStack(30000);
    stackB = newStack(30000);

    while (headA)
    {
        pushStack(stackA, headA);
        headA = headA->next;
    }
    while (headB)
    {
        pushStack(stackB, headB);
        headB = headB->next;
    }
    if(peekStack(stackA) != peekStack(stackB))
    {
        delStack(&stackA);
        delStack(&stackB);
        return NULL;
    }
  
    struct ListNode* save = peekStack(stackA);  //用来保存上一个出栈元素
    popStack(stackA);
    popStack(stackB);
  
    while (!emptyStack(stackA) && !emptyStack(stackB))
    {
        if(peekStack(stackA) != peekStack(stackB))
        {
            delStack(&stackA);
            delStack(&stackB);
            break;
        }
        save = peekStack(stackA);
        popStack(stackA);
        popStack(stackB);
    }
    return save;
}
```
