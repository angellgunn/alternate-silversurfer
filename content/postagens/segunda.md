---
title: "Segunda"
date: 2017-11-01T23:04:12-03:00
keywords: Hugo, Bitbucket, git, deploy, continuous integration, theme, foraTemer
---

Nesta segunda postagem, vou descrever o processo de integração contínua com o Bitbucket que permite, por exemplo, que eu esteja escrevendo este texto diretamente na interface do navegador de internet. O Bitbucket, repositório de controle de versionamento da gigante corporativa Atlassian, tem uma solução própria de integração contínua, o _bitbucket-pipelines_. Sua configuração é basicamente a usual, usando um arquivo em formato _yaml_.

A estrutura básica do arquivo _bitbucket-pipelines.yaml_ é simples:

```yaml
image: andthensome/alpine-hugo-git-bash

pipelines:
  branches:
    master:
      - step:
          script:
            - hugo
```

Escolhi esta imagem de docker em especial por ser baseada no Alpine Linux (e ter, assim, um tamanho mínimo, 5 MB básicos) e ter Hugo e Git instalados. O autor da imagem a criou justamente para integração contínua com o Wercker. No meu caso, eu tive que acrescentar algumas coisinhas a mais:

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

Apk é o gerenciador de pacotes do Alpine, e eu preciso do _openssh_ porque o _bitbucket.pipelines_ não salva artefatos da integração em contas gratuitas (exceto na área de download). É necessário sincronizar os arquivos atualizados com o remoto via ssh (daí também a necessidade de Git). Além disso, preciso dar um `git submodule update` pois o tema _Cocoa EH_ é um submódulo dentro do meu repositório.

Como o Hugo escreve os arquivos gerados da página no diretório _public_, filtrei os arquivos usando `git filter-branch --subdirectory-filter` e criei um novo _branch_ chamado `deploy`, fazendo _checkout_ nele em seguida. O remoto já tem um ramo `deploy`, porém o _bitbucket.pipelines_ somente clona o `master`. Por isso, também, torna-se necessário dar _push_ com a opção `git push -f` (_force_).

No mais, foi preciso gerar um par de chaves criptográficas no menu _pipelines_ e, em seguida, _configurações_. A chave pública pode ser copiada com um toque. Basta adicionar a chave nas configuracões de segurança do perfil do Bitbucket e tudo funcionará como se fosse um _commit_ a partir de um repositório origem para o remoto.

_Très, très simple!_ Fiz tudo de um tablet! E a integração ficou rápida como o raio, dura menos de 15 segundos!

P.S.: a única limitação para mim foi que o Alpine Linux não tem opção de configurar _locales_, e isso me impediu de acentuar o título da página. Mas foi somente isso. Os símbolos e acentuação dentro do texto das postagens são preservados.