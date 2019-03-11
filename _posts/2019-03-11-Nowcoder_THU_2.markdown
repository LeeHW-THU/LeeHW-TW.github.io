---
layout:     post
title:      "「牛客网」——「清华机考」—— 反序输出"
subtitle:   " 记录打码。 "
date:       2019-03-11 
author:     "LeeHW"
header-img: "img/post-bg-digital-native.jpg"
catalog: true
tags:
    - Nowcoder_THU
---

## 题目

输入任意4个字符(如：abcd)， 并按反序输出(如：dcba)

##### 输入描述:

```
题目可能包含多组用例，每组用例占一行，包含4个任意的字符。
```

##### 输出描述:

```
对于每组输入,请输出一行反序后的字符串。
具体可见样例。
```

##### 示例1

输入

```
Upin
cvYj
WJpw
cXOA
```

输出

```
nipU
jYvc
wpJW
AOXc
```

---

## 代码

```c++
#include<iostream>
using namespace std;
int main()
{
    string x;
    while(cin>>x)//不停的检测输入
    {
        for(int i=x.size()-1;i>=0;i--)//直接倒序输出
        {
            cout<<x[i];
        }
        cout<<endl;//控制换行
    }
}
```



---

## 结语

新手签到题。