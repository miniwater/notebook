# Git
用sourcetree管理

配置信息
```
git config --list
```

配置用户信息
```
$ git config --global user.name "runoob"
$ git config --global user.email test@runoob.com
```

查询配置信息
```
git config user.name
```

## 已有
克隆
```
git clone <repo> <directory>
```

## 创建

初始化
```
git init
```

## 常用命令

提交暂存文件
```
git add .
```

撤销暂存
```git
git reset HEAD
```

撤销滚暂存前的修改
```
git checkout .
```

撤销全部修改（包括暂存）
```
git checkout HEAD .
```

提交暂存文件
```
git commit -m '备注'
```

当前状态
```
git status
```

比较暂存区和工作区差异
```
git diff
```

回退
```
git reset HEAD^            # 回退所有内容到上一个版本  
git reset HEAD^ hello.php  # 回退 hello.php 文件的版本到上一个版本  
git reset  052e           # 回退到指定版本
```

提交历史
```
git log
```

创建分支
```
git branch (branchname)
```

切换分支
```
git checkout (branchname)
```

合并release分支
```
git merge release
```

标签
```
git tag -a v1.0 
```
:wq保存退出
:qa不保存退出



推
```
git push
```
拉
```
git pull
```






```
git remote -v

git remote rm origin

git remote add origin gitlab@gitlab.

git pull origin master

git branch --set-upstream-to=origin/master master

git config --global credential.helper store
 
git pull

```


## Gitlab

### docker-compose环境搭建

docker-compose.yml
```
version: '3.6'
services:
  gitlab:
    image: 'gitlab/gitlab-ee:latest'
    restart: unless-stopped
    hostname: 'git.xqdash.com'
    environment:
      GITLAB_OMNIBUS_CONFIG: |
        external_url 'https://git.xqdash.com'
        gitlab_rails['gitlab_shell_ssh_port'] = 1013
        nginx['listen_port'] = 80
        nginx['listen_https'] = false
    ports:
      - '1011:80'
      - '1013:22'
    volumes:
      - ./config:/etc/gitlab
      - ./logs:/var/log/gitlab
      - ./data:/var/opt/gitlab
    shm_size: '256m'
    networks:
      - gitlab
    
  gitlab-runner:
    image: 'gitlab/gitlab-runner:latest'
    restart: unless-stopped
    depends_on:
      - gitlab
    volumes:
      - ./runner/config:/etc/gitlab-runner
      - /var/run/docker.sock:/var/run/docker.sock
    networks:
      - gitlab
      
networks:
  gitlab:
```
busybox:latest



fdEPrgJFPzaEonKhK7sksPSL5aU/3OcTSYnZe3RKg8k=

iZRBDX7xtNZVt6hYmYdu

ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDt2AXwxDelMsaUuP9rZqiOOcYcjKY1k+uq1xAr6jfFPHX864qbXBf7krt7OsxOC8+uE0xgGeRYUPptxowiZ29HuBzgypIXSM0GKFVnc66pE4Wjkm5POoawoiMg+fatmiXJKD5XSDq0uhw4KWS3OHqbFNol0l3VhEc66/a2d3dqlPV1pPepYf41ncrN5ebNN5kk/nol3JnUC04HbfhsHXEU1kZxvmnt9Mk4kp9zApAMMmVU5EufWKZRucqFmWBBOgEIpcrNcanvM0uOgGBNd0YJttlt8ZRXMw4C/lzIeZaXeJRlN6evnUiClF1hZIglnbwPJL5aaaLSybBsRozUDpMmaIkI8p1fR8qS3sSDODBPJ9uW47Qoa39UKZuhOaJVn8Xo0lUiRRCBCjxkChkhp/1Bskfc2rStwsSKVwIOkzLC0au0mtDtN5lJo3nhPWot3hd51FGhR12DGCS2XfhjImfTtGiu5Wym1/AMmDbGioHVSLLoaOaSDxV1/6z0fGSKsTg9geKKL6uZRivjD1Iz09mgaUCzMRMakPAlI9NqcPQwvJdspZbDKGGKt4PoD5sLQT18CZUVKV6oQgoiFAtveuTsUgO2w2EvTfY+0e9GQiacozj2TOYNKkrEH5nFMHXGoH160XTva9Uff1J45DjXMTBrz+TPEyIVsESBACUiFmjZ4Q==