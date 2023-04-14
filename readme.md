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

### Step 2 - Add the manifest file

Elm-land does not have a directory with the name `static` right now, so we create it and add the `manifast` file. This can take an optional fileextension if you like `manifest.webmanifest` more. Files inside the static directory will be served, so it is now possible to bring this file to the client.

Let's have a look at the content. What is in there? I assume you know JSON, this file is formatted with it. So let's just talk about the minimal content.

- `name` and `short_name`: Well, here you can define the name of your app. It the name is too long for some use cases you can also define a shorter name. If you want to know more, have a look at the documentation for [name](https://developer.mozilla.org/en-US/docs/Web/Manifest/name) or [short_name](https://developer.mozilla.org/en-US/docs/Web/Manifest/short_name).
- `icons`: Without icon you get nothing to tap on your phones home screen, so this is mandatory to have an app.
- `start_url`: This is the path of your elm-land app you want to direct to, when the app is started. In almost every example of PWAs you will find `./index.html` as the default value. **WATCH OUT** elm-land will redirect you to page not found if you enter index.html after a route defined with `Home_.elm`. This took me some time to figure it out. Be smart and set `./` as the `start_url`.
- `display`: You have to define the look of the app. Our app should look like a normal app on the phone and not like a browser, that's why we use `standalon`. If you do not want this, choose an other option from the [documentation](https://developer.mozilla.org/en-US/docs/Web/Manifest/display).

That was a lot to explain for a 14 line file added. This means there is a lot of heavy lifting done by the browser vendors. The next time will be way easier :-)

### Step 3 - Wire up the manifest file

So the manifest file should now be reachable by requesting `http://localhost:1234/manifest` in your browser. If not, check the port or go back to `step 2`. But our application does not know about the manifest file. If you load your app and check the traffic in the dev tools you will not see the manifest file. So how do we get it to load in the client?

Elm-land makes this an easy task. Just add an link entry to the elm-land.json like you did with the stylesheet earlier.

Now refresh your page and you should see the manifest file in your network traffic. (Ignore the error for fivicon for now, we will be fixing this soon)
