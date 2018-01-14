# Building Apps with Electron #

Here, we are going to build a cross-platform desktop application using _Electron_.

## Introduction ##

Nowadays, there are plenty of solutions to create desktop apps that we can use. What about building a cross-platform application using _JavaScript_, _HTML_ and _CSS_? Let's take a look at this solution.

## What is Electron? ##

In a few words, _Electron_ is a framework for cross-platform desktop applications using _Chromium_ and _Node.js_. It allows us to create desktop applications with _HTML_, _JS_ and _Node.js_, then package it into an executable file and distribute it across _Windows_, _macOS_ and _Linux_.

_Electron_ is an open source project maintained by _GitHub_ and a community of contributors. Initially developed for _GitHub's Atom editor_, it has been used to create applications by companies like _Microsoft_, _Facebook_, _Slack_ or _Docker_.

_Electron_ also has built-in features like so:

* Automatic updates
* Native menus and notifications
* App crash reporting
* Debugging and profiling
* _Windows_ installer

## Anatomy of an Electron application ##

Before building our application, let's take a little look at what is behind the scenes.

### Main Process ###

We previously mentioned _Node.js_ and as we are going to see later, there is a _package.json_ file at the root of our application.

This file contains various information about our project, dependencies, but also, like most of every time when we create a _Node.js_ application, a _script section_ that tells _Electron_ which file to run when the app starts. Traditionally, this file is called _main.js_.

The process that runs the main script is called _Main Process_. It controls the life of the application and creates _Render Processes_.

### Render Process ###

_Electron_ uses _Chromium_ for displaying web pages, so, the multi-process architecture of this last one is also used. Each web page in _Electron_ runs in its own process, which is called the _Renderer Process_. It means that there can be many _Render Processes_ and each is independent. If one crashes, it won't affect the others.

While on normal browsers web pages usually run in a sandboxed environment and are not allowed to access native resources, _Electron_ allows to use _Node.js_ APIs to perform lower level operating system interactions.

### A little further ###

_Electron_ creates web pages by creating _BrowserWindow_ instances and each one runs the web page in its own _Renderer Process_. When a _BrowserWindow_ instance is destroyed, the corresponding _Renderer Process_ is also terminated. This means that the _Main Process_ manages all web pages and their corresponding _Renderer Processes_ and each one is isolated and only cares about the web page running in it.

The _Main Process_ can access the native _GUI_ through a series of modules. So, if we want to perform _GUI_ operations in a web page, the _Renderer Process_ of the web page must communicate with the _Main Process_ to request that the _Main Process_ perform those operations, because calling native _GUI_ related APIs is not allowed. Managing native _GUI_ resources in web pages is very dangerous and it is easy to leak resources.

An _Interprocess Communication System_ or _IPC_, can be used to pass messages between the _Main Process_ and the _Render Processes_.

## A simple example ##

Now we saw how _Electron_ works in theory, let's build a really simple app! Here, we are going to make a really simple application using the _IGDB API_ to display some information about video games.

### Setting up our tools ###

To reach our goal, we are going to use the _electron-quick-start_ repository to create our basic requirements.

    git clone https://github.com/electron/electron-quick-start.git simple-app
<small>_Cloning the repository_</small>

Let's head to our brand-new directory and install what we need:

    cd simple-app
    npm install
<small>_Installing dependencies_</small>

If we want, we can change some information in the _package.json_ file. Next, let's try our app with the following command:

    npm start
<small>_Starting the application_</small>

If everything is alright, a window saying "_Hello World!_" will open.

### A closer look at the main.js ###

