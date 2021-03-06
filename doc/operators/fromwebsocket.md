### `Rx.DOM.fromWebSocket(url, protocol, [observerOrOnNext])`
[&#x24C8;](https://github.com/Reactive-Extensions/RxJS-DOM/blob/master/src/websocket.js "View in source") 

Creates a WebSocket Subject with a given URL, protocol and an optional observer for the open event.

#### Arguments
1. `url` *(String)*: The URL of the WebSocket.
2. `protocol` *(String)*: The protocol of the WebSocket.
3. `openObserver` *(`Rx.Observer`)*: An optional Observer to capture the open event.
4. `closingObserver` *(`Rx.Observer`)*: An optional Observer capture the the moment before the underlying socket is closed.

#### Returns
*(`Subject`)*: A Subject which wraps a WebSocket.

#### Example
```js
// an observer for when the socket is open
var open = Observer.create(function(e) {
  console.info('socket open');
});

// an observer for when the socket is about to close
var closing = Observer.create(function() {
  console.log('socket is about to close');
});

// create a web socket subject
socket = Rx.DOM.fromWebSocket('ws://echo.websockets.org', null, open, closingObserver);

// send a message on the socket
// this will queue until the socket is open.
socket.onNext('test');

// subscribing creates the underlying socket and will emit a stream of incoming
// message events
socket.subscribe(function(e) {
  console.log('message: ', e.data); 
},
function(e) {
  // errors and "unclean" closes land here
  console.error('error: ', e);
},
function() {
  // the socket has been closed
  console.info('socket closed');
});
```

### closing an already open web socket neatly
```js
// create a web socket subject
socket = Rx.DOM.fromWebSocket('ws://echo.websockets.org', null, open, closingObserver);

// this is an echo service, so it'll echo this back
socket.onNext('close me');

// subscribing creates the underlying socket and will emit a stream of incoming
// message events
socket.subscribe(function(e) {
  // when we see our echoed response, let's kill the socket with a
  // custom error
  if(e.data === 'close me') {
    socket.onCompleted(); // closes with code 1000 and reason ""
  } 
},
function(e) {
  // errors and "unclean" closes land here
  console.error('error: ', e);
},
function() {
  // the socket has been closed
  console.info('socket closed');
});

```

### closing an already open web socket with a custom error
```js
// create a web socket subject
socket = Rx.DOM.fromWebSocket('ws://echo.websockets.org', null, open, closingObserver);

// this is an echo service, so it'll echo this back
socket.onNext('close me');

// subscribing creates the underlying socket and will emit a stream of incoming
// message events
socket.subscribe(function(e) {
  // when we see our echoed response, let's kill the socket with a
  // custom error
  if(e.data === 'close me') {
    // close with a specified status code and reason
    socket.onError({ code: 3001, reason: 'you told me to close' });
  } 
},
function(e) {
  // errors and "unclean" closes land here
  console.error('error: ', e);
},
function() {
  // the socket has been closed
  console.info('socket closed');
});

```

### Location

File:
- [`/src/websocket.js`](https://github.com/Reactive-Extensions/RxJS-DOM/blob/master/src/websocket.js)

Dist:
- [`rx.dom.js`](https://github.com/Reactive-Extensions/RxJS-DOM/blob/master/dist/rx.dom.js) | - [`rx.dom.compat.js`](https://github.com/Reactive-Extensions/RxJS-DOM/blob/master/dist/rx.dom.compat.js)

Prerequisites:
- If using `rx.js`
  - [`rx.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.js) | [`rx.compat.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.compat.js)
  - [`rx.binding.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.binding.js)
- [`rx.lite.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/rx.lite.js) | [`rx.lite.compat.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/rx.lite.compat.js)

NPM Packages:
- [`rx-dom`](https://preview.npmjs.com/package/rx-dom)

NuGet Packages:
- [`RxJS-Bridges-HTML`](http://www.nuget.org/packages/RxJS-Bridges-HTML/)

Unit Tests:
- [`/tests/tests.websocket.js](https://github.com/Reactive-Extensions/RxJS-DOM/blob/master/tests/tests.fromwebsocket.js)
