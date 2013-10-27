---
layout: post
title: "preload associations with `find_by_sql`"
date: 2013-10-26 16:36
comments: true
categories: rails activerecord fun
---

I have a very complex query which is made up of more than 1
subqueries. Arel is awesome, but it can't generate that query. So i
generate those subqueries separately and combine them by `union` or
`intersection` based on some condition. Finally the generated query is
given to `find_by_sql` to get the data.

We found out that, in the jbuilder template that was rendering this
data, that it was calling association objects resulting in `n+1` query
problem.

Generally this problem is solved by `eager-loading` in rails.

For eg. to load the company of the user, we do

``` ruby
@users = User.where(status: 'connected').includes(:company)
```

Rails will do the magic(load the company of the user also in the same query) and when we refer to `@user.company` from
view, it would not trigger any new sql query and directly access it
from ruby object.

But this doesn't work with `find_by_sql`.

``` ruby
@users = User.find_by_sql(some_condition).includes(:company)
```

will throw error. Because `find_by_sql` returns Ruby array and not
`ActiveRecord::Relation` object. So it doesn't respond to `includes`.

If we try calling `includes` first,

``` ruby
@users = User.includes(:company).find_by_sql(some_conditions)
```

The `includes` part is silently dropped and only `find_by_sql` part
gets executed.

`ActiveRecord::Associations::Preloader` class to our rescue.

### Only applicable for Rails 3

We can use the `run` method from `Preloader` class like follows:

``` ruby
@users = User.find_by_sql(some_condition)
ActiveRecord::Associations::Preloader.new(@users, :company).run
```

This will preload the company association for the `@users` object like
it will do when we use `includes`.

### Rails 4 way

In Rails 4, `run` method is not present. It is replaced by
the `preload` method which was `private` in Rails 3. We can call
`preload` method directly as follows.

``` ruby
@users = User.find_by_sql(some_condition)
ActiveRecord::Associations::Preloader.new.preload(@users, :company)
```

The first argument to this method is records array, second argument is
associations and third is options which are optional.

We can pass a single association like `:company`.

We can also pass multiple associations in the form of array `[:company, :account]`.

We can also pass a hash to eager load associations of associations
like `{ company: :category }`.

We can also mix last two options like `[{ company: :category }, :account]`.

Happy Hacking!
