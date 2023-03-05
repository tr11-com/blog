---
title: "Making a simple live stream with javascript"
datePublished: Thu Feb 02 2023 11:56:49 GMT+0000 (Coordinated Universal Time)
cuid: cldn1l9e4000h0amj05q7ddor
slug: making-a-simple-live-stream-with-javascript
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/8zsBofKrhP8/upload/949b3bbbeec216df7448a84e2b9d4989.jpeg
tags: socketio, javascript, nodejs, live-streaming

---

Hey there! If you're looking to add a live stream feature to your website, Javascript can be a great tool to achieve this. In this post, I'll share some code examples to help you get started.

To create a live stream, you'll need to use the Media Recorder API. This API lets you capture streaming data from the user's camera and microphone, and then send it to a server for broadcast.

Here's an example of how to use the Media Recorder API to capture video and audio from the user's device:

```javascript
navigator.mediaDevices.getUserMedia({ video: true, audio: true })
  .then(stream => {
    const mediaRecorder = new MediaRecorder(stream);
    mediaRecorder.start();
  
    const chunks = [];
    mediaRecorder.addEventListener("dataavailable", event => {
      chunks.push(event.data);
    });
  
    mediaRecorder.addEventListener("stop", () => {
      const blob = new Blob(chunks, { type: mediaRecorder.mimeType });
      // send the blob to a server for broadcast
    });
  
    // stop recording after 10 seconds
    setTimeout(() => {
      mediaRecorder.stop();
    }, 10000);
  })
  .catch(console.error);
```

Once you have the streaming data, you can send it to a server for broadcast. To do this, you'll need to set up a server-side script that can receive the streaming data and broadcast it to viewers.

Here's an example of how to broadcast a live stream using Node.js and the `socket.io` library:

```javascript
const express = require("express");
const socketio = require("socket.io");
const http = require("http");

const app = express();
const server = http.createServer(app);
const io = socketio(server);

io.on("connection", socket => {
  socket.on("stream", data => {
    socket.broadcast.emit("stream", data);
  });
});

server.listen(3000);
```

In this example, the server listens for connections from viewers and broadcasts the streaming data to all connected clients using the `socket.io` library.

This is just a basic example to get you started. You can customize and extend this code to meet your specific requirements.

Here's the full code:

```javascript
// Client-side code for capturing the live stream
navigator.mediaDevices.getUserMedia({ video: true, audio: true })
  .then(stream => {
    const mediaRecorder = new MediaRecorder(stream);
    mediaRecorder.start();
  
    const chunks = [];
    mediaRecorder.addEventListener("dataavailable", event => {
      chunks.push(event.data);
    });
  
    mediaRecorder.addEventListener("stop", () => {
      const blob = new Blob(chunks, { type: mediaRecorder.mimeType });
      // send the blob to the server for broadcast
      socket.emit("stream", blob);
    });
  
    // stop recording after 10 seconds
    setTimeout(() => {
      mediaRecorder.stop();
    }, 10000);
  })
  .catch(console.error);
```

```javascript
// Server-side code for broadcasting the live stream
const express = require("express");
const socketio = require("socket.io");
const http = require("http");

const app = express();
const server = http.createServer(app);
const io = socketio(server);

io.on("connection", socket => {
  socket.on("stream", data => {
    socket.broadcast.emit("stream", data);
  });
});

server.listen(3000);
```

In this example, the client-side code captures the live stream using the Media Recorder API. The captured stream is then sent to the server as a blob of data using the `socket.emit` function.

The server-side code is built using the `express` framework, the [`socket.io`](http://socket.io) library, and the `http` module. The server listens for connections from clients and broadcasts the received stream data to all connected clients using the `socket.broadcast.emit` function.

It's important to note that this example is just a starting point, and you'll likely need to customize and extend it to meet your specific requirements.

With this complete code example, you should now have a good understanding of how to create a live stream using JavaScript. I hope this helps you get started on your project! Good luck!