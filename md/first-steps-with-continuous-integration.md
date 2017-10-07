# First steps with continuous integration

In this post, we are going to overview some tools to use _continuous integration_.

_Continuous integration_ (CI) is a development practice that requires developers to integrate code into a shared _repository_ several times a day. Each check-in is then verified by an _automated build_, allowing teams to detect problems early. _CI_ originated from within the _extreme programming paradigm_, but the principles can be applied to any _iterative programming_ model, such as _agile programming_.

There are many tools that we can use and one of the most famous is probably _Jenkins_, written in _Java_. But we are not going to talk about _Jenkins_ in this post. Instead, we are going to generate a really small project with some other tools. For that, we need _Git, Ruby, PHP, Composer, PHPUnit, PHP-CS-Fixer, GitHub, Travis, StyleCI_ and _Codecov_. We may think that is a lot of stuff, but there is no reason to be afraid.

## Definitions

First of all, a few and short definitions!

**Git**

_Git_ is a free and open source distributed version software for tracking changes in computer files and coordinating work on those files among multiple people. It was created by _Linus Torvalds_.

**GitHub**

_GitHub_ is a _web-based Git_ that got a mascot called _Octocat_, a cat with five tentacles and a human-like face. It was created by _Chris Wanstrath, PJ Hyett_ and _Tom Preston-Werner_.

**Ruby**

A famous programming language. We will need it to install _gems_ (Ruby Libraries). 

**PHP**

Another famous programming language. We will create our project with it.

**Composer**

A dependency manager for _PHP_ that was initially developed by _Nils Adermann_ and _Jordi Boggiano_.

**PHPUnit**

A framework for _unit testing_ for _PHP_ created by _Sebastian Bergmann_.__

____

**PHP-CS-Fixer**

A tool to automatically fix _PHP_ coding standards issues.

**Travis CI**

A _continuous integration_ service used to build and test software projects hosted at _GitHub_.

**StyleCI**

_StyleCI_ makes sure that all code on our _PHP_ project is written to a consistent standard.

**Codecov**

_Codecov_ provides metrics and insights into the results of tests through _code coverage_.

For the sake of this article, we are not going to discuss here about _test driven development_ (TDD), how to write good and efficient tests and things like that. These are other subjects. The aim of this post is to see how to install and use all of those tools.

## Installation

**Composer**

