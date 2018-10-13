# Deploying WebParts built with SPFx on SharePoint on-premise #

Through this article, we are going to see how we can build a _WebPart_ using the _SharePoint Framework_ (_SPFx_) and deploy it on a _SharePoint 2016 on-premise_ installation.

## Introduction ##

The _SharePoint Framework_, aka _SPFx_, is a page and extension model that allows us to develop front-end apps using client-side code. Here, we are going to see how we can quickly build a _WebPart_ and deploy it on a _SharePint 2016 on-premise_ environment.

## Prerequisites ##

Before going any further, we have to check if the "_September 2017 Public Update for SharePoint 2016_" is installed on our server, otherwise, we can't use _SPFx_. The "_Configuration database_" version must be equal or greater than _16.0.4588.1000_. We also need to have our own custom _App Catalog_.

We also need to have _Node_ and _npm_ installed on our machine. Depending on the _Node_ version that we use, we may encounter an error like "_ERR\_SSL\_PROTOCOL\_ERROR_". In such a case, setting the environment variable "_NODE\_NO\_HTTP2_" to "_1_" or disabling the "_https_" parameter in the "_serve.json_" file that we will get later, can fix the problem.

## Setting up our project ##

First we need to install _Gulp_ and _Yeoman_ globally like so:

    npm install -g yo gulp

We are then going to initialize our project like so:

    npm init
    npm install @microsoft/generator-sharepoint --save-dev

Here, we chose to install the "_Yeoman SharePoint generator_" locally and not globally because it offers us the ability to switch between different projects using different versions of the "_Yeoman SharePoint generator_". We can now run the generator like so:

    yo @microsoft/sharepoint

A few questions will be prompted. We have to specify that we want to use the _SharePoint 2016_ version and we want to create a _WebPart_. For the sake of our example, we also need to select the "_No JavaScript framework_" option.

When the installation is done, we have to install the _Developer certificate_ like so: 

    gulp trust-dev-cert

Now, if we run the following command, a series of _Gulp_ tasks will be executed and our browser will launch, so we can preview our _WebPart_ in our local dev environment:

    gulp serve

## Deploying ##

Our WebPart is just a simple "_HelloWorld_" _WebPart_, but it is enough for the exercise. Here, we are not going to see how to use _SharePoint REST APIs_ or how we can improve our development. 

So, it is now time to deploy our _WebPart_. First, let's head to the "_config/write-manifests.json_" file and let's edit it like so:

    {
    "$schema": "https://dev.office.com/json-schemas/spfx-build/write-manifests.schema.json",
    "cdnBasePath": "https://tenant-name.com/accessible/folder"
    }

Here, we specify where we host our files. It could be _SharePoint_, _Azure_ or whatever we want.

We can now generate the files that we want to deploy:

    gulp bundle -–ship

If everything is alright, we now have two folders: "_dist_", where unminified bundles are, and "_temp/deploy_", where optimized bundles are placed.

Now we can generate the "_sppkg_" package like so:

    gulp package-solution -–ship

To finish the process, we have to upload the files placed in "_temp/deploy_" to the place we specified in our "_config/write-manifests.json_" file. Then, we can upload our package to the _App Catalog_. When it is done, we can simply add our app to a site like any other _SharePoint app_.

## Conclusion ##

Through this post, we saw how we can quickly start a project using _SPFx_ and how we can deploy our app when we have an on-premise environment. However, we did not see the development part and of course, we could go further by setting up a process where our application is built, packaged and deployed automatically (but this is another subject).

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!