# SharePoint Development Models #

Through this article, we are going to have an overview of the different _SharePoint_ development models. Let's get into it!

## Introduction ##

Before building an application, we have a large amount things to think: needs, goals, architecture, infrastructure, frameworks and so on. Developing for _SharePoint_ add an additional layer of complexity because we have to choose between various ways to work.

Each _SharePoint_ development model has its purposes, advantages and difficulties. Here, we are going to have an overview of those different models.

## Farm Solutions ##

Also known as _Full Trust Solutions_, they require to be developed on a _SharePoint_ server and have access to full server-side _SharePoint API_. They are supported in _SharePoint_ on-premise installations and have to be deployed by a _Farm Administrator_ and the various features they can contain are then available to the entire farm.

_Farms Solutions_ are distributed as _wsp_ packages and can have different scope: _Farm_, _Web Application_, _Site Collection_ or _Website_. They support things like _Features_, _Event Receivers_, _Timer Jobs_, _WebParts_, _Modules_ and so on.

When we deploy a _Farm Solution_, we have to keep in mind that an _IIS Reset_ will be performed.

## Sandbox Solutions ##

Because _Farm Solutions_ are very permissive, _Microsoft_ introduced another kind of solution: _Sandbox Solutions_. Their scope is smaller because they can only target the _Site Collection_ and have access to a small subset of the server-side _API_.

The _wsp_ packages can be deployed to a _Solutions Gallery_ and they don't force the server to reset and while a _Farm Solution_ can bring down the whole farm, a _Sandbox Solution_ has only impact on a _Site Collection_. They are very useful to deploy assets or _Content Types_ and _Lists_.

_Sandbox Solutions_ are now deprecated, but they can still be used.

## Add-Ins ##

Also known as _Apps_, _SharePoint Add-Ins_ are deployed in the _App Catalog_, in the form of an _.app_ file, public or private, and provide a way to develop an application without any server-side code executing on the _SharePoint_ server. This means that _Add-Ins_ run either in the context of the client browser or on another server.

This model introduced with the concept of the _Office Store_ and Cloud-related things in mind, provides a high level of isolation. _Add-Ins_ require working with the _Client Side Object Model_ (_CSOM_) or the _REST API_.

_Microsoft_ claims that a _Farm Solution_ can be converted into one or various _Add-Ins_. However, create things that could easily be done with a _Farm Solution_ with an _Add-In_ can be tricky.

_SharePoint Add-Ins_ come in various flavors:

* SharePoint-Hosted
* Provider-Hosted

_SharePoint-Hosted Add-Ins_ are installed on a _SharePoint Website_, called the _Host Web_ while their resources are hosted on an isolated subsite called the _App Web_. They only support _JavaScript_, a few _ASPX_ files and _XML_. _SharePoint-Hosted Add-Ins_ can access data and resources that are outside of the _App Web_ by using one of the following techniques to bypass the browser's same origin policy: a special _JavaScript_ cross-domain library or a specific _JavaScript WebProxy_ class.

_Provider-Hosted Add-Ins_ include components that are deployed on another server while they are installed on the _Host Web_. It means that we are able to run server-side code on another server and to communicate with _SharePoint_ using _CSOM_. They offer a great flexibility to develop the various elements we need.

If we can create _WepParts_ with _Farm Solutions_, _Add-Ins_ offer something similar called _Add-In Parts_, or _Client WebPart_. This concept is similar to _WebPart_, but it implies that the _Add-In Part_ displays a webpage that we specify by using an _IFrame_ in a page in the _Host Web_.

_SharePoint Add-Ins_ are security principals that need to be authenticated and authorized and this can be done in various ways. An _Add-In_ uses permission requests to ask for the permissions it needs. The permission requests specify the rights that the _Add-In_ needs and the scope at which it needs the rights.

## SharePoint Framework ##

Also known as _SPFx_, the _SharePoint Framework_ is the most recent addition to the SharePoint developer toolbox. It provides full support for client-side development it grows with the development of _SharePoint Online_. It allows us to develop components using modern web technologies such as _React_. For now, the support of this framework is more advanced in _SharePoint Online_ and it is only possible to develop _WebParts_ and _Extensions_.

One advantage of this framework is that we don't need _SharePoint_ to be installed on our machine to develop. We just have to download a few _Node_ packages and to run our server using _Gulp_. When we compile what we developed, we also get an _.app_ file.

## Conclusion ##

Through this article, we saw the various existing ways to develop for and with _SharePoint_. We saw the main idea behind each model, what they have in common and how they differ. We saw that _Solutions_ use server-side _API_ and _Add-Ins_ aim to execute in a client context. We also had a small overview of the _SharePoint Framework_.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!