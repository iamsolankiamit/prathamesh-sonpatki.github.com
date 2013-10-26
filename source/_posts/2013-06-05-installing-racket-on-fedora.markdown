---
layout: post
title: "Installing racket on fedora"
date: 2013-06-05 21:45
comments: true
categories:
---

As root user

``` bash
    cat >/etc/yum.repos.d/rpm-sphere.repo <<EOF
    [rpm-sphere]
    name=RPM Sphere
    baseurl=http://download.opensuse.org/repositories/home:/zhonghuaren/Fedora_18/
    gpgkey=http://download.opensuse.org/repositories/home:/zhonghuaren/Fedora_18/repodata/repomd.xml.key
    enabled=1
    gpgcheck=1
    EOF
```

Then

``` bash
    yum update
    yum install racket
```

<!-- more -->

To run racket from Emacs, enable marmalade repo

```
    (require 'package)
       (add-to-list 'package-archives
       '("marmalade" . "http://marmalade-repo.org/packages/"))
       (package-initialize)
```

Then install [geiser](http://www.nongnu.org/geiser/) package

```
   M-x package-install-RET geiser
   M-x run-geiser
```

It will ask for choice of language `guile` or `racket`.
Selecting racket will open the REPL to play with it.
