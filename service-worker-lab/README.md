# Notes
- Service workers must be registered.
- Always start by checking if the browser supports service workers. The service
worker is exposed on the browser's [Navigator](https://developer.mozilla.org/en-US/docs/Web/API/Navigator) i.e. `window.navigator.serviceWorker`

## SW Life Cycle
- The service worker emits an `install` event at the end of registration (this is a good place for caching static assets). SW emits an  `activate` event when it takes control of the page (is often used to update caches).
```javascript
  self.addEventListener('install', function(event) {
    console.log('Service worker installing...');
    });

  self.addEventListener('activate', function(event) {
    console.log('Service worker activating...');
  });
```
- fetch events are received for every HTTP request (we could also create and return our own custom response with arbitrary resources).
```javascript
self.addEventListener('fetch', function(event) {
    console.log('Fetching:', event.request.url);
});
```

## SW Scope
- The promise returned by `register()` resolves to the registration object, which contains the service worker's scope.

**The default scope is the path to the service worker file, and extends to all lower directories. So a service worker in the root directory of an app controls requests from all files in the app.**

- Notice if `service-worker.js` is in a subdirectory (/below) then the service worker only controls requests for the requests in that subdirectory. The service worker's default scope is the path to the service worker file. Since the service worker file is now in app/below/, that is its scope. The console is now only logging fetch events for another.html, another.css, and another.js, because these are the only resources within the service worker's scope (app/below/).

- Can set an arbitrary scope using the options in serviceWorker.register().
```javascript
navigator.serviceWorker.register('service-worker.js',
{scope: './below'})
.then(function(registration) {
    console.log('Registered at scope:', registration.scope);
})```
