---
title: "Primeira"
author: "bcalvino"
date: 2017-09-15T23:04:12-03:00
keywords: Hugo, Bitbucket, static, deploy, Cocoa EH, theme, foraTemer
authors: "bcalvino, jcalangro, aconselheiro, ffelix, hagemanto, ssurfer, unown"
---

Esta é a postagem inicial nesta página, que ainda não tenho certeza se será um blog, um sítio, um portal, um serviço,
ou coisa que o valha. Como é de praxe, um pouco sobre como a página foi criada. O pessoal anglófono agora entraria
com um _awesome_ ou _outstanding_ ou outros desses adjetivos exagerados que eles gostam. Se toda página estática minimalista for **fantástica**, então o mundo inteiro é uma sopa homogênea de coisas extraordinárias que acaba
por ser _incrivelmente monótona_.

A página é estática, ou seja, sem trabalho no lado do servidor. Os arquivos são armazenados no servidor, mas é o lado cliente que interpreta. A tendência recente a este modelo vem da velocidade de carregamento dos ativos e de uma estética do mínimo que prefere a ênfase na tipografia bem cuidada, na organização econômica e em cores equilibradas. Não que não haja espaço para o vibrante. Eu tenho uma outra [página][pag], feita com outra ferramenta para páginas estáticas, que parece um _chiclete_.

A ferramenta que usei aqui é composta pelo [Hugo][hugo], _uma estrutura de construção de páginas web_, como se autodefine este gerador de páginas estáticas escrito em [Go][go], e [Cocoa EH][cocoaeh], o tema ultraminimalista com _tipografia limpa e legal_ para Hugo. Uso um macbook pro early 2011 com 16 Gb de RAM. Sem excessivas minúncias:

- Instalei o Hugo com [Homebrew][brew], um dos gerenciadores de pacotes para MacOs:

```ruby
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
$ brew install hugo
```

Escolhi o tema entre as [centenas][temas] disponíveis e o instalei, baixando o conteúdo do repositório na pasta ```themes\cocoa-eh``` dentro do local que escolhi para a abrigar o código da página. Como uso o controlador de versões [Git][git], iniciei um repositório git na pasta onde mantenho os arquivos da página e, depois, clonei o tema como um submódulo:

```git
$ cd pasta
$ git init
$ git submodule add https://github.com/fuegowolf/cocoa-eh-hugo-theme.git themes/cocoa-eh
```

Adicionei esta linha no arquivo ```config.toml```:

```bash
$ echo 'theme = cocoa-eh' >> config.toml
```

Este arquivo armazena os parâmetros de configuração em formato ```toml```. O tema vem com um exemplo que pode ser personalizado:

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

Após instalar Hugo e baixar o tema, bastou rodar o código:

```golang
$ hugo
$ hugo server
```

Após iniciar o servidor, apontei o navegador para http://localhost:1313 e a página pôde ser visualizada.

Porém, ainda faltava a implementação da página em algum servidor. Especificamente, eu queria usar o [Bitbucket][bucket], e o portal de armazenamento de código da Atlassian serve conteúdo estático em endereços tipo ```<usuário>.bitbucket.io```. Para proceder a isso, criei um repositório em minha conta do Bitbucket, nomeando-o de acordo com a convenção acima (o nome da página). Em seguida, sincronizei a origem com o ramo _master_ do repositório local da página:

```git
$ git remote add origin https://<usuário>@bitbucket.org/<usuário>/<usuário>.bitbucket.io.git
$ git push -u origin master
```

Como _apenas o conteúdo da pasta **public**_ deve ser servido, criei um novo ramo do repositório, filtrando a pasta em questão:

```git
$ git subtree split --branch deploy --prefix public/
$ git checkout deploy
$ git push -u origin deploy
```

Dessa forma, o novo ramo ```deploy``` já fora sincronizado com a nuvem. O último detalhe foi configurar o repositório no Bitbucket para que o ramo principal fosse ```deploy``` e não ```master```. Em seguida, naveguei para ```https://<usuário>.bitbucket.io``` e verifiquei o resultado final, que é este.

[pag]: https://oncologiaped.gitlab.io/eventosadversos/
[hugo]: https://gohugo.io
[go]: https://golang.org
[cocoaeh]: https://themes.gohugo.io/cocoa-eh-hugo-theme/
[brew]: https://brew.sh
[temas]: https://themes.gohugo.io
[git]: https://git-scm.com
[bucket]: https://bitbucket.org
