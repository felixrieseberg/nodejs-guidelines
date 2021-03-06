# Microsoft + Node.js Guidelines
Microsoft :heart: Node.js!

We work hard to contribute to the Node.js community and we want to make sure your experience is as seamless as possible. In particular, our goals here are to:
* make it easier for people using Microsoft services and technologies to get started on the right foot with Node.js
* consolidate Microsoft's Node.js offerings in a centralized place to make it easier for you to find information
* communicate status on key issues we're addressing and collecting feedback from the Node.js community
* provide a forum to connect various teams at Microsoft working on improving the Node.js experience.

Note that this is not intended to be a comprehensive set of recommendations. Rather it's meant to be a helpful set of content that makes it easier to avoid any potential gotchas, and the beginning of what we expect to be an ongoing conversation on how we can improve the Node.js experience on Microsoft platforms.

## Contribution guidelines

### Pull requests are welcome!
This is still a work in progress, an experiment - if you will, and we'd love your help to make it better! Please file an issue on any major changes you're planning - e.g. "Add new X section", and assign yourself to issues you intend to take on to prevent duplication of effort.

It's also good to make a pull request for any change to the README (minor spelling and wording changes are fine to commit to master). This enables everyone to stay updated on what's going on in the repo. We'll be lax on this in the beginning as we work together to populate this with an initial set of content. 

### Emoji legend

> :bulb: This is a tip that provides the reader with some additional info that's not necessary, but potentially useful for the task at hand.

> :triangular_flag_on_post: **TODO** This describes a todo item that we'd like some help with.

> :chart_with_upwards_trend: **IN PROGRESS** This provides awareness about an important issue that we're currently working on resolving.


