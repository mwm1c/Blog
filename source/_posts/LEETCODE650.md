---
title: 只有两个键的键盘
cover: https://i.imgtg.com/2022/05/14/zXzNj.jpg
tags: leetcode
date: 2021-09-21
---

> 题目链接： https://leetcode.cn/problems/2-keys-keyboard/

### 解题思路
本算法思路主要来源于宫水三叶的题解：
经过数学归纳推理可以发现，如果将1次`Copy All`操作和x次`Paste`操作看作一次操作序列，那么进行这样一次操作序列后，能打印出的字符数相当于进行这次操作序列之前字符数的(x+1)倍。

上面的那些思考捋清楚后，就可以求最少操作次数了：
1. 进行1次`Copy All`操作和(k_1-1)次`Paste`操作，打印出的字符数相当于进行这次操作序列之前字符数的k_1倍。
2. 进行1次`Copy All`操作和(k_2-1)次`Paste`操作，打印出的字符数相当于进行这次操作序列之前字符数的k_2倍。
...
i. 进行1次`Copy All`操作和(k_i-1)次`Paste`操作，打印出的字符数相当于进行这次操作序列之前字符数的k_i倍。

在进行了这i次操作以后，得到了一个字符串长度为n的记事本，有n = 1 * k_1 * k_2 * ... * k_i。
这也保证了这里的n是一个合数(**所谓合数就是能被除1和自身以外正整数整除的数**，恰好与质数是对立的)。
这里，操作次数step = k_1 + k_2 + ... + k_i。由于对于任意的非1正整数都有`a+b <= a*b`，因此要想step达到最小值，只要保证每次分解出来的因子都是质数即可。

函数控制流程大致分为三种情况：
1. n=1时，无需做任何操作，直接`return 0;` 。
2. n为质数时，由于没有大于2的数能整除它，只能使用一次`Copy All`操作，因此最少操作次数就是n次。
3. n为合数时，根据上面的分析只需要慢慢试除即可。

### 代码

```c
int isPrime(int num)
{
    if(num == 1 || num%2 == 0 && num!=2)
    {
        return 0;
    }
    for(int i=3; i*i<=num; i+=2)
    {
        if(num%i == 0)
        {
            return 0;
        }
    }
    return 1;
}
int minSteps(int n)
{
    if(n == 1)
        return 0;
    else if(isPrime(n))
        return n;
    else
    {
        int step = 0;
        for(int i = 2; n > 1; i++)
        {
            while(n%i == 0)
            {
                step += i;
                n/=i;
            }
        }
        return step;
    }
}
```
