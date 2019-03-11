---
layout:     post
title:      "「牛客网」——「清华机考」—— 求最大最小数"
subtitle:   " 记录打码。 "
date:       2019-03-11 
author:     "LeeHW"
header-img: "img/post-bg-digital-native.jpg"
catalog: true
tags:
    - Nowcoder_THU
---

## 题目

输入N个（N<=10000）数字，求出这N个数字中的最大值和最小值。每个数字的绝对值不大于1000000。

输入描述:

```
输入包括多组测试用例，每组测试用例由一个整数N开头，接下去一行给出N个整数。
```

输出描述:

```
输出包括两个整数，为给定N个数中的最大值与最小值。
```

示例1

##### 输入

```
5
1 2 3 4 5
3
3 7 8
```

##### 输出

```
5 1
8 3
```

---

## 代码

```c++
#include<iostream>
using namespace std;
int main(){
    int N;
    while(cin>>N)
    {
        int max=0,min=0,flag=N,test;
        while(flag--)
        {
            cin>>test;
            if(test>max) max=test;
            if(test<min) min=test;
        }
        cout<<max<<" "<<min<<endl;
    }
}
```



---

## 结语

新手签到题。