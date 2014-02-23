---
layout: post
title: "conditional where in rails"
date: 2013-10-14 21:12
comments: true
tags: rails ruby Arel activerecord
---

### T.L.D.R

You can pass `blank` object to `where` clause and it will return
current relation as it is.


### Long version
Sometimes, we need to apply `where` clause conditionally.
For eg. apply timeframe condition if timeframe is given in params.
We can achieve this using simple if/unless.

``` ruby
  def most_recent(from = nil, to = Date.today)
    condition = Activity.where("created_at < ?", to)
    condition.where("created_at > ?", from) unless from.blank?
  end
```
<!-- more -->

This works but doesn't look good. If we can chain this second
conditional scope, it would be much better.
Enter magic of Rails. If we read documentation of `where` clause,

``` ruby
# === blank condition
    #
    # If the condition is any blank-ish object, then #where is a no-op and returns
    # the current relation.
```
This is interesting. This means if we pass any `blank` value to where
clause, it returns the `current relation` as it is.

Now we have to just write a method which will generate the query if
condition is satisfied. It will return `nil` otherwise resulting in a
`no-op`.

``` ruby
  def most_recent(from = nil, to = Date.today)
    Activity.where("created_at < ?", to).where(from_condition(from))
  end
```

``` ruby
  def from_condition(from)
    "created_at > #{from}" unless from.blank?
  end
```

This looks more cleaner than earlier solution. It will work the same
way if we pass `''`, `""`, `{}`, `[]`, `false` or any other object
that respond to blank as true instead of `nil`.

Happy Hacking!
