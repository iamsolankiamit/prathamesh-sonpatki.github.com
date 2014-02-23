---
layout: post
title: "Devise : Passing params to after_* paths"
date: 2014-02-23 18:59:08 +0530
comments: true
tags: rails devise
---

Devise allows us to customize `after_signup_path` by overriding a
`protected` method in `RegistrationsController`.

``` ruby
class RegistrationsController < Devise::RegistrationsController
  protected

  def after_inactive_sign_up_path_for(resource)
    your_custom_path
  end

  def after_sign_up_path_for(resource)
    your_custom_path
  end
end
```

If we are using `:confirmable` with devise, sometimes we need to show
user a message -

``` text
Confirmation email has been sent to you <user_email>.
Please confirm your email.
```

Lets say that route name for this is `confirmation_email_sent_path`.

``` ruby
class RegistrationsController < Devise::RegistrationsController
  protected

  def after_inactive_sign_up_path_for(resource)
    confirmation_email_sent_path
  end
end
```

Now how to pass user's email to this path?

<!-- more -->

One way is to override `create` action of Devise's
RegistrationsController. But this is bad as we will get dependent on
current Devise implementation.

Lets see how we can achieve this with minimum dependency.

`resource` has the `User` object. So we can use that to pass user's
email to the `confirmation_email_sent` action.

``` ruby
class RegistrationsController < Devise::RegistrationsController
  protected

  def after_inactive_sign_up_path_for(resource)
    confirmation_email_sent_path(email: resource.email)
  end
end
```

Now we can access this email in `confirmation_email_sent` action.

``` ruby
def confirmation_email_sent
  @email = params[:email]
end
```
