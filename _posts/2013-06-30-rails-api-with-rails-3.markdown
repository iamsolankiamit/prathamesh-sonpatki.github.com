---
layout: post
title: "Using `rails-api` with Rails 3"
date: 2013-06-30 11:31
comments: true
tags:
---


[`rails-api`](https://github.com/rails-api/rails-api) is an awesome gem to design API for `API-only` applications.
One more interesting thing about `rails-api` is, it doesn't have
separate versions for `Rails 3` and `Rails 4`.

`rails-api` picks up `latest Rails release` which is available. So with
the release of Rails 4, it will use Rails 4.

What if you are not yet ready for `Rails 4`?
What if you want to use `rails-api` with `Rails 3`?

Thanks to [@steveklanik](https://twitter.com/steveklabnik) for
answering my doubt.

<!-- more -->

You have to just specify in the Gemfile that you want `3.2.13 versions`
of gems that `rails-api` depends on.

For example -

```ruby
  gem 'rails-api'               # for Rails API
  gem 'actionpack', '3.2.13'    # Specifically say you want '3.2.13'
  # rails-api depends on actionpack.
  # only specifying actionpack dependency is enough.

```

And now `rails-api` will use `Rails 3`.
