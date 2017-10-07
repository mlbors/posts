# First Node.js app on Heroku #

In this article we are going to see how to build a really simple _Node.js_ app and how to deploy it on _Heroku_.

## Introduction ##

The project we are going to make is very simple. Its goal is to discover how to build a _Node.js_ app quickly and how to deploy this last one on _Heroku_. It is going to be really straight forward and we are not going to use additional frameworks like _Express_. Our application is just going to be a timestamp microservice that we are going to be able to reach with something like _http://localhost:3000/September%2026,%202017_ or _http://localhost:3000/1506384000_ and it is going to return a human readable date and a timestamp. 

## What is Node.js? ##

If we go on [_Node.js_'s official website](https://nodejs.org/en/), we can read the following statement:

"_Node.js® is a JavaScript runtime built on Chrome's V8 JavaScript engine. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient. Node.js' package ecosystem, npm, is the largest ecosystem of open source libraries in the world._"

It can be tough to understand. We can simplify it like so: _Node.js_ is an open source server framework that runs on various platforms and allows us to use _JavaScript_ on the server side. This is a basic explanation, but we can get the picture.

So, we are going to use _Node.js_ to build a really simple application that we are going to deploy on _Heroku_.

## What is Heroku? ##

[_Heroku_'s website](https://www.heroku.com/) says things like so: "_Heroku is a platform as a service that enables developers to build, run, and operate applications entirely in the cloud_". In other words, with _Heroku_, we can simply push our code to a remote repository and everything we need to deploy our application will be run for us. The _Heroku_ platform supports development in _Ruby on Rails_, _Java_, _Node.js_, _Python_, _Scala_ and _Clojure_.

In our small project, we are going to use the _Heroku_ platform to deploy our application.

## Installation ##

To achieve our goal, we need to install a few things. Let's grab [_Node_ and _NPM_](https://nodejs.org/en/) and [_Git_](https://git-scm.com/). We also need an account on [_GitHub_](https://github.com/) and [_Heroku_](https://www.heroku.com/). When it is done, we'll have to install [_Heroku CLI_](https://devcenter.heroku.com/articles/heroku-cli).

Now, let's build our app!

## Initializing our app ##
First, let's create a directory. We can then go into that last one. We now have to run the following command:

	npm init
_Initializing the project_

A few questions will be asked during the process. Our app is now in place.

## Pushing it on GitHub ##

It is time to create a repository on GitHub. When it is done, in our directory, we can run the following commands:

	git init
	git add .
	git commit -m "First commit"
	git remote add origin REPOSITORY
	git push -u origin master
_Linking our code to our remote repository_

Our app is on _GitHub_. We can move to the next step!

## Finishing the code ##

In our file _package.json_, in the scripts section, let's add the following line:

	"start": "node index.js"
_Package.json file_

This line allows us to execute our application with the following command:

	npm start
_Running our app_

We are now going to create a file called _index.js_ where we are going to put the following code:

	const http = require('http')
	const url = require('url')

	const service = require('./service')

	const hostname = '0.0.0.0';
	const port = process.env.PORT || 3000

	const server = http.createServer((req, res) => {

	  if (req.method === 'GET') {

		const path = url.parse(req.url).pathname.slice(1).replace(/%20/g, ' ')
		
		res.writeHead(200, {'Content-Type': 'application/json'})    
		res.write(JSON.stringify(service(path)));
		res.end();

	  }

	}).listen(port, hostname, function(){
	  console.log('Server running at http://${' + hostname + '}:${' + port + '}/');
	})
_index.js file_

With the first two lines, we import two core modules that we are going to use to create our server and to parse our URL. The _HTTP module_ allows _Node.js_ to transfer data over the _Hyper Text Transfer Protocol_ while the _URL module_ splits up a web address into readable parts. The next line imports another module that we will talk about later.

The port that we listen to will be the one defined in the _environment variables_ on _Heroku_ or _3000_.

Then, we create an _HTTP server_ with a request handler function as parameters. We have access to two objects, _request_ (req) and _response_ (res).  

After this, we check the method used for the request, get the _pathname_ attribute from our _URL_ and the set the headers before sending a response body in _JSON_. Then we just end the response.

With _listen_, we bind a port and listen for incoming connections. It is important to note that the _listen_ method needs to be called on the _server object_.

Now, let's take a closer look at the service module. This just another file, called _service.js_, where we just export one function:

	'use strict'

	const months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December']

	const isValidDate = date => {
	  return date.getTime() === date.getTime()
	} 
	  
	const parseTimeStamp = timestamp => {
	  const date = new Date(timestamp)
	  return months[date.getMonth()] +  ' ' + date.getDate() + ', ' + date.getFullYear()
	}

	const fromTs = timestamp => {
	  return {unixtime: timestamp, natural: parseTimeStamp(timestamp*1000)}
	}

	const fromNatural = date => {
	  const ts = date.getTime()
	  return {unixtime: ts/1000, natural: parseTimeStamp(ts)}
	}

	const prepareDate = str => {
	  let output = {}
		
	  if (typeof str !== 'undefined' && str !== '' && str !== null) {

		const input = parseInt(str)

		if (isNaN(input)) {
		  const date = new Date(str)
		
		  if (isValidDate(date)) {
			output = fromNatural(date)
		  } else {
			output = {error: 'invalid date format'}
		  }
	  
		} else {
		  const pattern = /\d{10}/g
		  const regex = new RegExp(pattern)
		  let m
		  if ((m = regex.exec(input)) !== null) {
			output = fromTs(input)
		  } else {
			output = {error: 'invalid timestamp'}
		  }
		}
		  
	  }
	  return output
	}

	module.exports = prepareDate
_service.js_

This is not a perfect code neither the cleanest, but it is just for the example. But in the end, it means that we can reach our application with something like _http://localhost:3000/September%2026,%202017_ or _http://localhost:3000/1506384000_ and it is going to return a human readable date and a timestamp.

## Preparing ##

We now have to tell _Heroku_ what command to execute to start our app. For that, we need to create a file called _Procfile_ without extension. In this last one, we are simply going to place the following line:

	web: node index.js
_Procfile file_

## Deployment ##
Now, we can deploy our app on _Heroku_. In our directory, we have to run the following commands:

	heroku login
	heroku create
	git push heroku master
_Pushing our application_

The application is now deployed. We can ensure that at least one instance of the app is running like so:

	heroku ps:scale web=1
_Scaling our app_

We can now visit our app through our _Heroku_ dashboard or with the next command:

	heroku open
_Opening our app_

In our dashboard, we can also set an additional option that will link our _GitHub_ repository to Heroku and execute the deployment process every time we push something on our _master branch_.

## Conclusion ##

Through this article we saw how to install _Node.js_ and how to initialize a simple application. We used core modules to create a really basic application to get more familiar with _Node.js_. We also deployed our code on _Heroku_ in a very smooth way with just a few commands. As an aside note, we can say that we did not really use _NPM_, the package manager for _Node.js_. We used it just to initialize our app. That's why there's no _node_modules_ folder in our app directory. We could also do it with _Yarn_ which is pretty close to _NPM_.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!