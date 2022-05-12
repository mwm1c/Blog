---
title: KMP算法（配图解释）
---

未经改进的KMP代码：
```c
void get_next(SString T,int next[])
{
    int i = 1, j = 0;
    next[1] = 0;
    //这里以及后面的代码都假设串的长度都存储在ch[0]这个地方
    while (i<T.ch[0])
    {
        if (j==0||T.ch[i]==T.ch[j])
        {
            ++i;
            ++j;
            //若pi=pj,则next[j+1] = next[j]+1;
            next[i] = j;
        }
        else
            //否则令j=next[j],循环继续
            j = next[i];
    } 
}
int Index_KMP(SString S,SString T)
{
    int i =1,j=1;
    int next[T.ch[0]+1];
    get_next(T, next);
    //假设字符串长度存在ch[0]这个位置
    while (i<=S.ch[0]&&j<=T.ch[0])
    {
        if(j==0||S.ch[i]==T.ch[i])
        {
            ++i;
            ++j;        //继续比较后继字符
        }
        else
            j=next[j];  //模式串向右移动
    }
    if(j>T.ch[0])
        return i-T.ch[0];  //匹配成功
    else
        return 0;
}
```

以主串 **googmegoogle** 与子串 **google** 进行模式匹配为例

```c
next[7]={0,0,1,1,2,1};	//这里的next[0]其实没什么用，在这个伪代码里根本没有用到
```
注意一下配图中我把 `j=0` 的图都省略掉了，因为当`j=0`时会发生`i++; j++; `没有必要画上去。
![查找过程](https://img-blog.csdnimg.cn/2071bb86f00946439d49fde210223bdd.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ3NDM1MTU4,size_16,color_FFFFFF,t_70#pic_center)
不过其实还可以改进一下这个过程，在这之前再来看一个例子：
以主串 **aaacaaaab** 与子串 **aaaab** 进行上面这种KMP模式匹配为例
![查找过程](https://img-blog.csdnimg.cn/30cfc8dc6a194aa6a44b2bd23fb81ed4.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ3NDM1MTU4,size_16,color_FFFFFF,t_70#pic_center)
可以看到，在子串中有连续出现大量相同字母时，上图中的**②、③、④**其实是多余的判断。这种未经改进算法的时间复杂度就相当于朴素的模式匹配算法。
那么应该如何改进？看到上面那两个例子，指针j的回溯位置是基于next数组的，因此理所当然的要改进next数组了。改进方法也很简单，看到子串中前四个字符相同，那么是不是可以用 **next[1]** 的值来取代后续与它相等的字符（ **next[2]、next[3]、next[4]** ）中的值呢？这就出现了改良后的next数组：**nextval[ ]**，实现代码如下：

```c
void get_nextval(SString T,int nextval[])
{
    int i = 1, j = 0;
    nextval[1] = 0;
    while (i<T.ch[0])
    {
        if (j==0||T.ch[i]==T.ch[j])
        {
            ++i;
            ++j;
            if(T.ch[i] != T.ch[j])	/* 若当前字符与前缀字符不同 */ 
            	nextval[i] = j;		/* 则当前的j为nextval在i位置的值 */ 
           	else
           		/* 如果与前缀字符相同，则将前缀字符的nextval值赋值给nextval在i位置的值 */
           		nextval[i] = nextval[j];
        }
        else
            j = nextval[i];		/* 若字符不相同，则j值回溯 */  
    }
}
```
