#WebSocket Server API#


This repository is the home of the [Web Socket Server API specification](http://w3c-jseverywhere.github.io/websocket-server/) being worked on by the [Client and Server JavaScript APIs Community Group](http://w3.org/community/jseverywhere) for Packaged Web apps and Server-Side JS implementations. This API is considered for implementation in [Wakanda Server](http://wakandadb.org)


##Concepts##

This specification reuse concepts and content from the following W3C recommendations or Draft:

* [The WebSocket API](http://www.w3.org/TR/websockets/): compatible with [WebSocket](http://www.w3.org/TR/websockets/#websocket) the Interface and its JS Binding
* [Web Workers](http://www.w3.org/TR/workers/): multiple connections handling via ports as in [SharedWorker](http://www.w3.org/TR/workers/#shared-workers-and-the-sharedworker-interface) interface
* [Raw Socket API](http://www.w3.org/TR/raw-sockets/): Packages Web App Security context and [TCPServerSocket](http://raw-sockets.sysapps.org/#interface-tcpserversocket) Interface
* [HTML5 Web Messaging](http://www.w3.org/TR/webmessaging): handling port and origin

In Short, the proposed `WebSocketServer` interface looks like the `WebSocket`interface but messaging behave with the `SharedWorker` port mechanism.


##How to use?##

###Dedicated Socket Workers###

```JavaScript
var webSocketMetaData = self.webSocket;

self.onmessage = function (msgEvent) {
	self.postMessage(message);
};

```

###Shared Socket Workers###
```JavaScript

self.onconnect = function handleClient(event) {
	var port = event.ports[0];
	var webSocketMetaData = event.webSocket;

	// handle client messages
	port.onmessage = function handleMessage(msgEvent) {
		// send a message to a specific client
		port.postMessage(message);
	}
};
```

##Interfaces##

```JavaScript
Interface WebSocketServer {
	void addWebSocketHandler(String workerPath, String id, Boolean dedicatedOrShared)
	void removeWebSocketHandler(String id)
}
```

```JavaScript
Interface WebSocketWorkerScope: DedicatedWorkerGlobalScope {
	readonly attribute WebSocketMeta webSocket
}
```

```JavaScript
Interface WebSocketSharedWorkerScope: SharedWorkerGlobalScope {
}
```

```JavaScript
Interface WebSocketMeta {

  readonly attribute DOMString url;

  // ready state
  const unsigned short CONNECTING = 0;
  const unsigned short OPEN = 1;
  const unsigned short CLOSING = 2;
  const unsigned short CLOSED = 3;
  readonly attribute unsigned short readyState;
  readonly attribute unsigned long bufferedAmount;

  // networking
  readonly attribute DOMString extensions;
  readonly attribute DOMString protocol;

  // messaging
           attribute DOMString binaryType;
}
```

It is possible to connect to WebSocket SharedWorkers using the standard SharedWorker constructor with the same ID. In this situation the event parameter of the 'onconnect' handler won't have the specific "webSocket" property

##How to contribute?##

Every one is welcome to contribute to this specification.
If not already a member, start by joining the community group, which is open to anyone, and present yourself [community group mailing list](public-jseverywhere@w3.org). Then create an issue on the github repository and mention it in another amail to the mailing list. You may join a pull request with edition suggestions.


##Useful links##

* The specification: http://w3c-jseverywhere.github.io/websocket-server/
* A related wiki page: http://www.w3.org/community/jseverywhere/wiki/Web_Sockets
* The Community Group homepage: http://w3.org/community/jseverywhere
* The Community Group mailing-list: http://lists.w3.org/Archives/Public/public-jseverywhere/
