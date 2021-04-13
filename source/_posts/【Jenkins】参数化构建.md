---
title: 参数化构建
description: '-'
tags:
  - Jenkins
abbrlink: cbb3db44
date: 2021-03-28 18:24:13
---



为了方便看到全部配置，这里直接看截图就好了。

![172.20.0.80](http://blog.cdn.ionluo.cn/blog/FireShot Capture 164 - 测试站点 Config [Jenkins] - 172.20.0.80.png)





构建代码：

```bash
#!/bin/bash

set -e

if [ $force == 'no' ];then
  if [ ! $GIT_PREVIOUS_SUCCESSFUL_COMMIT ];then
      echo "GIT_PREVIOUS_SUCCESSFUL_COMMIT is not exists."
      exit 0
  else
      echo "GIT_COMMIT=[$GIT_COMMIT],GIT_PREVIOUS_SUCCESSFUL_COMMIT=[$GIT_PREVIOUS_SUCCESSFUL_COMMIT]"
      if [ $GIT_PREVIOUS_SUCCESSFUL_COMMIT == $GIT_COMMIT ];then
          echo "GIT_COMMIT is equals to GIT_PREVIOUS_SUCCESSFUL_COMMIT,skip build."
          exit -1
      else
          echo "GIT_COMMIT is not equals to GIT_PREVIOUS_SUCCESSFUL_COMMIT"
      fi
  fi
  
fi


project_full_name=''

case $project_name in 
	web)
		project_full_name='web_xxx_net_1';;
    japan)
		project_full_name='web_xxx_net_2';;
    apac)
		project_full_name='web_xxx_net_3';;
    korea)
		project_full_name='web_xxx_net_4';;
	*)	
		echo "Params project_name is neccesary!"
  		exit 0;;
esac

echo -e project_full_name: ${project_full_name}


cd /var/www/$project_full_name/

result=$(git pull)
git checkout master
# git reset --hard origin/master

source /var/www/env/$project_full_name/bin/activate

# 费时操作根据判断是否执行
if [ $(echo $result | grep -c "requirements.txt") -ge 1 ]; then
    pip install -r ./requirements.txt
fi

if [ $(echo $result | grep -c "app/migrations") -ge 1 ]; then
    python manage.py migrate --database=default --noinput
fi

if [ $(echo $result | grep -c "locale/") -ge 1 ]; then
    python manage.py compilemessages
fi

if [ $(echo $result | grep -c -E "\.js|\.css|\.less") -ge 1 ]; then
    cd app/static/
    ls -dt ./build/*/ | tail -n +5 | xargs rm -rf
    grunt build
    git rev-parse --short FETCH_HEAD > ../../VER
    cd ../../
fi

supervisorctl restart $project_name
python manage.py clear_cache
deactivate

exit 0
```





我的插件在这里补充下：

> 主要核心插件有：[Git](https://plugins.jenkins.io/git) [
> GitLab Plugin](https://plugins.jenkins.io/gitlab-plugin) [Pipeline](https://plugins.jenkins.io/workflow-aggregator) [
> Publish Over SSH](https://plugins.jenkins.io/publish-over-ssh) [SSH Build Agents](https://plugins.jenkins.io/ssh-slaves)