# NoobHub

# This is a fork of the original NoobHub repo, this fork is intended for only use with LÖVE2D as it cleans up the code to only have L2D specific code.

[![JavaScript Style Guide](https://img.shields.io/badge/code%20style-standard-brightgreen.svg)](http://standardjs.com/)

OpenSource multiplayer and network messaging for CoronaSDK, Moai, Gideros, LÖVE & Defold

Battle-tested and production ready. Handling thousands of CCU (concurrent users), serving hundreds of thousands multiplayer games daily, routing hundreds of messages per second, with months of uptime.

- Connections are routed through socket server with minimum latency, ideal for action games.
- Simple interface. Publish/subscribe paradigm in action.
- Server written on blazing fast Nodejs.
- Zero dependency. Works out of the box, no NPM ecosystem required. (for websocket bridge relies on `ws` module, read below)
- Socket connections, works great through any NAT (local area network), messages delivery is reliable and fast.
- Low CPU and memory footprint

Repo includes server code (so you can use your own server) and CoronaSDK/Moai/Gideros/LÖVE client. More clients to come.
You can test on my server, credentials are hardcoded in demo project!

Lua code may serve as an example of how LuaSocket library works.

## How to use it

START SERVER

```bash
        $ nodejs node.js
```

To make use of the websocket bridge, uncomment wsPort in the config.  
If so, make sure to do `npm install` as well.  
These are useful to serve browser clients. Irrelevant otherwise.

INITIALIZE

```lua
        hub = noobhub.new({ server = "127.0.0.1"; port = 1337; });
```

```js
const hub = noobhub.new({ server: '127.0.0.1', port: 2337 });
```

SUBSCRIBE TO A CHANNEL AND RECEIVE CALLBACKS WHEN NEW JSON MESSAGES ARRIVE

```lua
        hub:subscribe({
          channel = "hello-world";
        	callback = function(message)

        		if(message.action == "ping")   then
        			print("Pong!")
        		end;

        	end;
        });
```

```js
hub.subscribe({
  channel: 'hello-world',
  callback: (data) => {
    console.log('callback', data);
  }
});
```

SAY SOMETHING TO EVERYBODY ON THE CHANNEL

```lua
        hub:publish({
            message = {
                action  =  "ping",
                timestamp = system.getTimer()
            }
        });
```

```js
hub.publish({
  action: 'ping',
  timestamp: Date.now()
});
```

## Clients

- CoronaSDK
- Gideros
- Moai
- LÖVE
- Node.js
- PHP (debug console only)
- JS / Browser

## Tests

Simple acceptance test uses Nodejs client to test the server itself:

```bash
    $ ./run-tests.sh
    starting Noobhub server...
    NoobHub on :::1337
    running tests...
    tests ok
```

## Getting ready for production use

If you expect more than 1000 concurrent connections, you should increase limits on your server (max open file descriptors,
max TCP/IP connections) and optionally fine-tune your server's TCP/IP stack.
To make sure server process stays alive you might want to use tools such as forever.js or supervisord.

## Authors

- Igor Korsakov
- Sergii Tsegelnyk

## Licence

[WTFPL](http://www.wtfpl.net/txt/copying/)

## Official discussion thread

- [old] http://developer.coronalabs.com/code/noobhub
- [new] http://forums.coronalabs.com/topic/32775-noobhub-free-opensource-multiplayer-and-network-messaging-for-coronasdk
