---
title: "Atak"
author: "bcalvino"
date: 2017-09-15T23:04:12-03:00
keywords: Hugo, Bitbucket, static, deploy, Cocoa EH, theme, foraTemer
authors: "bcalvino, jcalangro, aconselheiro, ffelix, hagemanto, ssurfer, unown"
---

UNDER CONSTRUCTION

Translated from english to Dothraki unig LingoJam and Fun Translations. Untranslated terms are highlighted for further work.

Jini jin _initial post_ she jin _page_, majin anha zin zin vo _sure_ fin me tikh tikh jin _blog_, jin _site_, jin _portal_, jin _service_, che ato oakah ann.  Ven _usual_, jin naqis qisi kikerosi jin _page_ ki _created_.  Jin _english-speaking_ chomak tikh ajjin _begin_ ma _awesome_ che _outstanding_ che eshna _such exaggerated adjectives_ mori allayafi.  Fin ei _minimalistic static page_ ajjin _fantastic_ arrek jin _whole world_ ajjin jin _homogeneous_ mesina ki _“extraordinary” things_ rek _ends_ ha _being incredibly_ flech.

Jin _page_ ajjin _static, meaning_ vo _processing_ she _server side.  Files_ hash _stored_ she _server_, vosma me ajjin jin _client side_ rek _interprets_ eyak.  Jin _recent trend_ she jin _model comes_ arrekoon _asset loading_ athdikar ma _minimal aesthetics_ rek _prefers_ jin _emphasis_ she chek-_kept typography_, _economical organization_ akka balanced vishiya.  Vo rek hazze ajjin vo room ha jin _vibrant_.  Anha zhorre eshna _[page][pag], made_ ma eshna marriya ha _static pages_, majin _looks_ allayafi jin rich _gum_.

Jin marryia anha _use_ gwe hash _composed_ ki [Hugo][hugo], jin _framework_ ha _building websites_, ven jin _static page generator_ ajjin _self-defined, written_ she elat, akka [Cocoa EH][cocoaeh], jin _ultra-minimalist_ aranikh ma clean akka _cool typography_ ha Hugo.  Anha _use_ jin Macbook pro _early_ 2011 ma 16 Gb ki RAM. Oma _excessive details_:

- Anha _installed_ Hugo ma [homebrew][brew], ato ki _package managers_ ha MacOS:

```ruby
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
$ brew install hugo
```

Anha chose jin aranikh arrekoon jin _[hundreds][themes] available_ akka _installed_ me ki _downloading_ jin _contents_ ki _repository into_ jin ```themes\cocoa-eh``` _folder_ mra jin _location_ anha _chose_ tat _house_ jin page code.  Ven anha _use_ jin [Git][git] _version control system_, anha _started_ jin git _repository_ kijinosi _folder_ finne anha _keep_ jin _page files_, majin _cloned_ jin aranikh ven jin _submodule_:

```git
$ cd pasta
$ git init
$ git submodule add https://github.com/fuegowolf/cocoa-eh-hugo-theme.git themes/cocoa-eh
```

Anha alikh jin _line_ tat ```config.toml``` _file_:

```bash
$ echo 'theme = cocoa-eh' >> config.toml
```
Jin _file store configuration parameters_ she ```toml``` _format_.  Jin aranikh _comes_ ma at _example_ rek laz tikh _customized_:

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

Irge _installing_ Hugo akka _downloading_ jin aranikh, disse lanat jin _code_:

```golang
$ hugo
$ hugo server
```

Irge _starting_ jin _server_, anha _pointed_ jin _browser_ tat http://localhost:1313 akka _page_ laz tikh _viewed_.

Vosma, anha stiil ki tat _implement_ jin _page_ she ale _web server_.  _Specifically_, anha _wanted_ tat _use_ [Bitbucket][bucket], akka Atlassian _code storage portal serves static content_ finne ```<user>.bitbucket.io``` _addresses_.  Tat jin, anha _created_ jin _repository_ she anna _Bitbucket account_, _naming_ me _according_ tat oleth _convention (the page_ hake).  Arrek anha _synchronized_ jin _source_ ma "master" _branch_ ki _page's local repository_:

```git
$ git remote add origin https://<usuário>@bitbucket.org/<usuário>/<usuário>.bitbucket.io.git
$ git push -u origin master
```

Ven _just_ jin _contents_ ki _public folder_ jif tikh _served_, anha _created_ jin sash _branch_ ki _repository_ ki _filtering_ jin _folder_ she qaf:

```git
$ git subtree split --branch deploy --prefix public/
$ git checkout deploy
$ git push -u origin deploy
```

Kijinosi, jin sash _branch_ ```deploy``` ki ray _been synchronized_ ma fas.  Jin nakhok _detail_ ki tat _configure_ jin _repository_ she Bitbucket ma rek jin _main branch_ tikh tikh ```deploy``` akka vo ``` master```.  Arrek anha _navigated_ tat ```https://<user>. bitbucket. io``` akka _checked_ jin _final result_.

[pag]: https://oncologiaped.gitlab.io/eventosadversos/
[hugo]: https://gohugo.io
[go]: https://golang.org
[cocoaeh]: https://themes.gohugo.io/cocoa-eh-hugo-theme/
[brew]: https://brew.sh
[temas]: https://themes.gohugo.io
[git]: https://git-scm.com
[bucket]: https://bitbucket.org
