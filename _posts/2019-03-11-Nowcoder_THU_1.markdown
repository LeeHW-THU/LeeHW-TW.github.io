---
layout:     post
title:      "「牛客网」——「清华机考」—— 反序数"
subtitle:   " 记录打码。 "
date:       2019-03-11 
author:     "LeeHW"
header-img: "img/post-bg-digital-native.jpg"
catalog: true
tags:
    - NowCoder
---

## 题目

设N是一个四位数，它的9倍恰好是其反序数（例如：1234的反序数是4321）
求N的值

## 输入描述:

```
程序无任何输入数据。
```

## 输出描述:

```
输出题目要求的四位数，如果结果有多组，则每组结果之间以回车隔开。
```

---

## 代码

```c++
#include<iostream>
using namespace std;
void Reverse(int x) //求反序
{
    int res=0,flag=x;
    while(x>0){
        res=res*10+x%10;
        x/=10;
    }
    return res;
}
int main(){
    for(int i=1000;i<=1111;i++){
        //跳过不可能的情况(尝试加快运行时间)
        if((i%9!=0)||i%10==0) continue;
        //与反序对比
        if(Reverse(i)==i*9)
            cout<<i<<endl;
    }
}
```



---

## 结语

取反序参考[Leetcode-7-Reverse Integer](https://lihongwei.site/2019/02/10/LeetCode_7/)中的方法。

运用数学技巧发现，搜索区间为【1000~1111】(四位数最大值，9999/9=1111)

