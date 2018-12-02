# Initializing a project with Parcel #

Through this small post, we are going to see how to initialize a project with _Parcel_.

## Introduction ##

There are many tools that help us to build our application or our website. Some are really complex and sometimes tedious to set up just for a small project. This is where we can use _Parcel_.

Parcel is a bundler that requires almost zero configuration to achieve or goal. Our example is going to be pretty straightforward.

## Installing ##

Before going any further, we need to install _Node.js_ and _npm_. Then, we can install _Parcel_ globally like so:

    npm install -g parcel-bundler

Now, let's initialize our project:

    npm init

We can now install a few dependencies:

    npm install -save bootstrap jquery popper.js

And now, a few dev dependencies:

    npm install --save-dev sass

## Adding a few files ##

Let's create the following files:

    src/
        assets/
            img/
                foo-img.jpg
            scripts/
                main.js
            styles/
                main.scss
    index.html

We can now fill our different files like so:

    require('bootstrap');
    require('../img/*.*');
<small>_main.js_</small>

    @import "../../../node_modules/bootstrap/scss/bootstrap.scss";
<small>_main.scss_</small>

    <!DOCTYPE html>
    <html>
        <head>
            <meta charset="utf-8">
            <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
            <link rel="stylesheet" href="assets/styles/style.scss">
        </head>
        <body>

            <div class="container-fluid">
                <div class="row">
                
                    <div class="col">
                        <h1>Using Parcel</h1>
                        <p>Foo content!</p>
                    </div>
                </div>
            </div>

            <script src="assets/scripts/main.js"></script>
        </body>
    </html>
<small>_index.html_</small>

## Package.json ##

In our _package.json_ file, we can now add the following lines:

    "main": "src/index.html",
    "scripts": {
        "serve": "parcel src/index.html --out-dir dist",
        "watch": "parcel watch src/index.html --out-dir dist",
        "build": "parcel build src/index.html --out-dir dist"
    }

We can now run the following command

    npm run serve

If everything is alright, our website will be available at http://localhost:1234. 

## Conclusion ##

Through this small article, we saw how we can use _Parcel_ to set up quickly a simple project. As we saw, there is almost no configuration to do to achieve our goal.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!