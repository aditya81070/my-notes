[Home](/)
# Service worker notes
  - [Registering service worker](#registering-service-worker)
    - [Checking Compatibility](#checking-compatibility)
  - [Scope](#scope)
  - [Service worker events](#service-worker-events)
    - [fetch event](#fetch-event)
    - [install event](#install-event)
    - [activate event](#activate-event)
  - [Life Cycle of a service worker](#life-cycle-of-a-service-worker)
    - [Insight into lifecycle of service worker](#insight-into-lifecycle-of-service-worker)
    - [Telling user about update](#telling-user-about-update)
    - [Updating the service worker](#updating-the-service-worker)
    - [SW Controller](#sw-controller)
  - [Caching the things](#caching-the-things)
    - [Opening a cache](#opening-a-cache)
    - [Putting data into the cache](#putting-data-into-the-cache)
    - [Getting data from the cache](#getting-data-from-the-cache)
    - [Deleting a cache](#deleting-a-cache)
  - [IndexDB](#indexdb)
    - [Shape of IndexDB](#shape-of-indexdb)


## Registering service worker
```js
navigator.serviceWorker.register('/sw.js', {
  // scope- service worker will have access to all the urls starting with scope
  scope: '/my-app/'
}).then(reg => {
  console.log(reg)
  console.log('service worker registered')
}).catch(err => {
  console.log(err)
})
```
### Checking Compatibility
```js
if(navigator.serviceWorker) {
  //register service worker
} else {
  // Do nothing
}
```

## Scope
The default scope will be the path to the `sw.js`. So just put the `sw.js` at thr right place.

## Service worker events
```js
self.addEventListener('install', function (event) {

});

self.addEventListener('activate', function(event){

});

self.addEventListener('fetch', function (event) {

});
```

### fetch event
Every request from the page that is controlled by service worker goes through the sw and fetch event is emitted. We can check the request using `event.request`.

```js
self.addEventListener('fetch', function(e) {
  e.respondWith(
    // a promise
  )
})
```
We can send a custom response with `event.respondWith`. It will take a response and return it. We can handle the different situations here like showing the 404 error page or offline status.

### install event
When a browser runs a service worker for the first time, install event is fired within it.
The browser won't let this service worker take control of the page until its `install` event is completed.

We use this opportunity to get everything from the network and create a cache for them.
```js
self.addEventListener('install', function(e) {
  e.waitUntil(
    //Promise
  )
})
```
`event.waitUntil()`: It is used to hold the service worker in installing state until tasks complete. If promise passed to it rejects then this sw is discarded. This is to ensure that sw is not installed until all the tasks in it's install events are successfully completed.

### activate event
This event is called when a service has taken over the control of the page and previous sw has gone.
We can use this event to delete the old caches.

```js
self.addEventListener('activate', function(e) {
  // delete all the cache that are not required anymore
})
```

## Life Cycle of a service worker
1. We have one window open and then we hit a refresh. Now this first refresh will open a new window client(in case it is a new page) and it will get our new service worker. `All additional request will bypass the service worker`.
2. Now if we again hit the refresh, the sw has taken the control of it and now every request will go through it.
3. If any changes have done to the sw then `it will be in waiting state`. It will not take control of the page until the already opened page is closed or we move to a completely different page. This ensure that our page uses one service worker at a time.

### Insight into lifecycle of service worker
We can programmatically control the lifecycle of a sw. When we register a sw, it gives us a registration object that have various method and properties.
```js
navigator.serviceWorker.register('/sw.js').then(reg => {
reg.unregister()
reg.update()

//properties
reg.installing
reg.waiting
reg.active
})
```
* If `reg.waiting` is true then we know that there is a sw that is waiting to take over.
* Registration object will emit `updateFound` when new update is found and `reg.installing` become a new worker.

On this new worker, we can check the state.
```js
var sw = reg.installing //new worker
console.log(sw.state); //logs installing -> sw is installing
// installed-> sw has installed but not yet activated
// activating-> sw is activating but not ye activated
// activated -> sw has been activated and has taken control over the page
// redundant-> sw has been discarded either by a new sw or it is not installed correctly
```
sw also fire `statechange` event whenever state property changes.
```js
sw.addEventListener('statechange', function(e) {

})
```

### Telling user about update
```js
if(reg.waiting) {
  // update ready
  // tell the user about it
}
if(reg.installing) {
  // there is an update in progress
  reg.installing.addEventListener('statechange', function() {
    if(this.state === 'installed') {
      // update ready
    }
  })
}
// otherwise we listen for `updateFound` event
reg.addEventListener('updatefound', function() {
  reg.installing.addEventListener('statechange', function() {
    if(this.state === 'installed') {
      // update ready
    }
  })
})
```

### Updating the service worker
1. A sw can use `self.skipWaiting` to skip waiting and take the control straight away.
2. We can send any message to sw from our `indexController` or `main.js` using `reg.installing.postMessage(data)`.
3. A sw can listen to message events-
   ```js
   self.addEventListener('message', function(e) {
     // e.data
     // here you can call `self.skipWaiting()`
   })
   ```
4. To inform the page that service worker has been updated and reload the page. We can listen to `controllerchange` event.
   ```js
   navigator.serviceWorker.addEventListener('controllerchange', function(e) {
     //navigator.serviceWorker.controller has changed
   })
   ```

### SW Controller
`navigator.serviceWorker.controller` refers to the sw that is controlling th page.
```js
if(!navigator.serviceWorker.controller) {
  // this page didn't loaded by service worker
}
```
## Caching the things
Browser provides a `cache` api that provies a `caches` global object

### Opening a cache
```js
caches.open('cache-name').then(cache => {
  console.log(cache);
})
```
A cache box contains `requst-response` pairs from any secure origin.

### Putting data into the cache
**cache.put(request, response)**: It will take a `request or url` and `response` and will add to the cache.

**cache.addAll([urls])**: It will take an array of request or url and fetch them and cache the request-response pair. This operation is atomic, if any of them fail to cache, then nothing will be added to cache.

### Getting data from the cache
**cache.match(request)**: It will take a `request or url` and will return promise if found otherwise `null`.

**caches.match(request)**: It also does the same thing but it will try to find in all the caches including old ones.

### Deleting a cache
**caches.delete(cacheName)**: It will delete cache called `cacheName`.

**caches.keys()**: It will return the name of all caches.

Both of them returns a promise.

## IndexDB

### Shape of IndexDB
We can have any number of databases with any name into IndexDB.
* Database
  * Object Stores
    * Data
* Key will be unique in object store to identify different values.
* Read and write operations must be part of `transaction` in indexDB. If one of the operation fails then no operation will be completed.
* We can use multiple type of shapes so data in object store will be sorted according to that index.
