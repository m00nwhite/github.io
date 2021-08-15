---
git@gitee.com:sjdt/geekhall.cn.gitlayout: post
title:  "如何更改IDEA使用内存"
date:   2021-08-08 10:10:25 +0800
categories: IDEA
---

## 更改启动最大最小内存
在访达中找到IDEA，右键“显示包内容”，找到bin/idea.vmoptions，编辑如下内容修改最小内存和最大内存：
```
-Xms4096m
-Xmx8192m
```

也可以在IDEA的Help-> Edit Custom VM Options菜单中直接修改。


## 更改Maven的importer使用内存大小
![](https://yinyang.space/img/20210808_idea.png)

## 更改编译器内存大小
![](https://yinyang.space/img/20210808_idea1.png)

## 勾选Show memory indicator选项
![](https://yinyang.space/img/20210808_idea2.png)

这样在idea的右下角就可以看到当前内存的使用状况了，
重启IDEA，验证上面修改是否已经生效了

其他Pycharm等工具的修改方法基本相同。


