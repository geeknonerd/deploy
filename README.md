
# deploy

  简化版Shell部署脚本,可直接在服务器上执行.

__[Englist Document](README.en.md)__

__该项目Fork自 [deploy](https://github.com/visionmedia/deploy), 原项目将Shell脚本放置的本地电脑, 通过SSH远程执行命令; 本脚本是放置在远程服务器上直接执行__

## 安装

    $ make install

  访问原始 [wiki](https://github.com/visionmedia/deploy/wiki) 以获取更多使用信息。

## 使用

      Usage: deploy [options] <env> [command]

      Options:

        -C, --chdir <path>   change the working directory to <path>
        -c, --config <path>  set config path. defaults to ./deploy.conf
        -T, --no-tests       ignore test hook
        -V, --version        output program version
        -h, --help           output help information

      Commands:

        setup                run remote setup commands
        revert [n]           revert to [n]th last deployment or 1
        config [key]         output config file or [key]
        curr[ent]            output current release commit
        prev[ious]           output previous release commit
        exec|run <cmd>       execute the given <cmd>
        list                 list previous deploy commits
        [ref]                deploy to [ref], the 'ref' setting, or latest tag

## 配置

 默认情况下，`deploy(1)` 将查找 _./deploy.conf_，由一个或多个环境、`[stage]`、`[production]` 等组成，然后是指令。

    [stage]
    repo git@github.com:visionmedia/express.git
    path /var/www/myapp.com
    ref origin/master
    post-deploy /var/www/myapp.com/update.sh

## 指令

### ref (可选的)

  被指定时，__HEAD__ 重置为 ref。在部署生产环境时，通常不会使用它，因为 `deploy(1)` 默认使用最新的标签，但是这对于暂存环境很有用，如下所示，其中 __HEAD__ 已更新并设置为开发分支。

        ref origin/develop

### repo

  要克隆的 GIT 存储库。

       repo git@github.com:visionmedia/express.git

### path

  部署路径。

        path /var/www/myapp.com

## Hooks

  所有 hooks 都是任意命令，相对于`path/current`执行，`pre-deploy` 用于先前部署和 `post-deploy`用于新部署后。当然你也可以指定绝对路径。

### pre-deploy

      pre-deploy ./bin/something

### post-deploy

      post-deploy ./bin/restart

### test

  测试命令用于 `post-deploy` 执行后。如果这命令失败，`deploy(1)` 将尝试恢复到上一个部署，并忽略当前测试，它们被认为已经正确运行。

      test ./something
