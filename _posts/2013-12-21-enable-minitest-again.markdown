---
layout: post
title: "enable minitest again"
date: 2013-12-21 22:51:34 +0530
comments: true
tags: rails minitest
---

If you have skipped minitest/test-unit while creating a rails app with `-T` or
want to move to minitest from rspec or want to start with minitest in
an existing rails project without tests, its very easy.

Just include

``` ruby
    require "rails/test_unit/railtie"
```

in `config/application.rb`.

After this `rake test` will start working. You may have to create test
folder structure on your own or can copy from an existing project with tests.
