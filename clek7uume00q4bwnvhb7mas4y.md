# Introduction to PWAs (Progressive Web Apps)

Progressive Web Apps (PWAs) are a new way of delivering web content that combines the best of both worlds - the ease of use of native apps and the accessibility of traditional websites. PWAs are designed to provide users with a fast and seamless experience, even on slow networks or when offline.

To get started with PWAs, it's important to understand the key features that make them different from traditional websites. Some of these features include:

* Offline support: PWAs can work offline or with limited connectivity, providing users with a seamless experience even when their internet connection is poor.
    
* Home screen icons: PWAs can be installed on a user's device and accessed just like a native app, with a dedicated icon on their home screen.
    
* Push notifications: PWAs can send push notifications to users, even when the app is not currently open.
    

So, how do you build a PWA? It starts with creating a web app that meets certain technical requirements, such as having a service worker, a web manifest file, and HTTPS. From there, you can enhance your PWA with features like offline support, push notifications, and a home screen icon.

Here's a code example of a basic service worker in JavaScript:

```javascript
// Register a service worker
if ('serviceWorker' in navigator) {
  window.addEventListener('load', function() {
    navigator.serviceWorker.register('/sw.js').then(function(registration) {
      // Registration was successful
      console.log('ServiceWorker registration successful with scope: ', registration.scope);
    }, function(err) {
      // registration failed :(
      console.log('ServiceWorker registration failed: ', err);
    });
  });
}
```

In this example, we're using the `navigator` object to check if the `serviceWorker` API is supported in the user's browser. If it is, we use the `window.addEventListener` method to wait for the page to load and then call `navigator.serviceWorker.register` to register our service worker file (`/sw.js`).

This is just a basic introduction to PWAs, but there's a lot more to learn and explore in this exciting area of web development. Whether you're building a new web app or looking to enhance an existing one, PWAs are a great way to provide users with a fast and seamless experience.

Here's a great starter for you to remix: [~easy-pwa](https://glitch.com/edit/#!/easy-pwa?path=README.md%3A12%3A20) on Glitch

# 👋