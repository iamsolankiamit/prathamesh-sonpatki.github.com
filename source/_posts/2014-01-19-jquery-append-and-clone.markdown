---
layout: post
title: "jQuery append and clone"
date: 2014-01-19 09:28:28 +0530
comments: true
categories: jQuery
---

I was trying to append some part of the DOM to some other element of
the page.

``` html
<div id='container'>
  <ul class='contacts_list'>
  </ul>
</div>
...
<div id='updated_contacts'>
</div>
```

On a click event, i wanted to append `contacts_list` to
`updated_contacts`. Using jQuery and `append` method

``` js
  $("#updated_contacts").append($(".contacts_list"));
```

But every time, the original `updated_contacts` from `container` was
getting removed.

<!-- more -->

After searching [documentation](http://api.jquery.com/append/) of
`append` method, i found out that when an element is inserted to some
other part of DOM, it is removed from the original location.

>> If an element selected this way is inserted into a single location
>> elsewhere in the DOM, it will be moved into the target (not cloned):

Using `clone` method we can solve this problem.

`clone` creates a copy of the existing element and appends it to new
location keeping original version as it is.

``` js
  $("#updated_contacts").append($(".contacts_list").clone());
```

Documentation of `clone` method is [here](http://api.jquery.com/clone/)


>> Using `clone()` has the side-effect of producing elements with
>> duplicate id attributes, which are supposed to be unique.
>> Where possible, it is recommended to avoid cloning elements with this
>> attribute or using class attributes as identifiers instead.

is important point to consider while using `clone`.
