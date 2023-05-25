---
title: "第一的"
author: "bcalvino"
date: 2017-09-15T23:04:12-03:00
keywords: Hugo, Bitbucket, static, deploy, Cocoa EH, theme, foraTemer
---

这是本页面的初始文章，我还不确定它会是一个博客、一个网站、一个门户网站、一个服务，还是其他什么东西。按照惯例，简要介绍一下页面的创建过程。对于讲英语的人来说，他们可能会使用“令人惊叹”、“出色”或其他夸张的形容词来形容。如果每个简约的静态页面都是**奇妙的**，那么整个世界就是一碗由各种非凡事物组成的均质汤，最终变得*令人难以置信的单调*。

该页面是静态的，也就是说，服务器端没有工作。文件存储在服务器上，但是由客户端解释。最近这种模式的趋势源于资源加载速度和追求极简美感，更注重精心设计的排版、经济的组织和平衡的颜色。这并不意味着没有空间来展现生机勃勃的元素。我有另一个用另一种静态页面工具制作的[页面][pag]，看起来像一块口香糖。

我在这里使用的工具包括[Hugo][hugo]，这是一款用[Go][go]编写的静态网页生成器，它自称为*网页构建框架*，以及[Cocoa EH][cocoaeh]，这是一个超迷你主题，适用于Hugo的*干净而炫酷的排版*。我使用一台2011年早期的MacBook Pro，配备16GB的内存。不再赘述细节：

- 我使用[Homebrew][brew]在MacOS上安装了Hugo，它是MacOS上的一个软件包管理器：

```ruby
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
$ brew install hugo
```

我从可用的[数百个][temas]主题中选择了一个主题，并将其安装在我选择用于托管页面代码的位置的```themes\cocoa-eh```文件夹中。由于我使用版本控制系统[Git][git]，我在存储页面文件的文件夹中初始化了一个Git仓库，然后将主题作为子模块进行了克隆：

```git
$ cd pasta
$ git init
$ git submodule add https://github.com/fuegowolf/cocoa-eh-hugo-theme.git themes/cocoa-eh
```

我在```config.toml```文件中添加了以下行：

```bash
$ echo 'theme = cocoa-eh' >> config.toml
```

该文件以```toml```格式存储配置参数。主题附带了一个示例，可以进行自定义：

```toml
baseurl = "https://example.com/"
theme = "cocoa-eh"
builddrafts = true
canonifyurls = true
contentdir = "content"
languageCode = "en-US"
layoutdir = "layouts"
publishdir = "public"
author = "Alexis Tacnet"
title = "Cocoa Enhanced"
disqusshortname = ""
pluralizelisttitles = false

[permalinks]
blog = "blog/:slug/"

[params]
dateform = "Jan 2, 2006"
dateformfull = "Mon Jan 2 2006 15:04:05 MST"
description = "Example blog"
copyright = "Copyright © 2015 Nishanth Shanmugham"
# copyrightUrl = "https://creativecommons.org/licenses/by-sa/4.0/"
logofile = "img/logo.png"
faviconfile = "img/logo.png"
highlightjs = true
progressively = true
share = true
latestpostcount = 5
github = "example"
email = "you@example.com"
linkedin = "john-example-aa80ue8è"
twitter = "example"
facebook = "facebook_id"
social_banner = "img/banner.png"
usesmallsummarycard = true
posts_navigation = true
# issoHost = "comments.domain.tld:1234"
# githubRepo = "githubUsername/repositoryName"
small_banner_logo = false

[params.colors]
identifier = "#527fc1f"
identifier_dark = "#1a3152"
trivial = "#6a7a8b"
foreground = "#181d2a"
background = "#f9f9f9"
background_dark = "#282a36"
code = "#87a5d2"
type = "#97d28b"
special = "#ffcb8d"
value = "#96c2d7"
statement = "#ff8e91"
```

安装Hugo并下载主题后，只需运行以下代码：

```golang
$ hugo
$ hugo server
```

启动服务器后，我将浏览器指向http://localhost:1313，即可查看页面。

然而，还缺少将页面部署到服务器的步骤。具体来说，我想使用[Bitbucket][bucket]，Atlassian的代码存储门户以```<用户名>.bitbucket.io```的形式提供静态内容。为此，我在Bitbucket账户中创建了一个仓库，根据上述约定给它命名（页面的名称）。然后，我将源与页面本地仓库的```master```分支同步：

```git
$ git remote add origin https://<usuário>@bitbucket.org/<usuário>/<usuário>.bitbucket.io.git
$ git push -u origin master
```

由于只有 **public** 文件夹的内容需要提供，我在仓库中创建了一个新分支，只包含该文件夹的内容：

```git
$ git subtree split --branch deploy --prefix public/
$ git checkout deploy
$ git push -u origin deploy
```

这样，新的```deploy```分支已经与云端同步。最后一个细节是在Bitbucket中配置仓库，使主要分支为```deploy```而不是```master```。然后，我导航到```https://<用户名>.bitbucket.io```，并验证了最终的结果，即此页面。

[pag]: https://oncologiaped.gitlab.io/eventosadversos/
[hugo]: https://gohugo.io
[go]: https://golang.org
[cocoaeh]: https://themes.gohugo.io/cocoa-eh-hugo-theme/
[brew]: https://brew.sh
[temas]: https://themes.gohugo.io
[git]: https://git-scm.com
[bucket]: https://bitbucket.org
