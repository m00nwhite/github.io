---
layout: post
title:  "同时提交Github和Gitee"
date:   2021-03-14 23:57:25 +0800
categories: github gitee git
---

# 同时提交github和gitee



示例代码

```bash
# 1 查看仓库信息
git remote -v

# 2 删除原有远程仓库，然后添加两个远程仓库
git remote rm origin
git remote add github git@github.com:geekhall/geekhall.cn.git
git remote add gitee git@gitee.com:sjdt/geekhall.cn.git

# 2 拉代码，合并两个仓库的历史记录
git pull gitee main --allow-unrelated-histories
git pull github main --allow-unrelated-histories

# 3 设置上游分支
git push --set-upstream github main
git push --set-upstream gitee main

# 3 push 代码
git push gitee main
git push github main


```





## 提交脚本

```bash
usage()
{
    echo "使用方法："
    echo "commit.sh"
    echo " or"
    echo "commit.sh 提交注释"
    exit 2
}

if [ $# -eq 1 ]; then
    comment=$1
else
    comment=`date +'%Y%m%d-%H%M%S'`
fi

git add .
git commit -m "$comment"
git push github
git push gitee
```





远端自动部署脚本

```bash
cd /usr/share/nginx/html/geekhall.cn
git pull
workon mwd
rm -rf static_dist
python manage.py collectstatic
deactivate
./stopsvr.sh	
./startuwsgi.sh
nginx -s stop
nginx

```