Before going any further, let's take a closer look at the _main.js_ file!

    const electron = require('electron')
    // Module to control application life.
    const app = electron.app
    // Module to create native browser window.
    const BrowserWindow = electron.BrowserWindow

    const path = require('path')
    const url = require('url')

    // Keep a global reference of the window object, if you don't, the window will
    // be closed automatically when the JavaScript object is garbage collected.
    let mainWindow

    function createWindow () {
      // Create the browser window.
      mainWindow = new BrowserWindow({width: 800, height: 600})

      // and load the index.html of the app.
      mainWindow.loadURL(url.format({
        pathname: path.join(__dirname, 'index.html'),
        protocol: 'file:',
        slashes: true
      }))

      // Open the DevTools.
      // mainWindow.webContents.openDevTools()

      // Emitted when the window is closed.
      mainWindow.on('closed', function () {
        // Dereference the window object, usually you would store windows
        // in an array if your app supports multi windows, this is the time
        // when you should delete the corresponding element.
        mainWindow = null
      })
    }

    // This method will be called when Electron has finished
    // initialization and is ready to create browser windows.
    // Some APIs can only be used after this event occurs.
    app.on('ready', createWindow)

    // Quit when all windows are closed.
    app.on('window-all-closed', function () {
      // On OS X it is common for applications and their menu bar
      // to stay active until the user quits explicitly with Cmd + Q
      if (process.platform !== 'darwin') {
        app.quit()
      }
    })

    app.on('activate', function () {
      // On OS X it's common to re-create a window in the app when the
      // dock icon is clicked and there are no other windows open.
      if (mainWindow === null) {
        createWindow()
      }
    })

    // In this file you can include the rest of your app's specific main process
    // code. You can also put them in separate files and require them here.
<small>_main.js file_</small>

Here we can see that a few things are imported first. Then, we have the _createWindow_ function that is responsible the _BrowserWindow_ instantiation. This function is used as a callback function with the ready event. We can also see that, in this function, we load a file named _index.html_.

We can also notice the _render.js_ file. We are going to rename it _index.js_ for convenience.

### Coding ###

Let's add some new _Node modules_:

    npm install --save dotenv igdb-api-node 

Now, let's edit our _index.html_ file like so:

    <!DOCTYPE html>
    <html>
      <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
        <title>List of Super Mario Games</title>
      </head>
      <body>

        <div class="container-fluid">
          <div class="row">
            <div class="col-md-12">
                <h1>It's-a me, Mario!</h1>
            </div>
          </div>

          <div class="row">
              <div class="col-md-12">
                  <ul id="list" class="list-group">
                  </ul>
              </div>
            </div>
        </div>

        <script>
          require('./index.js')
        </script>
      </body>
    </html>
<small>index.html file</small>

And now our _index.js_ file:

    const igdb = require('igdb-api-node').default

    require('dotenv').config()

    // A really simple output function
    const displayItem = (item) => `
      <li class="list-group-item">
        <img src="${typeof item.cover !== 'undefined' ? 'https://' + item.cover.url : ''}" />
        <h2>${item.name}</h2>
        <p>${typeof item.summary !== 'undefined' ? item.summary : 'No summary'}</p>
      </li>`

    // Setting up the IGDB API
    const client = igdb(process.env.API_KEY)

    // A simple request
    client.games(
      {
        offset: 0,
        order: 'release_dates.date:asc',
        limit: 20,
        search: 'mario'
      },
      [
        'name',
        'cover',
        'summary'
      ])
    .then(response => {
      const data = response.body

      if (data.length > 0) {
        const html = data.map(displayItem).join('')
        document.getElementById("list").innerHTML = html
      }

    })
    .catch(error => {
      throw error
    })
<small>_index.js file_</small>

If we run our application, a list of _Super Mario_ games should appear on the screen.

## Packing the application ##

Now let's pack our application. We need _Electron Packager_ for this.

    npm install electron-packager -g
<small>_Installing Electron Package globally_</small>

We can now pack our application like so:

    electron-packager /path/to/application SimpleApp --platform=darwin --arch=x64 --out=/output/path
<small>_Packing our application_</small>

Here, we just set a few parameters:

* The project folder
* The name of the application
* The targeted platforms, here _macOS_
* The architecture
* The output path

## Embedding a database ##

Embedding a database directly in our application is a question that we could ask ourselves. Unfortunately, packing something like _MongoDB_ in our application is really straightforward. It is easier to have a _MongoDB Server_ running somewhere and to establish a connection between this last one and our application.

Nevertheless, we have many alternatives like _PouchDB_, _NeDB_ or _TingoDB_. We could also use _filesystem_ or _LocalStorage_.

## Conclusion ##

Through this article we saw what _Electron_ is, how it works and what it allows. We also saw how easy it is to build a simple cross-platform desktop application using _JavaScript_.

Of course, our application is not really useful nor somehow exciting. But now, we are ready to do greater things, like building a more sophisticated app our embedding something like _Angular_ or _React_ in our project.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!