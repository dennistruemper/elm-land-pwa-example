# elm-land PWA example

## Why ?

This is to demonstrate the basic steps of turning an elm-land application into an installable PWA (Progressive Web App). This is just the beginning, you have to add offline functionality or PWA features like nofification, taking pictures and others.

## How ?

This uses the elm-land example [05-user-auth](https://github.com/elm-land/elm-land/tree/34bb0d8/examples/05-user-auth) as a starting point. If you are not familiar with it, have a look at the [turorial](https://elm.land/guide/user-auth.html) first.

We have a branch `main` where the finished result is. A branch `initial` where the original tutorial code is. And a branch `introduction-of-pwa` where every step has its own commit.

You will find a description of every step in this file in the section **Steps**

## Steps

Following every step needed to installable PWA.

### Step 0

We need a server to authenticate the user. We do not want to run it on every device using the PWA, so a localhost implementation isn't a great idea for this. So first lets swap the localhost address to some address I made up with a similar implemented server.

### Step 1 - Let's make a plan

So now the boring stuff is out of our way, lets start with PWA specific things. First of all, what do we need?

[MDN](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Tutorials/js13kGames/Installable_PWAs) tells us, we need 4 things:

1. A web manifest
2. Web site to be served from a secure HTTPS domain.
3. An icon to represant the app on the device
4. A service worker

Oh, that sounds like a lot of work and a lot of questions. Lets tackle it one by one.
