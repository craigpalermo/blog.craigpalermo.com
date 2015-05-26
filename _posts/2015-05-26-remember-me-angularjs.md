---
layout: post
title:  "\"Remember me\" at login with AngularJS (repost)"
date:   2015-05-26 09:50:00
categories: [tutorial, angularjs]
---

*This is a repost of a tutorial I wrote in Nov. 2014; this information might
not be entirely up to date.*

# What we're doing here
A common task when creating a login page is enabling users to persist their
session after they close their browser/tab. Normally, this would be an
annoying bit of work that would require you to manage sessions on the
backend or manually set a cookie to expire after a certain time. Like many
other things, AngularJS makes this task much simpler.

# How do?
To accomplish this, first we'll add a checkbox to our login form and assign
it a model in our login controller's scope.

~~~html
/* my_view.html */
...
<form>
  ...
  <label>
    <input type="checkbox" ng-model="rememberMe"> Remember Me
  </label>

  <button ng-click="submit()">Sign In</button>
</form>
~~~

This allows us to do something similar to the following in whichever controller
handles our login logic.

~~~coffeescript
# login.coffee (because JS is hard)
# dependencies needed: $cookies, $window

angular.module("app").controller "LoginCtrl", ($scope, $cookies, $window) ->
  # function to handle sign in logic
  $scope.submit = ->
    # do something to get auth token
    authToken = "token"

    if $scope.rememberMe
      # user DOES to be remembered
      $cookies.token = authToken
    else
      # user DOES NOT want to be remembered
      $window.sessionStorage.token = authToken
    ...

  ...
~~~

The difference between this approach is fairly simple. Depending on whether the
user wants to be remembered or not, we either store the token (or other
identifying info of your choice) as a cookie or in session storage. Cookies
remain in the browser until they're manually removed or expire. Session
storage is cleared automatically every time the browser or tab is closed.

# Usage
Accessing the token after setting it is just as easy as setting it.

~~~coffeescript
# other_controller.coffee
# dependencies needed: $cookies, $window
...

token1 = $cookies.token
token2 = $window.sessionStorage.token

# check if user is logged in
isLoggedIn = ->
  # returns true if token is set in either session storage or cookie
  return $window.sessionStorage.token or $cookies.token
~~~


# Logging Out
When the user is ready to log out, all we have to do is ensure to clear out
both places that the token could've been stored.

~~~coffeescript
# logout.coffee
# dependencies needed: $cookieStore, $window
...
# function to remove auth token from browser, ie log user out
logout = ->
  # delete token in session storage
  delete $window.sessionStorage.token

  # delete token cookie
  $cookieStore.remove("token")
~~~