Installing _Composer_ is really easy. We just have to go to [https://getcomposer.org/download/](https://getcomposer.org/download/) and follow the instructions. On _Windows_, there is an installer.

**PHPUnit**

Again, we have to go to [https://phpunit.de/manual/current/en/installation.html](https://phpunit.de/manual/current/en/installation.html) and follow the instructions. On _Windows_, globally install _PHPUnit_ is a bit trickier.

**Ruby**

On a _Linux_ system and on _macOS_, _Ruby_ is already here. On _Windows_, we have to download the installer from [https://rubyinstaller.org/downloads/](https://rubyinstaller.org/downloads/).

**Git**

We just need to go here [https://git-scm.com/downloads](https://git-scm.com/downloads) and follow the instructions.

**PHP-CS-Fixer**

On a _Linux_ system and on _macOS_, we can globally install _PHP-CS-Fixer_ by following the instructions written here [https://github.com/FriendsOfPHP/PHP-CS-Fixer](https://github.com/FriendsOfPHP/PHP-CS-Fixer).

**GitHub, Travis CI, StyleCI, Codecov**

We just have to go on _GitHub_ ([https://github.com/](https://github.com/)) and create an account. We will then have access to _Travis CI_ ([https://travis-ci.org/](https://travis-ci.org/)), _StyleCI_ ([https://styleci.io/](https://styleci.io/)) and _Codecov_ ([https://codecov.io](https://codecov.io)) with that same account.

**Travis gem**

The _gem_ can be install by doing the following command in the terminal:
    
    
    gem install travis

## Setting up the project

Now that we have all our new tools, we can move to the next step and set up our project.

**Composer**

The first thing we need to do is to create a _composer.json_ file at the root of our project. We can create it manually or by doing the following command:
    
    
    composer init

_Command_
    
    
    {
        "name": "mlbors/simple-travis-codevoc-styleci",
        "description": "Simple project using Composer, PHPUnit, Travis, Codecov, PHP-CS-Fixer and StyleCI",
        "license": "MIT",
        "require": {
            "php": ">=5.6",
            "ext-curl": "*",
            "friendsofphp/php-cs-fixer": "2.0.0"
        },
        "require-dev": {
            "phpunit/phpunit": "5.7.*",
            "vlucas/phpdotenv": "^2.0"
        },
        "autoload": {
            "psr-4": {
                "mlbors\\SimpleApp\\": "app/"
            }
        },
        "autoload-dev": {
            "psr-4": {
                "mlbors\\SimpleApp\\Tests\\": "tests/"
            }
        },
        "homepage": "https://github.com/mlbors/simple-travis-codevoc-styleci",
        "authors": [ 
            {
                "name": "mlbors",
                "email": "matyas.bors@gmail.com",
                "homepage": "http://www.mlbors.com/"
    
            }
        ]
    
    }

_composer.json file_

In the above example, in the _require_ section, we say that we need _PHP 5.6_ or higher, _Curl_ and _PHP-CS-Fixer_. In _require-dev_, we have _PHPUnit_ and _PHPDotEnv_. In the _autoload_ and _autoload-dev_ sections, we say that we use the _PSR-4_ standard and how our namespaces will be.

It is time to do the following commands to install and generate the _autoload_:
    
    
    composer install
    composer dump‑autoload -o

**GitHub**

First of all, we create two files: _.gitignore_ and _.gitattributes_.
    
    
    /vendor
    /node_modules
    .env
    .php_cs.cache
    composer.lock
    npm-debug.log

_.gitignore file_
    
    
    /.gitattributes export-ignore  
    /.gitignore export-ignore
    /phpunit.xml export-ignore
    /.env export-ignore
    /.env.example export-ignore

_.gitattributes file_

Then, we go on _GitHub_ and create a new _repository_. Then, on our system, at the root of our project, we do the following commands:
    
    
    git init
    git add .
    git commit –m "First commit"
    git remote add origin https://github.com/user/repository.git
    git push –u origin master

Replace _user_ and _repository_ by your username and your repository

Now we can go on _Travis CI_, _StyleCI_ and _Codecov_ and link our _repository_ to those services.

**StyleCI**

Now, at the root of our project, we create the _styleci.yml_ that will tell _StyleCI_ file which rules we use.
    
    
    preset: psr1

_.styleci.yml file_

**.env File**

The _.env_ file allows us to place some values that we do not want to appear in our code. In our example, we can load them with _PHPDotEnv_. The _.env_ file will be ignored by _Git_ but we can create a _.envexample_ file with fake values just to tell the world that we need a _.env_ file.
    
    
    SIMPLE_KEY="someValue"

_.env file_

**Travis CI**

Now we can create the _.travis_ file that will tell _Travis CI_ what to do with our files. We can also include critical values in that file, for example, the ones that are in the _.env_ file, by encrypting them like so:
    
    
    travis encrypt SOMEVAR=secretvalue

_Command_
    
    
    language: php
    php:
    - 5.6
    - 7.0
    install:
    - sudo pip install codecov
    before_script:
    - composer self-update
    - composer install --no-interaction
    after_success:
    - bash <(curl -s https://codecov.io/bash)
    script: 
    - vendor/bin/phpunit --coverage-clover=coverage.xml
    env:
    - SIMPLE_KEY=someValue
    notifications:
      email: false

_.travis file_

In the above example, we tell _Travis CI_ a few things: we state that we use _PHP_ and our app will have to work on _PHP 5.6_ and _PHP 7_. We also install the _Python_ _codecov_ package with _pip_ for _code coverage_. Our dependencies will also be installed with _Composer_ and _PHPUnit_ will be executed. If everything is alright, we send the information to _Codecov_.

More information about how to customize the build are available [here](https://docs.travis-ci.com/user/customizing-the-build)!

**PHP-CS-Fixer**

The _.php_cs file_ tells _PHP-CS-Fixer_ the rules that we use.
    
    
    notPath('vendor')
        ->in(__DIR__)
        ->name('*.php')
        ->ignoreDotFiles(true)
        ->ignoreVCS(true);
    
    $rules = [
        '@Symfony' => true
    ];
    
    return PhpCsFixer\Config::create()
        ->setRules($rules)
        ->setFinder($finder)
        ->setUsingCache(false);

_.php_cs file_

**PHPUnit**

We create a _phpunit.xml_ to tell _PHPUnit_ how to run our _unit tests_. We also set the _processUncoveredFilesFromWhitelist_ attribute for _code coverage_.
    
    
    <?xml version="1.0" encoding="UTF-8"?>
    <phpunit backupGlobals="false"
             backupStaticAttributes="false"
             bootstrap="vendor/autoload.php"
             colors="true"
             convertErrorsToExceptions="true"
             convertNoticesToExceptions="true"
             convertWarningsToExceptions="true"
             processIsolation="false"
             stopOnFailure="false"
             syntaxCheck="false">
        <testsuites>
            <testsuite name="SimpleApp Test Suite">
                <directory suffix="Test.php">./tests/</directory>
            </testsuite>
        </testsuites>
        <filter>
            <whitelist processUncoveredFilesFromWhitelist="true">
                <directory suffix=".php">./app</directory>
            </whitelist>
        </filter>
    </phpunit>

_.phpunit.xml_

In the above example, among other things, we tell _PHPUnit_ where our _test suite_ will be and how to compute the _code coverage_.

**README**

The _README.md_ file tells the world about our project. We can also include into it the badges that _Travis CI, StyleCI_ and _Codecov_ will generate.
    
    
    [![Build Status](https://travis-ci.org/mlbors/simple-travis-codevoc-styleci.svg?branch=master)](https://travis-ci.org/mlbors/simple-travis-codevoc-styleci)
	[![StyleCI](https://styleci.io/repos/75407002/shield?branch=master)](https://styleci.io/repos/75407002)
	[![codecov](https://codecov.io/gh/mlbors/simple-travis-codevoc-styleci/branch/master/graph/badge.svg)](https://codecov.io/gh/mlbors/simple-travis-codevoc-styleci)
    
    Simple project using Composer, PHPUnit, Travis, Codecov, PHP-CS-Fixer and StyleCI

_.README.md file_

## Let’s use all those tools

**PHP-CS-Fixer**

To use _PHP-CS-Fixer_, we can enter the following commands in our terminal:
    
    
    $ php php-cs-fixer.phar fix /complete/path/to/dir
    $ php php-cs-fixer.phar fix /complete/path/to/file

On _Windows_ we can do it like so:
    
    
    php path\to\project\vendor\friendsofphp\php-cs-fixer\php-cs-fixer fix path\to\fileordir

**PHPUnit**

To run our tests, we just have to use
    
    
    phpunit

On _Windows_, we can do it like so:
    
    
    php path\to\project\vendor\phpunit\phpunit\phpunit

## And we push

It is time! Now we can do
    
    
    git add .
    git commit –m "This is a commit"
    git push origin mybranch

If everything is alright, our files will be send to _GitHub_, then to _Travis CI_ and _StyleCI_. _Travis CI_ will do what we wrote in our _.travis_ file then send coverage information to _Codecov_. And voilà!

## Advantages

_Continuous integration_ advantages are several:

  * Integration bugs are detected early and are easy to track down
  * Constant availability of a “current” build for testing, demo, or release purposes
  * Frequent code check-in pushes developers to create modular, less complex code
  * Enforces discipline of frequent automated testing
  * Immediate feedback on system-wide impact of local changes



And so on. On the other hand, we can state that _continuous integration_ requires a considerable amount of work and time. And 100% _code coverage_ doesn’t mean that every execution path is tested and our software is bug free.

You can find the complete example on my _GitHub_ ([https://github.com/mlbors/simple-travis-codevoc-styleci](https://github.com/mlbors/simple-travis-codevoc-styleci).
