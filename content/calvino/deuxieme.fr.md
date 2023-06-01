---
title: "Intégration continue avec Bitbucket"
author: "bcalvino"
date: 2017-11-01T23:04:12-03:00
keywords: Hugo, Bitbucket, git, deploy, intégration continue, thème, hors Temer
---

Dans ce deuxième billet, je vais décrire le processus d'intégration continue avec Bitbucket, qui me permet, par exemple, d'écrire ce texte directement dans l'interface du navigateur web. Bitbucket, le référentiel de contrôle de version de la grande entreprise Atlassian, dispose de sa propre solution d'intégration continue, _bitbucket-pipelines_. Sa configuration est essentiellement la même que d'habitude, en utilisant un fichier au format YAML.

La structure de base du fichier _bitbucket-pipelines.yaml_ est simple:

```yaml
image: andthensome/alpine-hugo-git-bash

pipelines:
  branches:
    master:
      - step:
          script:
            - hugo
```

J'ai choisi cette image Docker en particulier car elle est basée sur Alpine Linux (et donc de taille minimale, seulement 5 Mo) et contient Hugo et Git. L'auteur de l'image l'a créée spécifiquement pour l'intégration continue avec Wercker. Dans mon cas, j'ai dû ajouter quelques petites choses supplémentaires:

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

Apk est le gestionnaire de paquets d'Alpine, et j'ai besoin d'_openssh_ car _bitbucket.pipelines_ ne sauvegarde pas les artefacts de l'intégration pour les comptes gratuits (sauf dans la zone de téléchargement). Il est nécessaire de synchroniser les fichiers mis à jour avec le serveur distant via ssh (d'où aussi la nécessité de Git). De plus, je dois exécuter `git submodule update` car le thème _Cocoa EH_ est un sous-module dans mon référentiel.

Comme Hugo écrit les fichiers générés de la page dans le répertoire _public_, j'ai filtré les fichiers en utilisant `git filter-branch --subdirectory-filter` et j'ai créé une nouvelle branche appelée `deploy`, puis je me suis basculé sur cette branche. Le serveur distant a déjà une branche `deploy`, mais _bitbucket.pipelines_ ne clone que la branche `master`. C'est pourquoi il est également nécessaire de faire un _push_ avec l'option `git push -f` (_force_).

En outre, il a fallu générer une paire de clés cryptographiques dans le menu _pipelines_, puis dans _Paramètres_. La clé publique peut être copiée en un seul clic. Il suffit d'ajouter la clé dans les paramètres de sécurité du profil Bitbucket et tout fonctionnera comme s'il s'agissait d'un _commit_ à partir d'un référentiel source vers le serveur distant.

_Très, très simple !_ J'ai tout fait depuis une tablette ! Et l'intégration s'est faite à la vitesse de l'éclair, en moins de 15 secondes !

P.S.: la seule limitation pour moi était qu'Alpine Linux n'a pas d'option pour configurer les _locales_, ce qui m'a empêché d'accentuer le titre de la page. Mais c'était juste ça. Les symboles et les accents à l'intérieur du texte des billets sont préservés.
