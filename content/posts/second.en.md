---
title: "Second"
author: "bcalvino"
date: 2017-11-01T23:04:12-03:00
keywords: Hugo, Bitbucket, git, deploy, continuous integration, theme, outTemer
---

In this second post, I will describe the process of continuous integration with Bitbucket that allows, for example, that I write this text directly in the web browser interface. Bitbucket, versioning control repository of corporate giant Atlassian, has its own seamless integration solution _bitbucket-pipelines_. Its configuration is basically the usual, using a _yaml_ format file.

The basic structure of the _bitbucket-pipelines.yaml_ file is simple:

```yaml
image: andthensome / alpine-hugo-git-bash

pipelines:
  branches:
    master:
      - step:
          script:
            - hugo
```

I chose this particular docker image because it is based on Alpine Linux (and thus has a minute size of 5MB) and has Hugo and Git preinstalled. The author of the image created it precisely for continuous integration with Wercker. In my case, I had to add a few more things:

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
            - git add. && git commit -m "new update"
            - git filter-branch --prune-empty --subdirectory-filter public
            - git checkout -b deploy
            - git push -f origin deploy
```

Apk is Alpine's package manager, and I need _openssh_ because _bitbucket.pipelines_ doesn't save integration artifacts in free accounts (except in the download area). It is necessary to synchronize the updated files with the remote via ssh (hence also the need for Git). Also, I need to give a `git submodule update` because the _Cocoa EH_ theme is a submodule inside my repository.

Since Hugo writes the generated page files to the _public_ directory, I filtered the files using `git filter-branch --subdirectory-filter` and created a new _branch_ called` deploy`, then _checkout_ on it. The remote already has a `deploy` branch, but _bitbucket.pipelines_ only clones `master`. So it is also necessary to _push_ with the `git push -f` (_force_) option.

In addition, it was necessary to generate a cryptographic key pair in the _pipelines_ menu and then _settings_. The public key can be copied with one touch. Just add the key to the Bitbucket profile security settings and everything will work like a _commit_ from a source repository to the remote.

_Très, très simple!_ I did everything from a tablet! And the integration was lightning fast, lasting less than 15 seconds!

PS: The only limitation for me was that Alpine Linux has no option to configure _locales_, and this prevented me from accentuating the page title. But that was all. The symbols and accentuation within the text of the posts are preserved.
