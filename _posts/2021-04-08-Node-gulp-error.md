---
layout: post
title:  "gulp常见问题及解决方法"
date:   2021-03-14 06:57:25 +0800
categories: gulp
---
* 目录
{:toc}

## 执行gulp的时候报错：
```bash
➜  front git:(main) ✗ gulp
Error: Node Sass does not yet support your current environment: OS X 64-bit with Unsupported runtime (88)
For more information on which environments are supported please see:
https://github.com/sass/node-sass/releases/tag/v4.14.1
    at module.exports (/Users/yiny/Sites/geekhall.cn/front/node_modules/node-sass/lib/binding.js:13:13)
    at Object.<anonymous> (/Users/yiny/Sites/geekhall.cn/front/node_modules/node-sass/lib/index.js:14:35)
    at Module._compile (node:internal/modules/cjs/loader:1108:14)
    at Object.Module._extensions..js (node:internal/modules/cjs/loader:1137:10)
    at Module.load (node:internal/modules/cjs/loader:973:32)
    at Function.Module._load (node:internal/modules/cjs/loader:813:14)
    at Module.require (node:internal/modules/cjs/loader:997:19)
    at require (node:internal/modules/cjs/helpers:92:18)
    at Object.<anonymous> (/Users/yiny/Sites/geekhall.cn/front/node_modules/gulp-sass/index.js:166:21)
    at Module._compile (node:internal/modules/cjs/loader:1108:14)
```

![](https://yinyang.space/img/20210408_node_err.png)

不知道具体原因，但是下面的命令可以修复：
```bash
npm rebuild node-sass
```