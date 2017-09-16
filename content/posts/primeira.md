---
title: "Primeira"
date: 2017-09-15T23:04:12-03:00
draft: true
---

Esta é a postagem inicial nesta página, que ainda não tenho certeza se será um blog, um sítio, um portal, um serviço,
ou coisa que o valha. Como é de praxe, um pouco sobre como a página foi criada. O pessoal anglófono agora entraria
com um _awesome_ ou _outstanding_ ou outros desses adjetivos exagerados que eles gostam. Se toda página estática minimalista for **fantástica**, então o mundo inteiro é uma sopa homogênea de coisas extraordinárias que acaba
por ser _incrivelmente monótona_.

A página é estática, ou seja, sem trabalho no lado do servidor. Os arquivos são armazenados no servidor, mas é o lado cliente que interpreta. A tendência recente a este modelo vem da velocidade de carregamento dos ativos e de uma estética do mínimo que prefere a ênfase na tipografia bem cuidada, na organização econômica e em cores equilibradas. Não que não haja espaço para o vibrante. Eu tenho uma outra página, feita com outra ferramenta para páginas estáticas, que parece um _chiclete_.

A ferramenta que usei aqui é composta pelo [Hugo][hugo], _uma estrutura de construção de páginas web_, como se autodefine este gerador de páginas estáticas escrito em [Go][go], e [Cocoa EH][cocoaeh], o tema ultraminimalista com _tipografia limpa e legal_ para Hugo. Uso um macbook pro early 2011 com 16 Gb de RAM. Sem excessivas minúncias:

- Instalei o Hugo com [Homebrew][brew], um dos manipuladores de pacotes para MacOs:

```ruby
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
$ brew install hugo
```

Escolhi o tema entre a as [centenas][temas] disponíveis e o instalei, baixando o conteúdo do repositório na pasta ```themes\cocoa-eh``` dentro do local que escolhi para a abrigar o código da página. Como uso o controlador de versões [Git][git], 

[hugo]: https://gohugo.io
[go]: https://golang.org
[cocoaeh]: https://themes.gohugo.io/cocoa-eh-hugo-theme/
[brew]: https://brew.sh
[temas]: https://themes.gohugo.io
[git]: https://git-scm.com
