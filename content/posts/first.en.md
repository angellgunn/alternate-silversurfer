---
title: "First"
author: "bcalvino"
date: 2017-09-15T23:04:12-03:00
keywords: Hugo, Bitbucket, static, deploy, Cocoa EH, theme, outTemer
---

This is the initial post on this page, which I'm still not sure if it will be a blog, a site, a portal, a service,
or something worth it. As usual, a little about how the page was created. The English-speaking people would now begin
with _awesome_ or _outstanding_ or other such exaggerated adjectives they like. If every minimalistic static page is **fantastic** then the whole world is a homogeneous soup of "extraordinary" things that ends
for being _incredibly dull_.

This page is static, meaning no processing on the server side. Files are stored on the server, but it is the client side that interprets them. The recent trend in this model comes from asset loading speed and minimal aesthetics that prefers the emphasis on well-kept typography, economical organization and balanced color. Not that there is no room for the vibrant. I have another [page][pag], made with another tool for static pages, which looks like a _bubble gum_.

The tools I use here are composed of [Hugo][hugo], _a framework for building websites_, as this static page generator is self-defined, written in [Go][go], and [Cocoa EH][cocoaeh], the ultra-minimalist theme with _clean and cool typography_ for Hugo. I use a macbook pro early 2011 with 16 Gb of RAM. Without excessive details:

- I installed Hugo with [Homebrew][brew], one of the package managers for MacOs:

```ruby
$ ruby -e "$ (curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
$ brew install hugo
```

I chose a theme from the [hundreds][themes] available and installed it by downloading the contents of the repository into the ```themes\cocoa-eh``` folder within the location I chose to house the page code. As I use the [Git][git] version control system, I started a git repository in the folder where I keep the page files, and then cloned the theme as a submodule:

```git
$ cd folder
$ git init
$ git submodule add https://github.com/fuegowolf/cocoa-eh-hugo-theme.git themes / cocoa-eh
```

I added this line to the ```config.toml``` file:

```bash
$ echo 'theme = cocoa-eh' >> config.toml
```

This file stores configuration parameters in ```toml``` format. The theme comes with an example that can be customized:

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
# thisHost = "comments.domain.tld: 1234"
# githubRepo = "githubUsername/repositoryName"
small_banner_logo = false

[params.colors]
identifier = "# 527fc1f"
identifier_dark = "# 1a3152"
trivial = "# 6a7a8b"
foreground = "# 181d2a"
background = "# f9f9f9"
background_dark = "# 282a36"
code = "# 87a5d2"
type = "# 97d28b"
special = "# ffcb8d"
value = "# 96c2d7"
statement = "# ff8e91"
```

After installing Hugo and downloading the theme, just run the code:

```golang
$ hugo
$ hugo server
```

After starting the server, I pointed the browser to http://localhost:1313 and the page could be viewed.

However, I stiil had to implement the page in some web server. Specifically, I wanted to use [Bitbucket][bucket], and the Atlassian code storage portal serves static content at ```<user> .bitbucket.io``` addresses. To do this, I created a repository in my Bitbucket account, naming it according to the above convention (the page name). Then I synchronized the source with the _master_ branch of the page's local repository:

```git
$ git remote add origin https://<user>@bitbucket.org/<user>/<user>.bitbucket.io.git
$ git push -u origin master
```

As _just the contents of the ** public ** folder should be served, I created a new branch of the repository by filtering the folder in question:

```git
$ git subtree split --branch deploy --prefix public /
$ git checkout deploy
$ git push -u origin deploy
```

This way, the new branch ```deploy``` had already been synchronized with the cloud. The last detail was to configure the repository in Bitbucket so that the main branch would be ```deploy``` and not ``` master```. Then I navigated to ```https://<user>.bitbucket.io``` and checked the final result.

[pag]: https://oncologiaped.gitlab.io/eventosadversos/
[hugo]: https://gohugo.io
[go]: https://golang.org
[cocoaeh]: https://themes.gohugo.io/cocoa-eh-hugo-theme/
[brew]: https://brew.sh
[temas]: https://themes.gohugo.io
[git]: https://git-scm.com
[bucket]: https://bitbucket.org
