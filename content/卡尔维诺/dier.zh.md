---
title: "Bitbucket的持续集成"
author: "bcalvino"
date: 2017-11-01T23:04:12-03:00
keywords: Hugo, Bitbucket, git, deploy, 的持续集成, 主题, 特梅尔之外
---

在这篇第二篇文章中，我将描述与Bitbucket的持续集成过程，这使我可以直接在浏览器界面上编写本文。Bitbucket是Atlassian企业的版本控制存储库，它拥有自己的持续集成解决方案——_bitbucket-pipelines_。它的配置基本上与通常一样，使用一个YAML格式的文件。

_bitbucket-pipelines.yaml_ 文件的基本结构很简单：

```yaml
image: andthensome/alpine-hugo-git-bash

pipelines:
  branches:
    master:
      - step:
          script:
            - hugo
```

我选择了这个特殊的Docker镜像，因为它基于Alpine Linux（因此非常小，只有基本的5 MB），并且安装了Hugo和Git。这个镜像的作者专门为Wercker的持续集成创建了它。在我的情况下，我还需要添加一些额外的东西：

```yaml
pipelines:
  branches:
    master:
      - step:
          script:
            - apk update
            - apk add openssh
            - git submodule update
            - hugo
            - git config user.name <username>
            - git config user.email <user@mail.com>
            - git add . && git commit -m "new update"
            - git filter-branch --prune-empty --subdirectory-filter public
            - git checkout -b deploy
            - git push -f origin deploy
```

Apk是Alpine的软件包管理器，我需要 _openssh_，因为 _bitbucket.pipelines_ 不会在免费帐户中保存集成的构件（除非是在下载区域）。必须通过ssh将更新的文件同步到远程（这也是为什么需要Git）。此外，我需要运行`git submodule update`，因为 _Cocoa EH_ 主题是我存储库中的一个子模块。

由于Hugo将生成的页面文件写入_public_目录，我使用`git filter-branch --subdirectory-filter`过滤文件，并创建了一个名为 _deploy_ 的新分支，然后切换到该分支。远程仓库已经有一个 _deploy_ 分支，但 _bitbucket.pipelines_ 只克隆 _master_ 分支。因此，还需要使用`git push -f`选项进行推送（强制推送）。

另外，还需要在 _pipelines_ 菜单和_设置_中生成一对加密密钥。公钥可以通过轻触进行复制。只需将密钥添加到Bitbucket配置文件的安全设置中，一切就会像从源存储库提交到远程一样工作。

非常，非常简单！我用平板电脑完成了所有操作！整个集成过程非常快速，少于15秒！

附注：对我来说唯一的限制是Alpine Linux没有配置区域设置的选项，这使我无法为页面的标题添加重音符号。但这只是一个小问题。文章中的符号和重音符号都被保留了下来。
