---
title: "Première"
author: "collective"
date: 2017-09-15T23:04:12-03:00
keywords: Hugo, Bitbucket, static, deploy, Cocoa EH, theme, hots Temer
---

Ceci est le premier article sur cette page, dont je ne suis pas encore sûr qu'elle sera un blog, un site, un portail, un service ou autre chose du genre. Comme d'habitude, voici un bref aperçu de la façon dont la page a été créée. Les anglophones diraient maintenant que c'est *impressionnant* ou *exceptionnel* ou d'autres adjectifs exagérés qu'ils aiment utiliser. Si chaque page statique minimaliste est **fantastique**, alors le monde entier est une soupe homogène de choses extraordinaires qui finit par être *incroyablement monotone*.

La page est statique, c'est-à-dire sans travail côté serveur. Les fichiers sont stockés sur le serveur, mais c'est le côté client qui les interprète. Cette tendance récente à ce modèle vient de la vitesse de chargement des ressources et d'une esthétique minimaliste qui privilégie la mise en valeur d'une typographie soignée, d'une organisation économique et de couleurs équilibrées. Ce n'est pas qu'il n'y a pas de place pour le dynamique. J'en ai fait une autre [page][pag] créée avec un autre outil pour les pages statiques, qui ressemble à un *chewing-gum*.

L'outil que j'ai utilisé ici est composé de [Hugo][hugo], *un générateur de sites statiques*, comme le décrit lui-même ce générateur de pages statiques écrit en [Go][go], et [Cocoa EH][cocoaeh], un thème ultraminimaliste avec une *typographie propre et sympa* pour Hugo. J'utilise un MacBook Pro début 2011 avec 16 Go de RAM. Sans entrer dans les détails excessifs:

- J'ai installé Hugo avec [Homebrew][brew], l'un des gestionnaires de paquets pour macOS:

```ruby
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
$ brew install hugo
```

J'ai choisi le thème parmi les [centaines][temas] disponibles et l'ai installé en téléchargeant le contenu du référentiel dans le dossier ```themes\cocoa-eh``` à l'emplacement que j'ai choisi pour héberger le code de la page. Étant donné que j'utilise le gestionnaire de versions [Git][git], j'ai initialisé un référentiel Git dans le dossier où je stocke les fichiers de la page, puis j'ai cloné le thème en tant que sous-module:

```git
$ cd pasta
$ git init
$ git submodule add https://github.com/fuegowolf/cocoa-eh-hugo-theme.git themes/cocoa-eh
```

J'ai ajouté cette ligne dans le fichier ```config.toml```:

```bash
$ echo 'theme = cocoa-eh' >> config.toml
```

Ce fichier stocke les paramètres de configuration au format ```toml```. Le thème est livré avec un exemple qui peut être personnalisé:

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

Après avoir installé Hugo et téléchargé le thème, il suffit d'exécuter le code:

```golang
$ hugo
$ hugo server
```

Après avoir démarré le serveur, j'ai ouvert le navigateur sur http://localhost:1313 et la page était visible.

Cependant, il manquait encore l'implémentation de la page sur un serveur. Plus précisément, je voulais utiliser [Bitbucket][bucket], et le service d'hébergement de code d'Atlassian sert du contenu statique à des adresses de type ```<utilisateur>.bitbucket.io```. Pour ce faire, j'ai créé un référentiel dans mon compte Bitbucket, en lui donnant le nom correspondant à la convention susmentionnée (le nom de la page). Ensuite, j'ai synchronisé l'origine avec la branche _master_ du référentiel local de la page:

```git
$ git remote add origin https://<usuário>@bitbucket.org/<usuário>/<usuário>.bitbucket.io.git
$ git push -u origin master
```

Étant donné que _seul le contenu du dossier **public**_ doit être servi, j'ai créé une nouvelle branche du référentiel en filtrant le dossier en question:

```git
$ git subtree split --branch deploy --prefix public/
$ git checkout deploy
$ git push -u origin deploy
```

De cette manière, la nouvelle branche ```deploy``` a déjà été synchronisée avec le cloud. Le dernier détail a été de configurer le référentiel dans Bitbucket pour que la branche principale soit ```deploy``` et non ```master```. Ensuite, j'ai navigué vers ```https://<utilisateur>.bitbucket.io``` et vérifié le résultat final, qui est celui-ci.

[pag]: https://oncologiaped.gitlab.io/eventosadversos/
[hugo]: https://gohugo.io
[go]: https://golang.org
[cocoaeh]: https://themes.gohugo.io/cocoa-eh-hugo-theme/
[brew]: https://brew.sh
[temas]: https://themes.gohugo.io
[git]: https://git-scm.com
[bucket]: https://bitbucket.org
