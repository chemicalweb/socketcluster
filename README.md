SocketCluster
======

[![Join the chat at https://gitter.im/SocketCluster/socketcluster](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/SocketCluster/socketcluster?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

[![SocketCluster logo](https://raw.github.com/SocketCluster/socketcluster/master/assets/logo.png)](http://socketcluster.io/)

Complete documentation available at: http://socketcluster.io/

## Change log

**14 June 2015** (v2.2.26)

Note that there has been an important fix in iocluster (a submodule of SC).
The iocluster module is the one which provides the scServer.global object. There was a major bug which prevented it from storing data properly.
This has been fixed in iocluster v2.4.6.
To update iocluster on an existing app, you should:

1. Navigate to your app's node_modules/socketcluster/ directory
2. Then from there, use the command: ```rm -R -f ./node_modules/iocluster```
3. ```npm cache clean```
4. ```npm install iocluster```
5. Check ./node_modules/iocluster/package.json to confirm that the version number is '2.4.6'

This is only relevant if you want to use the global object to store volatile data, otherwise it shouldn't affect 
SC in any way.

**6 June 2015** (v2.2.25)

- SocketCluster client - Major refactoring was undertaken - This should make the code much more robust and maintainable.
- SocketCluster client - The 'status' event was removed - Instead, you can now get the status object as the first argument to the 'connect' event handler.
- Authentication is now localStorage-based (it falls back to storing directly on the instance if localStorage is not supported) instead of cookie-based. This should allow the client authentication feature to work in more places including mobile.
- Authentication is now fully customizable on both the client and server so it can integrate with any existing token-based solution - Details on how to do this will be posted on the website at some point.

**4 May 2015** (v2.2.9)

SC2 is now the default version of SocketCluster so to install SC2, it's now:

```bash
npm install -g socketcluster
```

```bash
socketcluster create myProject
```

You can still install SC1, but the package name has changed:

```bash
npm install -g sc1
```

```bash
sc1 create myProject
```

## Introduction

SocketCluster is a fast, highly scalable HTTP + realtime server engine which lets you build multi-process 
realtime servers that make use of all CPU cores on a machine/instance.
It removes the limitations of having to run your Node.js server as a single thread and makes your backend 
resilient by automatically recovering from worker crashes and aggregating errors into a central log.

Follow the project on Twitter: https://twitter.com/SocketCluster
Subscribe for updates: http://socketcluster.launchrock.com/

## Memory leak profile

SocketCluster has been tested for memory leaks.
The last full memory profiling was done on SocketCluster v0.9.17 (Node.js v0.10.28) and included checks on load balancer, worker and store processes.

No memory leaks were detected when using the latest Node.js version.
Note that leaks were found when using Node.js versions below v0.10.22 - This is probably the Node.js 'Walmart' memory leak - Not a SocketCluster issue.

## Main Contributors

- Jonathan Gros-Dubois
- Nelson Zheng
- wactbprot (nData)
- epappas (nData)
- Gabriel Muller

## Installation

There are two ways to install SocketCluster.

### The easy way (Sets up boilerplate - Ready to run):

Setup the socketcluster command:

```bash
npm install -g socketcluster
```

OR 

```bash
sudo npm install -g socketcluster
```

Then

```bash
socketcluster create myapp
```

Once it's installed, go to your new myapp/ directory and launch with:

```bash
node server
```

Access at URL http://localhost:8000/

### The hard way (More modular - Separate server and client):

```bash
npm install socketcluster
```

You will also need to install the client separately which you can get using the following command:

```bash
npm install socketcluster-client
```

The socketcluster-client script is called socketcluster.js (located in the main socketcluster-client directory) 
- You should include it in your HTML page using a &lt;script&gt; tag in order to interact with SocketCluster.
For more details on how to use socketcluster-client, go to https://github.com/SocketCluster/socketcluster-client

It is recommended that you use Node.js version >=0.10.22 due to memory leaks present in older versions.

### Using over HTTPS

In order to run SocketCluster over HTTPS, all you need to do is set the protocol to 'https' and 
provide your private key and certificate as a start option when you instantiate SocketCluster - Example:

```js
var socketCluster = new SocketCluster({
  balancers: 1,
  workers: 3,
  stores: 3,
  port: 8000,
  appName: 'myapp',
  workerController: 'worker.js',
  protocol: 'https',
  protocolOptions: {
    key: fs.readFileSync(__dirname + '/keys/enc_key.pem', 'utf8'),
    cert: fs.readFileSync(__dirname + '/keys/cert.pem', 'utf8'),
    passphrase: 'passphase4privkey'
  }
});
```

The protocolOptions option is exactly the same as the one you pass to a standard Node HTTPS server:
http://nodejs.org/api/https.html#https_https_createserver_options_requestlistener

Note that encryption/decryption in SocketCluster happens at the LoadBalancer level (SocketCluster launches one or more 
lightweight load balancers to distribute traffic evenly between your SocketCluster workers).
LoadBalancers are responsible for encrypting/decrypting all network traffic. What this means is that your code (which is in the worker layer)
will only ever deal with raw HTTP traffic.

## Contribute to SocketCluster

- More integration test cases needed
- Unit tests
- Efficiency/speed - faster is better!
- Suggestions?

To contribute; clone this repo, then cd inside it and then run npm install to install all dependencies.

## License

(The MIT License)

Copyright (c) 2013-2015 TopCloud

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the 'Software'), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.