---
title: "How to implement lazy loading in your website"
datePublished: Thu Feb 09 2023 15:00:42 GMT+0000 (Coordinated Universal Time)
cuid: cldx88ou301wwfnnvbz4l882h
slug: how-to-implement-lazy-loading-in-your-website
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/yGPxCYPS8H4/upload/c2a7c381db92f8e107ee70c85b3ea6e9.jpeg
tags: javascript, html5, best-practices, lazy-loading

---

As web applications grow larger and more complex, it's important to consider how to optimize the user experience for slow internet connections or underpowered devices. One way to achieve this is by implementing lazy loading, which is a technique that delays loading of images or other content until it is needed.

Here's a simple implementation of lazy loading using the Intersection Observer API:

```javascript
const images = document.querySelectorAll("img");

const options = {
  root: null,
  rootMargin: "0px",
  threshold: 0.1
};

const observer = new IntersectionObserver(entries => {
  entries.forEach(entry => {
    if (entry.intersectionRatio > 0) {
      const image = entry.target;
      image.src = image.dataset.src;
      observer.unobserve(image);
    }
  });
}, options);

images.forEach(image => {
  observer.observe(image);
});
```

The Intersection Observer API makes it easy to detect when an element enters or leaves the viewport, which is ideal for lazy loading. In this example, we query all `img` elements on the page and observe each one. When the observer determines that an image has entered the viewport, it sets the `src` attribute to the value stored in the `data-src` attribute, effectively loading the image.

Note that this is just one example of how to implement lazy loading, and there are many other techniques and libraries available. No matter what approach you take, the goal is to make your web applications fast and responsive, regardless of the network or device conditions. For example, one of the easiest ways to lazy load images in HTML is by using the `loading="lazy"` property:

```javascript
<img src="/image.png" loading="lazy">
```

It's also recommended to lazy load content. An easy and fast way to use this is, for example, to make multiple fetches. Or you can even use WebSockets!