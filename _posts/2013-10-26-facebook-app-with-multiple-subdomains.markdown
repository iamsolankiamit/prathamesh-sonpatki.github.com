---
layout: post
title: "facebook app with multiple subdomains"
date: 2013-10-26 11:14
comments: true
tags: facebook social fun
---

I had a requirement of adding facebook integration with our Rails app.
I am using `omniauth-facebook` gem for the authentication part and it
works great.

Our app has multiple subdomains for all clients, so i wanted to
callback URLs separate for each client.

<!-- more -->

For example,

For `foo.example.com`, callback should be `http://foo.example.com/auth/facebook`
For `bar.example.com`, callback should be `http://bar.example.com/auth/facebook`

We can do that in our Facebook app settings, using `App Domains` field.

The description of `App Domains` is as follows:

> Enable auth on domain and subdomain(s) (e.g., "example.com" will enable *.example.com)

To test this locally, i added entries in `/etc/hosts/` for testing

```
    127.0.0.1   foo.myapp.com
    127.0.0.1   bar.myapp.com
    127.0.0.1   baz.myapp.com
```

Then accessing `foo.myapp.com:3000` and `bar.myapp.com:3000` and
clicking on facebook authenticaton, it redirected me correctly to
`foo.myapp.com:3000/auth/facebook` and
`bar.myapp.com:3000/auth/facebook` respectively.

In production, we have to replace the App Domain with actual URL of our website.