## Table of contents
* [Hello World](https://github.com/Microsoft/nodejs-guidelines/blob/master/README.md#hello-world)
* [Working with npm packages](https://github.com/Microsoft/nodejs-guidelines/blob/master/README.md#working-with-npm-packages)
  * [Using an existing npm package](https://github.com/Microsoft/nodejs-guidelines/blob/master/README.md#using-an-existing-npm-package)
  * [Managing npm dependencies](https://github.com/Microsoft/nodejs-guidelines/blob/master/README.md#managing-npm-dependencies)
  * [Publishing npm packages to the registry](https://github.com/Microsoft/nodejs-guidelines/blob/master/README.md#publishing-npm-packages-to-the-registry)
  * [Local vs. Global packages](https://github.com/Microsoft/nodejs-guidelines/blob/master/README.md#local-vs-global-packages)
* [Configuring your Windows development environment](https://github.com/Microsoft/nodejs-guidelines/blob/master/README.md#configuring-your-windows-development-environment)
  * [Command-line console recommendations and other tools](https://github.com/Microsoft/nodejs-guidelines/blob/master/README.md#command-line-console-and-other-useful-tools)
  * [Editors and IDEs](https://github.com/Microsoft/nodejs-guidelines/blob/master/README.md#editors-and-ides)
  * [MAX_PATH explanation and workarounds](https://github.com/Microsoft/nodejs-guidelines/blob/master/README.md#max_path-explanation-and-workarounds)
  * [Compiling native addon modules](https://github.com/Microsoft/nodejs-guidelines/blob/master/README.md#compiling-native-addon-modules)
* [Writing cross-platform apps](https://github.com/Microsoft/nodejs-guidelines/blob/master/README.md#writing-cross-platform-apps)
* [Deployment](https://github.com/Microsoft/nodejs-guidelines/blob/master/README.md#deployment)
* [Application-specific tips](https://github.com/Microsoft/nodejs-guidelines/blob/master/README.md#application-specific-tips-and-tricks)
* [Node.js + Microsoft products, services, and contributions](https://github.com/Microsoft/nodejs-guidelines/blob/master/README.md#nodejs--microsoft-products-services-and-contributions)
* [Anything you want!](https://github.com/Microsoft/nodejs-guidelines/blob/master/README.md#contribution-guidelines)

## Hello World
Let's start with the basics. 

1. Install Node.js: https://nodejs.org.
> :bulb: When you install Node.js, you'll want to ensure your `PATH` variable is set to your install path so you can call Node from anywhere.

2. Create a new directory named `hello-world`, add a new `app.js` file:
  ```js
  /* app.js */
  console.log('Hello World!')
  ```
 
3. In the commmand prompt, run `node app.js`.
> :bulb: Your environment variables are set at the time when the command prompt is opened, so ensure to open a new command prompt since step 1 if you get any errors about Node not being found.

4. Moving beyond from simple console applications...
  ```js
  /* app.js */
  
  // Load the built-in 'http' module.
  var http = require('http');

  // Create an http.Server object, and provide a callback that fires after 'request' events.
  var server = http.createServer(function (request, response) {
     // Respond to the http request with "Hello World" and a basic header.
     response.writeHead(200, {'Content-Type': 'text/plain'});
     response.end('Hello World!\n');
  });

  // Try retrieving a port from an environment variable, otherwise fallback to 8080.
  var port = process.env.PORT || 8080;
  
  // Start listening on the specified port and print out a url to visit.
  server.listen(port);
  console.log('Listening on http://localhost:' + port);
  ```

5. In the commmand prompt, run `node app.js`, and visit the url that's printed out to the console.

6. To stop the application, run `Ctrl+C`.

## Working with npm packages
As shown above, it's pretty impressive what you can do with so few lines of code in Node.js. Part of the philosophy of Node.js is that the core should remain as small as possible. It provides just enough built-in modules, such as filesystem and networking modules, to empower you to build scalable applications. However, you don't want to keep re-inventing the wheel every time for common tasks. 

Introducing, npm! 

npm is the package manager for JavaScript. npm ships with Node.js, so there's no need to install it seperately. 

### Using an existing npm package
To get a sense for how to use npm packages in your app, let's try getting started with `express`, the most popular web framework for Node.js.

1. Create a new directory entitled `my-express-app`, then install `express` from within that directory. When `express` is installed, the package and its dependencies appear under a `node_modules` folder.
  ```
  C:\src> mkdir my-express-app
  C:\src> cd my-express-app
  C:\src\my-express-app> npm install express
  ```
  
  > :bulb: We recommend starting with a short path like C:\src to work around any potential MAX_PATH issues.

2. Now, create a new file, `app.js`. This code will load the express module we just installed, and use it to start a lightweight web server.
  ```js
  /* app.js */
  
  var express = require('express');
  var app = express();
 
  app.get('/', function (req, res) {
    res.send('Hello World!');
  })
 
  var port = process.env.PORT || 3000;
 
  app.listen(port);
  console.log('Listening on http://localhost:' + port);
  ```

3. Start the app by running `node app.js` in the command line. Tada! 

There are many more packages available at your disposal (200K and counting!). Head on over to https://www.npmjs.com to start exploring the ecosystem.

> :bulb: Most of the packages available via npm tend to be pure JavaScript, but not all of them. For instance, there's a small percentage of native module addons available via npm that provide Node.js bindings, but ultimately call into native  C++ code. This includes packages with `node-gyp`, `node-pre-gyp`, and `nan` dependencies. In order to install and run these packages, some additional machine configuration is required (described below).

### Managing npm dependencies
Once you start installing npm packages, you'll need a way to keep track of all of your dependencies. In Node.js, you do this through a `package.json` file. 

1. To create a `package.json` file, run the `npm init` in your app directory. 
  ```
  C:\src\my-express-app> npm init
  ```
  
2. Npm will prompt you to fill in the details about your package.
3. In the `package.json` file, there is a "dependencies" section, and within it, an entry for `"express"`. A value of `"*"` would mean that the latest version should be used. To add this entry automatically when you install a package, you can add a `--save` flag: `npm install express --save`.
 > :bulb: If you only require a dependency as part of a development environment, then you could/should install the package in the "devDependencies".  This is accomplished by using the `--save-dev` parameter. For example: `npm install --save-dev mocha`.

4. Now that your packages are listed in `package.json`, npm will always know which dependencies are required for your app. If you ever need to restore your packages, you can run `npm install` from your package directory.

> :bulb: When you distribute your application, we recommend adding the `node_modules` folder to `.gitignore` so that you don't clutter your repo with needless files. This also makes it easier to work with multiple platforms. If you want to keep things as similar as possible between machines, npm offers many options that enable you to fix the version numbers in `package.json`, and even more fine-grained control with `npm-shrinkwrap.json`.

### Publishing npm packages to the registry
Once you've created a package, publishing it to the world is only one command away!

`C:\src\my-express-app> npm publish`

> :bulb: Use npm's private modules.

> :triangular_flag_on_post: **TODO** Add description about how to authorize the machine using `npm adduser`.

### Local vs. global packages
There are two types of npm packages - locally installed packages and globally installed packages. It's not an exact science, but in general...
* Locally installed packages are packages that are specific to your application
* Globally installed packages tend to be CLI tools and the like

We went through locally installed packages above, and installing packages globally is very similar. The only difference is the `-g` command.

1. `npm install http-server -g` will install the module globally.

  > :bulb: The module will be installed to the path indicated by `npm bin -g`.
  
2. Run `http-server .` to start a basic fileserver from any directory.

### And much more!
* [npm docs and tutorials](https://docs.npmjs.com/)
* [Laurie Voss - npm past, present, and future](https://www.youtube.com/watch?v=-fqu-5IuOkc)

## Configuring your Windows development environment
### Command line console and other useful tools
One of the pain points we hear from users is that the command line console in Windows could use some work. We hear ya, and we're [working on it](https://wpdev.uservoice.com/forums/266908). In the meantime, we want to enable you to have the best experience possible. So here are some links to recommended tools to complement your existing experience.
* **cmd:** cmd has had some improvements in Windows 10, so be sure to check it out if you abandoned ship in the past :smiley:. When you're working with Node.js, chances are you'll be spending a bit more time in the console, so it's well worth brushing up on your CLI commands.
* **PowerShell:** PowerShell is a powerful object-oriented shell (as opposed to a text-based shell). It's a bit of a learning curve, but well worth it. It also has a bunch of aliases for commands, like `ls`, that'll make bash-happy people feel more at home. Here's a [good walkthrough](https://developer.rackspace.com/blog/powershell-101-from-a-linux-guy/) of some PowerShell commands from a *nix perspective, and there are [many other resources](https://technet.microsoft.com/en-us/scriptcenter/dd742419.aspx) to help you get started.
* **Chocolatey:** [Chocolatey](https://chocolatey.org) is the apt-get of Windows. There are also some other alternatives like Ninite which have their own advantages, but Chocolatey is the most commonly used.
* **Git:** `choco install git`.
* **nvm-windows:** https://github.com/coreybutler/nvm-windows - there are new versions of Node.js coming out all the time, so it can be annoying to migrate between versions. nvm-windows makes it way easier to switch between various versions.
* **npm-windows-upgrade:** npm is shipped with Node.js, and upgrading on Windows often requires manual upgrade steps. npm-windows-upgrade makes this process much easier. Install it by running `npm install npm-windows-upgrade -g`, and run the command by running `npm-windows-upgrade`.
* **terminal emulators:** cmder and ConEmu.
* **Cygwin:** [Cygwin](http://cygwin.com/index.html) can be handy if you're more familiar with bash, or are trying to use a Node app that assumes a *nix environment. Cygwin is a distribution of popular GNU and other open source tools running on Microsoft Windows. The core part is the Cygwin library which provides the POSIX system calls and environment these programs expect.
* **Putty:** ssh client.
* **WinSCP:** free FTP client.
* **Fiddler:** a web debugging tool. In general, people use it for the browser-side debugging, but you can also [configure it](http://stackoverflow.com/questions/8697344/can-a-proxy-like-fiddler-be-used-with-node-jss-clientrequest) to view server-side requests from Node.js.

> :triangular_flag_on_post: **TODO** Provide more dev environment options and a PowerShell script to make things easier.

> :chart_with_upwards_trend: **IN PROGRESS** We're currently planning the next Windows release, so it's a great time to let us know your biggest command line pain points! 

### Editors and IDEs
* **[Visual Studio Code](https://code.visualstudio.com/)** is a light weight code editor. Yet, it offers powerful capabilies in [editing](https://code.visualstudio.com/Docs/editor/editingevolved),  [debugging](https://code.visualstudio.com/Docs/editor/debugging), and [git integration](https://code.visualstudio.com/Docs/editor/versioncontrol) for Node.js development. It is free and available on your favorite platform - Windows, Mac, and Linux. For more information, check out: http://johnpapa.net/visual-studio-code.

* **[Node.js Tools for Visual Studio](https://aka.ms/explorentvs)** is a free, open-source extension that turns Visual Studio into a powerful Node.js IDE: intelligent code completions, advanced debugging and profiling, cloud deployment, unit-testing, REPL window, and more. For more information, check out this [overview video.](https://channel9.msdn.com/Blogs/Seth-Juarez/Nodejs-Tools-for-Visual-Studio)

### MAX_PATH explanation and workarounds
For the uninitiated, MAX_PATH is a limitation with many Windows tools and APIs that sets the maximum path character length to 260 characters. There are some workarounds involving UNC paths, but unfortunately not all APIs support it, and that's not the default. This can be problematic when working with Node modules because dependencies are often installed in a nested manner.

#### Workarounds

* :heart: Start in a short path (e.g. c:\src)
* `> npm install -g rimraf`
  delete files that exceed max_path
* `> npm dedupe`
  moves duplicate packages to top-level
* `> npm install -g flatten-packages`
  moves all packages to top-level, but can cause versioning issues
* :heart: Upgrade to npm@3
  * Ships with Node v5
  * Or… > npm install –g npm-windows-upgrade
* Future:
  * .NET file APIs:
    * The plan: https://www.youtube.com/watch?v=lpa2OFauASM
    * Progress :tada: https://github.com/dotnet/corefx/issues/645

For additional discussion, please see https://github.com/Microsoft/nodejstools/issues/69

### Compiling native addon modules
There are three primary reasons you might be interested in this section: 
* you have an existing C++ libary you'd like to take advantage of in your Node.js application
* you are interested in optimizing the performance of some code by writing it in C++
* you're running into dreaded `node-gyp` issues and have no idea what's going on.

#### Identifying native modules
How do you know if an npm package you want to install is a native module? Look for `nan`, `node-gyp`, or `node-pre-gyp` dependencies.

#### C++ and Node.js? Tell me more...
* Node.js addon documentation: https://nodejs.org/api/addons.html
* NodeSchool tutorial https://github.com/workshopper/goingnative

#### Environment setup and configuration:
##### Prerequisites
**Standalone C++ Build Tools (Technical Preview)**

1. Install [VC++ Build Tools Technical Preview](https://www.microsoft.com/en-us/download/details.aspx?id=49512)
> :bulb: [Windows 7 only] requires [.NET Framework 4.5.1](http://www.microsoft.com/en-us/download/details.aspx?id=40773)

2. Install [Python 2.7](https://www.python.org/downloads/), and add it to your `PATH`, `npm config set python python2.7`
3. Launch cmd, `npm config set msvs_version 2015 --global` (this is instead of `npm install [package name] --msvs_version=2015` every time.)
4. *SO MUCH npm install* :tada: 

**Visual Studio 2015** (takes longer)
* Download Python 2.7 (3.x will not work)
* Download Visual Studio 2015 (free Community Edition and Express for Desktop work)

 > :bulb: During installation, be sure to check the the C++ option.

> :chart_with_upwards_trend: **IN PROGRESS** there are currently two efforts underway to make it easier to install native modules.
  * We recognize that installing full VS can be burdensome, so we're investigating ways to provide a bundle with just the required compiler dependencies on Windows. Watch [this thread](https://github.com/nodejs/node-gyp/issues/629) for updates.
  * There are [long-term](https://github.com/nodejs/build/issues/151) efforts underway to build and cache pre-compiled packages on a server to get rid of compiler dependencies altogether.

##### Verify it's working
Here are a few packages you can try installing to see if your environment is set up properly.
* bson
* bufferutil
* kerberos
* node-sass
* sqlite3
* phantomjs
* utf-8-validate

##### Resolving common issues
![native-cheatsheet](https://cloud.githubusercontent.com/assets/762848/11049315/4b070502-86f2-11e5-8969-606bb9fa9959.png)


##### Deploying native modules
Sometimes, when deploying a native module to production, oftentimes it is not possible to set up the production machine with all the required prerequisites to build the native addon. Therefore, building locally or on a CI server and deploying `node_modules` may be the best option assuming there aren't any platform differences between the development and deployment machines.

## Writing cross-platform apps
* http://shapeshed.com/writing-cross-platform-node/
* https://gist.github.com/domenic/2790533

## Deployment
> :triangular_flag_on_post: **TODO**
* Continuous integration with VSO.
* Docker and containers.
* Cross-platform remote debugging.
* iisnode:
  * [GitHub repo](https://github.com/tjanczuk/iisnode/wiki) and [wiki](https://github.com/tjanczuk/iisnode/wiki)
  * [Scott Hanselman's blog post](http://www.hanselman.com/blog/InstallingAndRunningNodejsApplicationsWithinIISOnWindowsAreYouMad.aspx)

## Application-specific tips and tricks
> :triangular_flag_on_post: **TODO**
* Setting up SQL.
* .NET in-process using Edge.js.
* [node-windows](https://github.com/coreybutler/node-windows): Windows services, logging, and commands using Node.js.

### Setting up and working with MongoDB

1. [Install MongoDB on Windows](https://docs.mongodb.org/manual/tutorial/install-mongodb-on-windows/)
2. Install the MongoDB npm package in the project root by running `npm install mongodb`
3. *(Optional, but helpful for IntelliSense support in Visual Studio Code)* Install the type definition file for MongoDB with `tsd install mongodb`
4. Load the mongodb module: `var MongoClient = require('mongodb').MongoClient;`
5. [Determine your MongoDB connection string URI](https://docs.mongodb.org/manual/reference/connection-string/) and set it to a variable: `var mongoUrl = '...'`;
 > Example: mongodb://localhost:27017/yourDatabaseName
 
 > :bulb: MongoDB by default listens on 27017, but to verify this open the MongoDB log file `mongod.log` in the log directory and navigate the line that shows the port number (i.e. `[initandlisten] waiting for connections on port 27017`)
6. Access your MongoDB database by calling `MongoClient.connect()` and query the returned database
```javascript
MongoClient.connect(mongoUrl, function (err, db) {
    if (!err) {
        // query the collection and return a cursor to use 
        // to access the data
        //
        // note: calling find() with no parameters is the equivalent 
        // of pulling all documents in the collection with no 
        // predicate
        var cursor = db.collection('yourCollectionName').find();
        
        // loop through all of the documents returned by the 
        // query
        cursor.each(function (err, element) {
            if (!err && element) {
                // do something with the document
            }
            // if error and the document are undefined then 
            // we have reached the no-more-documents condition
            if (!err && !element) {
                console.log('done!');
            }
        });
    }
});
```

## Node.js + Microsoft products, services, and contributions
* [**Visual Studio Code**](https://code.visualstudio.com/): Lightweight cross-platform editor for building and debugging modern web and cloud applications.
* [**Node.js Tools for Visual Studio**](https://www.visualstudio.com/features/node-js-vs): Free, open-source extension that turns Visual Studio into a powerful Node.js development environment.
* [**TypeScript**](https://www.npmjs.com/package/typescript): TypeScript is a language for application scale JavaScript development.
  * Also useful for working with typescript is [tsd](https://www.npmjs.com/package/tsd), which enables you to quickly download TypeScript definition files.
* [**Azure SDK for Node.js**](https://github.com/Azure/azure-sdk-for-node#readme): We provide both [fine-grained modules](https://www.npmjs.com/~windowsazure) for different Microsoft Azure services which you can install separately, and an [all-up module](https://www.npmjs.com/package/azure) which contains everything.
* [**Application Insights**](https://www.npmjs.com/~msftapplicationinsights): Monitor your application's performance and usage with just a few lines of code.
* [**Node-Chakra and Windows IoT**](https://github.com/Microsoft/node#readme): This project enables Node.js to optionally use the Chakra JavaScript engine on Windows 10, allowing Node.js to run on Windows 10 IoT.
* [**Visual Studio Online**](https://www.npmjs.com/~vsonline)
* [**Docker Tools, `yo docker`**](https://github.com/Microsoft/DockerToolsDocs#yo-docker)
* [**Node.js Technical Steering Committee**](https://nodejs.org/en/foundation/tsc/) and [**Node.js Foundation Board**](https://nodejs.org/en/foundation/board/)
* [**Others**](https://www.npmjs.com/~microsoft)

> :triangular_flag_on_post: **TODO** Add other Microsoft services related to Node.js.
