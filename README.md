#WebSocket Server API#


This repository is the home of the [Web Socket Server API specification](http://w3c-jseverywhere.github.io/websocket-server/) being worked on by the [Client and Server JavaScript APIs Community Group](http://w3.org/community/jseverywhere) for Packaged Web apps and Server-Side JS implementations.


##Concepts##

This specification reuse concepts and content from the following W3C recommendations or Draft:

* [The WebSocket API](http://www.w3.org/TR/websockets/): compatible with [WebSocket](http://www.w3.org/TR/websockets/#websocket) the Interface and its JS Binding
* [Web Workers](http://www.w3.org/TR/workers/): multiple connections handling via ports as in [SharedWorker](http://www.w3.org/TR/workers/#shared-workers-and-the-sharedworker-interface) interface
* [Raw Socket API](http://www.w3.org/TR/raw-sockets/): Packages Web App Security context
* [HTML5 Web Messaging](http://www.w3.org/TR/webmessaging): handling port and origin

In Short, the proposed `WebSocketServer` interface looks like the `WebSocket`interface but messaging behave with the `SharedWorker` port mechanism.


##How to use?##

```
var webSocketServer = new WebSocketServer(urlToListen, serverID);
webSocketServer.onconnect = function handleClient(event) {
	var port = event.port[0];

	// security related informations
	var origin = port.origin; // check same origin or CORS access
	var cookies = port.cookies; // initial HTTP cookies
	var authorization = port.authorization; // initial HTTP Authorization header
	var remoteURI = port.remoteURI; // ex: "tcp:194.43.12.5:80"

	// handle client messages
	port.onmessage = function handleMessage(msgEvent) {
		// send a message to a specific client
		port.postMessage(message);
	}
};

// send basic message to all connected clients
webSocketServer.send(message);

webSocketServer.onmessage = function (msgEvent) {
	// global handling of client message

	var origin = msgEvent.origin; // check same origin or CORS access
	var cookies = msgEvent.cookies; // initial HTTP cookies
	var authorization = msgEvent.authorization; // initial HTTP Authorization header
	var remoteURI = msgEvent.remoteURI; // ex: "tcp:194.43.12.5:80"
};

```

The optionnal `serverID` parameter allows to work with the same WebSocketServer reference from any worker or iframe, as well as from any thread in multi-threaded SSJS implementation.


##How to contribute?##

Every one is welcome to contribute to this specification.
If not already a member, start by joining the community group, which is open to anyone, and present yourself [community group mailing list](public-jseverywhere@w3.org). Then create an issue on the github repository and mention it in another amail to the mailing list. You may join a pull request with edition suggestions.


##Useful links##

* The specification: http://w3c-jseverywhere.github.io/websocket-server/
* A related wiki page: http://www.w3.org/community/jseverywhere/wiki/Web_Sockets
* The Community Group homepage: http://w3.org/community/jseverywhere
* The Community Group mailing-list: http://lists.w3.org/Archives/Public/public-jseverywhere/