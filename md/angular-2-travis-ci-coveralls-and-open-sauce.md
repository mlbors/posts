## [Angular 2, Travis CI, Coveralls and Open Sauce](https://mlbors.tumblr.com/post/160471409055/angular-2-travis-ci-coveralls-and-open-sauce)

In this post, we are going to see how we can make an _Angular 2_ app work with _Travis CI_, _Coveralls_ and _Open Sauce_.

In a previous post, we saw how to set up a _PHP_ project using _Travis CI_, _StyleCI_ and _Codecov_. We are now going to do quite the same, but with a small _Angular 2_ app. Our purpose here is to test our app under multiple environments on each commit.

So the first thing we need to do is to sign up for a [_GitHub_](https://github.com) account, then we will have access to [_Coveralls_](https://coveralls.io/) with that same account. The next thing is to open another account on [_Open Sauce_](https://saucelabs.com/open-source). We are now ready to begin! Let’s create our repository, commit and push!

To achieve our goal, it is important that our project runs with [_angular-cli_](https://github.com/angular/angular-cli). We are going to assume that is the case and that _Git_ is also installed.

## Dependencies

For a start, we need to install several dependencies with _NPM_. Let’s do it like so in the terminal:
    
    
    npm install karma-coverage karma-coveralls karma-firefox-launcher angular-cli-ghpages --save-dev

_Install dependencies_

With that last command, we installed _karma-coverage_ that is responsible for code instrumentation and _coverage reporting_. _karma-coveralls_ will help us to transmit the report to _Coveralls_. _karma-firefox-launcher_ is a _Karma_ plugin that will help us with our tests. Finally, _angular-cli-ghpages_ will help us to deploy our app on _GitHub Pages_.

## Karma

Now we need to set a few things in the _karma.conf.js_ file that is included in the root of our folder. The file will look like so:
    
    
    module.exports = function (config) {
      config.set({
        basePath: '',
        frameworks: ['jasmine', '@angular/cli'],
        plugins: [
          require('karma-jasmine'),
          require('karma-chrome-launcher'),
          require('karma-firefox-launcher'),
          require('@angular/cli/plugins/karma'),
          require('karma-coverage')
        ],
        client:{
          clearContext: false // leave Jasmine Spec Runner output visible in browser
        },
        files: [
          { pattern: './src/test.ts', watched: false }
        ],
        preprocessors: {
          'dist/app/**/!(*spec).js': ['coverage'],
          './src/test.ts': ['@angular/cli']
        },
        mime: {
          'text/x-typescript': ['ts','tsx']
        },
        coverageReporter: {
          dir : 'coverage/',
            reporters: [
              { type: 'html' },
              { type: 'lcov' }
            ]
        },
        angularCli: {
          config: './angular-cli.json',
          codeCoverage: 'coverage',
          environment: 'dev'
        },
        reporters: config.angularCli && config.angularCli.codeCoverage
                  ? ['progress', 'coverage']
                  : ['progress'],
        port: 9876,
        colors: true,
        logLevel: config.LOG_INFO,
        autoWatch: true,
        browsers: ['Chrome', 'Firefox'],
        singleRun: false
      });
    };

_karma.conf.js file_

_Karma_ is a test runner that is ideal for writing and running _unit tests_ while developing the application.

## Protractor

We are now going to configure _Protractor_ for _e2e testing_. There is already a file for that in the root of our folder, but is only suitable for local tests with a single browser. Let’s create a new one a folder called _config_. We will name that file _protractor.sauce.conf.js_ and it will be like the following:
    
    
    var SpecReporter = require('jasmine-spec-reporter').SpecReporter;
    
    var buildNumber = 'travis-build#'+process.env.TRAVIS_BUILD_NUMBER;
    
    exports.config = {
      sauceUser: process.env.SAUCE_USERNAME,
      sauceKey: process.env.SAUCE_ACCESS_KEY,
      allScriptsTimeout: 72000,
      getPageTimeout: 72000,
      specs: [
        '../dist/out-tsc-e2e/**/*.e2e-spec.js',
        '../dist/out-tsc-e2e/**/*.po.js'
      ],
      multiCapabilities: [
        {
          browserName: 'safari',
          platform: 'macOS 10.12',
          name: "safari-osx-tests",
          shardTestFiles: true,
          maxInstances: 5
        },
        {
          browserName: 'chrome',
          platform: 'Linux',
          name: "chrome-linux-tests",
          shardTestFiles: true,
          maxInstances: 5
        },
        {
          browserName: 'chrome',
          platform: 'macOS 10.12',
          name: "chrome-macos-tests",
          shardTestFiles: true,
          maxInstances: 5
        },
        {
          browserName: 'chrome',
          platform: 'Windows 10',
          name: "chrome-latest-windows-tests",
          shardTestFiles: true,
          maxInstances: 5
        },
        {
          browserName: 'firefox',
          platform: 'Linux',
          name: "firefox-linux-tests",
          shardTestFiles: true,
          maxInstances: 5
        },
        {
          browserName: 'firefox',
          platform: 'macOS 10.12',
          name: "firefox-macos-tests",
          shardTestFiles: true,
          maxInstances: 5
        },
        {
          browserName: 'firefox',
          platform: 'Windows 10',
          name: "firefox-latest-windows-tests",
          shardTestFiles: true,
          maxInstances: 5
        },
        {
          browserName: 'internet explorer',
          platform: 'Windows 10',
          name: "ie-latest-windows-tests",
          shardTestFiles: true,
          maxInstances: 5
        },
        {
          browserName: 'MicrosoftEdge',
          platform: 'Windows 10',
          name: "edge-latest-windows-tests",
          shardTestFiles: true,
          maxInstances: 5
        }
      ],
      sauceBuild: buildNumber,
      directConnect: false,
      baseUrl: 'YOUR_GITHUB_PAGE',
      framework: 'jasmine',
      jasmineNodeOpts: {
        showColors: true,
        defaultTimeoutInterval: 360000,
        print: function() {}
      },
      useAllAngular2AppRoots: true,
      beforeLaunch: function() {
        require('ts-node').register({
          project: 'e2e'
        });
      },
      onPrepare: function() {
        jasmine.getEnv().addReporter(new SpecReporter());
      }
    };

_protractor.sauce.conf.js file_

We can notice that there are two environment values: _SAUCE_USERNAME_ and _SAUCE_ACCESS_KEY_. We can set theses values in our _Travis CI_ account in the settings sections of our project. The information can be found in our _Sauce Labs_ account settings.

_Protractor_ is an _end-to-end_ test framework for _Angular_. _Protractor_ runs tests against our application running in a real browser, interacting with it as a user would.

## e2e configuration

In the _e2e_ folder of our application, we need to place a file called _tsconfig.json_.
    
    
    {
      "compileOnSave": false,
      "compilerOptions": {
        "declaration": false,
        "emitDecoratorMetadata": true,
        "experimentalDecorators": true,
        "module": "commonjs",
        "moduleResolution": "node",
        "outDir": "../dist/out-tsc-e2e",
        "sourceMap": true,
        "target": "es5",
        "typeRoots": [
          "../node_modules/@types"
        ]
      }
    }

_tsconfig.json file in e2e folder_

We also need to place a similar file at the root of our application.
    
    
    {
      "compileOnSave": false,
      "compilerOptions": {
        "outDir": "./dist/out-tsc",
        "baseUrl": "src",
        "sourceMap": true,
        "declaration": false,
        "moduleResolution": "node",
        "emitDecoratorMetadata": true,
        "experimentalDecorators": true,
        "target": "es5",
        "typeRoots": [
          "node_modules/@types"
        ],
        "lib": [
          "es2016",
          "dom"
        ]
      }
    }

_tsconfig.json file_

_End-to-end_ (e2e) tests explore the application as users experience it. In _e2e_ testing, one process runs the real application and a second process runs _Protractor_ tests that simulate user behavior and assert that the application respond in the browser as expected.

## Coveralls

In our folder, we are now going to create a file called _.coverall.yml_ with the token corresponding to our respository that can be found in our _Coveralls_ account.
    
    
    repo_token: YOUR_TOKEN

_.coverall.yml file_

## Travis CI

Now it is time to tell _Travis CI_ what to do with our files. Let’s create a file called _.travis.yml_ and fill it like so:
    
    
    language: node_js
    sudo: true
    dist: trusty
    
    node_js:
      - '6'
    
    branches:
        only:
            - master
    
    env:
        global:
            - CHROME_BIN=/usr/bin/google-chrome
            - DISPLAY=:99.0
    
    cache:
      directories:
         - node_modules
    
    before_install:
         - ./scripts/install-dependencies.sh
         - ./scripts/setup-github-access.sh
    
    after_success:
        - ./scripts/delete-gh-pages.sh
        - git status
        - npm run build-gh-pages
        - npm run deploy-gh-pages
        - git checkout master
        - sleep 10
        - tsc -p e2e
        - npm run e2e ./config/protractor.sauce.conf.js
    
    notifications:
    email: false

_.travis.yml file_

## GitHub Token

Before we go any further, we need to create an access token on our _GitHub_ account. We can do that in the settings of our account.

## Scripts

In the previous section, we told _Travis CI_ to use three _bash_ scripts: _install-dependencies.sh_, _setup-github-access.sh_ and _delete-gh-pages.sh_. We are now going to create a folder called _scripts_ and create those three different scripts like so:

The first script, as its name lets us figure out, just install our dependencies.
    
    
    #!/bin/bash
    
    export CHROME_BIN=/usr/bin/google-chrome
    export DISPLAY=:99.0
    
    #Install chrome stable version
    sh -e /etc/init.d/xvfb start
    sudo apt-get update
    sudo apt-get install -y libappindicator1 fonts-liberation
    wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
    
    sudo dpkg -i google-chrome*.deb
    
    rm -f google-chrome-stable_current_amd64.deb

_install-dependencies.sh file_

The second script ensures that we can access to our _GitHub_ repository during the build. We can see in this script that a variable called _$GH_TOKEN_ is used. This environment variable can be set in _Travis CI_ by clicking on Settings in our repositories list.
    
    
    #!/bin/bash
    set -e
    echo "machine github.com" >> ~/.netrc
    echo "login your@email.com" >> ~/.netrc
    echo "password $GITHUB_TOKEN" >> ~/.netrc

_setup-github-access.sh file_

The last script deletes the branch _gh-pages_ to let us deploy our app on _GitHub Pages_ (we can’t do it if the branch is already here).
    
    
    #!/bin/bash
    set -e
    
    git ls-remote --heads | grep gh-pages > /dev/null
    
    if [ "$?" == "0" ]; then
      git push origin --delete gh-pages
    fi

_delete-gh-pages.sh file_

Now, we need to tell _GitHub_ and _Travis CI_ that these files are executable. We can do that with the following command:
    
    
    chmod +x the_file

_Git command to change chmod_

Perhaps the following command may also be needed:
    
    
    git update-index --chmod=+x the_file

_Git command to update index_

## package.json

We now have to make a few adjustments in our _package.json_ file, more specifically, in the scripts section. We can see that use once again the _$GH_TOKEN_ variable.
    
    
    "scripts": {
        "ng": "ng",
        "start": "ng serve",
        "build": "ng build",
        "test": "ng test --code-coverage true --watch false && cat ./coverage/*/lcov.info | ./node_modules/coveralls/bin/coveralls.js",
        "lint": "ng lint",
        "pree2e": "webdriver-manager update --standalone false --gecko false",
        "e2e": "protractor",
        "build-gh-pages": "ng build --prod --base-href \"/YOUR_REPOSITORY/\"",
        "deploy-gh-pages": "angular-cli-ghpages --repo=https://GH_TOKEN@github.com/YOUR_USERNAME/YOUR_REPOSITORY.git --name=YOUR_USERNAME --email=YOUR_EMAIL"
    }

_package.json file_

## app.po.ts

We need to make one last adjustment in the file called _app.po.ts_ that we can find in the _e2e_ folder. Let’s make it like so:
    
    
    import { browser, element, by } from 'protractor';
    
    export class BookreaderPage {
      navigateTo() {
        browser.ignoreSynchronization = true;
        return browser.get(browser.baseUrl);
      }
    
      getParagraphText() {
        return element(by.css('app-root h1')).getText();
      }
    }

_app.po.ts file_

## Here we go!

Now, we can push (again) our files and, if everything is alright, the magic will happen! We just have to check _Travis CI_, _Coveralls_ and _Sauce Labs_.
