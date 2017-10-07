Pack the Web with webpack

The number of available tools for Web development can sometimes make our life difficult because choosing the right one to use can be a tough task. Fortunately, _webpack_ seems to want to make our life easier and allows us to quickly set up a nice workflow. Let's see how it works.

_webpack_ is a module _bundler_ able to pack _JavaScript_ files or any other resource or asset for usage in a browser.

## Installing webpack ##
To install _webpack_, we need to have _[node.js](https://nodejs.org/en/)_ installed. Then, to install _webpack_ globally, we can do the following command:

	$ npm install webpack -g

## Creating a project ##
For our first try with _webpack_, we will create a directory with the following hierarchy:

	src/
		main.js
	index.html
	wepack.config.js
	
_Project hierarchy_

In our _index.html_ file, we are going to write the following lines:

	<!DOCTYPE html>
	<html>
	  <head>
		<meta charset="utf-8">
		<title>webpack</title>
	  </head>
	  <body>
		<h1>Hello World!</h1>
		<script src="./dist/bundle.js"></script>
	  </body>
	</html>
	
_index.html_ file

We are also going to keep our _main.js_ file very simple:

	console.log("Hello World!");
	
_main.js file_

## Defining the config file ##

For the moment our project is very simple and we could just run the following command to create a _build_:

	webpack src/main.js dist/bundle.js
	
_Building our project manually_

We would see a _dist_ folder and a _bundle.js_ file appear. But our project is going to be more complex and that is where the _webpack.config.js_ file is useful. So, let's fill this last one with the following lines:

	module.exports = {

		// Entry point
		entry: './src/main.js',

		// Output point
		output: {
			path: __dirname + '/dist',
			filename: 'bundle.js'
		}

	};
	
_webpack.config.js_ file

Now, from our root directory, we can run the next command:

	wepack
	
_Using our webpack.config.js file_

If everything is alright, our _bundle_ will be generated. But what are we doing here? The _webpack_ command takes two arguments, the _entry_ and the _output_ points. That is what we did when we ran the first command. The configuration file we created allows us to specify those parameters.

## Using loaders ##

Out of the box, _webpack_ only understands _.js_ files. _Loaders_ tell _webpack_ how to load files for bundling. To use them, we first need to create a _package.json_ file by running the following command:

	npm init
	
_Creating package.json file_

Nowadays, we are very tempted to write _JavaScript_ using _es6_. Unfortunately, not every browser supports it and we have to convert our code. We can do this with _Babel_:

	npm install babel-loader babel-core babel-preset-es2015 --save-dev
	
_Installing Babel_

Now, in our _webpack.config.js_ file, we can add our module like so:

	module: {
		loaders: [
			{
				test: /\.js$/,
				exclude: /node_modules/,
				loader: 'babel-loader',
				query: {
					presets: ['es2015']
				}
			}
		],
	}
	
_Adding module_

Here we tell _webpack_ to run through the _Babel Loade_r every _JS_ file, excluding those in _node_modules_, and to use the _es2015_ preset for _Babel_. So, adding the following lines in our _main.js_ file won't be a problem:

	const name = 'John Doe';
	setTimeout(() => alert(`Hey ${name}!`), 300);
	
_Editing main.js file_

We can also use loaders for _CSS_ and _SASS_. Let's give it a shot!

### CSS ###
We will start first with _CSS_ and for that, we need to install a bunch of _loaders_:

	npm install css-loader style-loader --save-dev
	
_Installing loaders_

Now, just for fun, we are going to create another file in the src folder named _assets.js_.

	require('./css/main.css');
	
_assets.js file_

We now have to modify our _main.js_ file. Let's put the following line at the top of this last one:

	require('./assets.js');
	
_Editing main.js file_

It is time to create the _main.css_ file in the _src/css_ folder. Nothing really fancy for the moment:

	body {
		background-color: #f3f3f3;
	}
	
_main.css file_

Now, in the _webpack.config.js_ file, in the _loaders_ section, we can add the following lines:

	{
		test: /\.css$/,
		exclude: /node_modules/,
		loader: 'style-loader!css-loader'
	}
	
_Adding CSS and Style Loaders_

Magically, the next time we will create a _bundle_, our _CSS_ file will be included for our greatest pleasure!

### SASS ###

Using _SASS_ with _webpack_ is a little bit trickier than using standard _CSS_. Let's start by installing what we need:

	npm install sass-loader node-sass extract-text-webpack-plugin install html-webpack-plugin clean-webpack-plugin --save-dev
	
_Installing some plugins_

Here, we are going to place standard _CSS_ and _SASS_ into a same file and then inject our _bundles_, _js_ and _css_, into an _index.html_ file generated on the fly.

In the _src_ folder, let's create some new files with the following hierarchy:

	src/
		css/
			main.css
		scss/
			partials/
				_main.scss
			app.scss
			
_Adding files_
		
For our example, we are going to feed our files like so:

	@import "partials/main";

_app.scss file_

	h1 {
		color: #f5f341;
		font-weight: bold;
	}
	
__main.scss file_

We also need to edit our _index.html_ file:

	<!DOCTYPE html>
	<html>
	  <head>
		<meta charset="utf-8">
		<title>webpack</title>
	  </head>
	  <body>
		<h1>Hello World!</h1>
	  </body>
	</html>
	
_Edited index.html file_

We can now edit our _webpack.config.js_ file like so:

	const CleanWebpackPlugin = require('clean-webpack-plugin');
	const ExtractTextPlugin = require("extract-text-webpack-plugin");
	const HtmlWebpackPlugin = require('html-webpack-plugin');
	const path = require('path');

	let pathsToClean = [
	  'dist'
	];

	module.exports = {

		entry: ['./src/main.js', './src/scss/app.scss'],

		output: {
			path: __dirname + '/dist',
			filename: 'bundle.[hash].js'
		},

		watch: true,
		devtool: "source-map",

		module: {
			rules: [
				{
					test: /\.js$/,
					exclude: /node_modules/,
					loader: 'babel-loader',
					query: {
						presets: ['es2015']
					}
				},
				{
					test: /\.css$/,
					exclude: /node_modules/,
					use: ExtractTextPlugin.extract({
						use: [{
							loader: "css-loader"
						}]
					}),
				},
				{
					test: /\.scss$/,
					exclude: /node_modules/,
					use: ExtractTextPlugin.extract({
						use: [{
							loader: "css-loader"
						}, {
							loader: "sass-loader"
						}],
						fallback: "style-loader"
					})
				}
			]
		},
		plugins: [
			new CleanWebpackPlugin(pathsToClean),
			new ExtractTextPlugin({
				filename: 'bundle.[contenthash].css',
				allChunks: true
			}),
			new HtmlWebpackPlugin({
				title: 'Custom template',
				filename: 'index.html',
				template: __dirname + '/index.html'
			})
		]

	};
	
_Edited webpack.config.js file_

As we can see, we made a lot of changes, but there is nothing horrible! We changed the entry point, created a few rules for our _CSS_ and _SCSS_ files. Finally, we used _ExtractTextPlugin_ to create a _bundle_ for our _CSS_ and _SCSS_ files and _HtmlWebpackPlugin_ to inject our files in a brand new _index.html_ file placed in the _dist_ folder and based on our _index.html_ file at the root. We also cleaned the _dist_ folder before building with _CleanWebpackPlugin_.

### Copying our images ###

We may also want to place our images in our dist folder. _CopyWebpackPlugin_ offers a solution to do that. Let's install it like so:

	npm install --save-dev copy-webpack-plugin
	
_Installing CopyWebpackPlugin_

To use this plugin, we have to edit our _webpack.config.js_ file like so:

	...
	const CopyWebpackPlugin = require('copy-webpack-plugin');
	...
	plugins: [
		...
		new CopyWebpackPlugin([
			{ from: __dirname + '/src/assets/img', to: __dirname + '/dist/assets/img' }
		])
	]
	
_Adding CopyWebpackPlugin_

If we want to process images, _[ImageWebpackLoader](https://github.com/tcoopman/image-webpack-loader)_ can also do the job.

## Watch mode ##

As we can see, in the previous section, we also added the _watch_ parameter. This allows us to change our code and automatically have it compiled.

## Server ##

_webpack_ can create a server that listens for any change in our project and then injects that change into our browser. For that, we first need to install the development server:

	npm install webpack-dev-server -g
	
_Installing webpack development server_

Then we can run the following command:

	webpack-dev-server
	
_Running webpack development server_

Our project will be available at _http://localhost:8080/_ and we will see our changes every time we save our files.

## Conclusion ##

Through this brief introduction, we saw that _webpack_ is a really powerful tool than can easily be integrated into our workflow. But we can go further by exploring the various options of this tool. For example, we could choose to do only certain operations under our development environment and others in production.