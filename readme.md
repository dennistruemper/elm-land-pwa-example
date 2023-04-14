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

### Step 4 - Add an Icon

So an Icon is a static file, just like the manifest file, right? So we put it inside static too. And the other this is, we already put a path and name inside our manifest file, so all we have to do is to find a geat logo for our app. Oh, you say this is a hard task, I know. Lucky me, I have a cute logo of a T-Rex with a clipboard at hand, so I will use this. He will track our progress and remind us if we forget anything.
While we are at it, I will add the same file as `static/favicon.ico` so we have a nive friendly tab icon.

**This is just for demonstration and no good practice for production apps, these images are way too big. Use small images as icon.**

So no more error in dev tools and a nice looking tab, on to the next one :-)

### Step 5 - Add the service worker file

**Just to make it clear upfront, this will be a very simple serviceWorker. Do not take it in production without checking your needs**

The base for my file can be found [here](https://googlechrome.github.io/samples/service-worker/basic/). I just made it a little bit easier (on the eyes), removed the multi cache stuff to have a simpler one cache solution and changed the `PRECACHE_URLS` to match elm-lands needs. If you need some stuff from the static directory loaded at runtime and want to store it on the device when the user installs the PWA, add it to this list.

So just have a look at the `static/serviceWorker.js`.

What you want to think about is the caching strategy of your PWA. Here I choose `Network First` strategy, so local files are just a backup when there is no internet connection. This depends on your goals, I think [this blogpost](https://blog.bitsrc.io/5-service-worker-caching-strategies-for-your-next-pwa-app-58539f156f52) is a good entry point.

So now we have a serviceWorker file. _This is where the magic of a PWA is._ [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API) might explain it better than me, take some time to have a look what the serviceWorker is all about before you continue.

Welcome back, I am really proud of you, because you took the time and read the documentation. Just like I suggested.

So now you ask: 'But just the file is not enough, what do we do with it?' Let's go to the next step to answer that.

### Step 6 - Wire up the service worker file

So as you know by now, we need to install or register the service worker. Most tutorials will suggest you to add some javascript to your `index.html` file. But we are using elm-land, so this will be generated and we can not add any javascript to it. Well, at least not direct.

But we can do the better solution: Create a javascript file and reference the script in our elm-land generted index.html. So how do we do that.

First we need our javascript in a file in the `static` directory. Mine is called `registerServiceworker.js` and I hope you can gess, what the content will do ;-). Have a look to be sure!

So now that we have a script, how do we execute it using elm-land? The answer is once again the `elm-land.json` file. Just add it in the `script` block.

```js
      "script": [{ "src": "./registerServiceworker.js" }]
```

Save, refresh your page and have a look in the devtools console. There it is: `Registration successful, scope is...`

So lets have a recap.

- webmanifest done
- serviceworker done
- logo done
- https todo

So on to the next part.

### Step 7 - Serve app over HTTPS

There are a lot of ways to host a webapp. Feel free to use an other way. This is why I put this as last step, so you can follow along all the way and host as you like. I will use [vercel](vercel.com) as I find it quick and easy to use and it has [documentation on elm-land](https://elm.land/guide/deploying.html#deploying-with-vercel).

So lets get started.

1. Copy the `vercel.json` from elm-land documentation.
2. execute vercel cli (no parameters needed) with `npx vercel` and follow instructions
3. Not even a full minute later the [app is online](https://elm-land-pwa-example.vercel.app/)
4. This is no commercial for vercel, I am just blown away by how easy it is, compared to other products I used in the past. And I know there are other modern and easy alternatives.

### Step 8 - Play around

I will use an iPhone with Safari do describe how it works. Android Desktop or other usage is an exercise for the reader.

1. First visit your website with your phone or use [mine](https://elm-land-pwa-example.vercel.app).
2. Tap on the share button and selet `Add to homescreen`
3. See your logo, the `short_name` defined in webmanifest and the URL. Tap on add.
4. Congratulations - You have installed your own PWA.
5. Come an, start it.
6. Try different cache strategies, test offline functionality (right now it is bad) and learn more.
