---
title: "Let's make a conferencing app with Javascript!"
datePublished: Fri Feb 03 2023 12:22:39 GMT+0000 (Coordinated Universal Time)
cuid: cldohybcd02332envbnxahqfp
slug: lets-make-a-conferencing-app-with-javascript
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/5QgIuuBxKwM/upload/ca5ec5f2d68b4fd5a071c01df1c1460b.jpeg
tags: websockets, javascript, video-conferencing

---

In the previous article, I talked about [making a live stream with javascript](https://blog.tiagorangel.com/making-a-simple-live-stream-with-javascript). Now, we're going to see how to make a *real* conferencing app that uses WebRTC and sockets!

1. ## **Setting up the WebRTC Connection**
    

The first step in creating a video conferencing app is to establish a WebRTC connection between the participants. WebRTC is a technology that enables real-time communication in the browser, and it provides the necessary APIs to create peer-to-peer connections.

Here's an example of how to get access to the user's camera and microphone and set up a WebRTC connection using JavaScript:

```javascript
// Get access to the user's camera and microphone
navigator.mediaDevices.getUserMedia({ video: true, audio: true })
  .then(stream => {
    const localVideo = document.getElementById("local-video");
    localVideo.srcObject = stream;
  
    // Set up the WebRTC connection
    const peerConnection = new RTCPeerConnection();
    peerConnection.addStream(stream);
    peerConnection.onicecandidate = event => {
      if (event.candidate) {
        // send the candidate to the other participant
      }
    };
  })
  .catch(console.error);
```

In this example, we use the `navigator.mediaDevices.getUserMedia` function to get access to the user's camera and microphone. The returned stream is then assigned to a `<video>` element on the page to display the local video.

Next, we create an `RTCPeerConnection` object to set up the WebRTC connection. The `peerConnection.addStream` method is used to add the local stream to the connection, and the `peerConnection.onicecandidate` event is used to receive the ICE candidates, which are used to establish the connection.

1. ### **Exchanging SDP Offer and Answer**
    

Once the WebRTC connection is set up, the next step is to exchange the SDP offer and answer between the participants. SDP (Session Description Protocol) is a protocol used to describe multimedia sessions, and it's used to negotiate the media and network settings for the WebRTC connection.

Here's an example of how to create an SDP offer and send it to the other participant:

```javascript
peerConnection.createOffer()
  .then(offer => {
    peerConnection.setLocalDescription(offer);
    // send the offer to the other participant
  })
  .catch(console.error);
```

In this example, we use the `peerConnection.createOffer` method to create an SDP offer, and then set it as the local description using the `peerConnection.setLocalDescription` method. Finally, we send the offer to the other participant.

To receive the SDP offer and send an SDP answer, we can use the following code:

```javascript
peerConnection.setRemoteDescription(offer);
peerConnection.createAnswer()
  .then(answer => {
    peerConnection.setLocalDescription(answer);
    // send the answer to the other participant
  })
  .catch(console.error);
```

In this example, we use the `peerConnection.setRemoteDescription` method to set the received SDP offer as the remote description. Next, we create an SDP answer using the `peerConnection.createAnswer` method and set it as the local description using the `peerConnection.setLocalDescription` method. Finally, we send the answer to the other participant.

1. ### **Displaying the Remote Video**
    

Once the WebRTC connection is established, we can start streaming video between the participants. To display the remote video, we need to use the `RTCPeerConnection.ontrack` event.

Here's an example of how to display the remote video:

```javascript
peerConnection.ontrack = event => {
  const remoteVideo = document.getElementById("remote-video");
  remoteVideo.srcObject = event.streams[0];
};
```

In this example, we use the `peerConnection.ontrack` event to display the remote video. The `event.streams[0]` property contains the remote stream, which we can assign to a `<video>` element on the page.

1. ### **Using a Server for Signaling**
    

Finally, to complete our video conferencing app, we need a way to exchange messages between the participants. To do this, we can use a web server to handle the signaling. The signaling messages include the SDP offer and answer, as well as the ICE candidates.

Here's an example of how to send and receive signaling messages using WebSockets:

```javascript
const socket = new WebSocket("wss://example.com");

socket.addEventListener("open", event => {
  // send the offer or answer to the other participant
});

socket.addEventListener("message", event => {
  const message = JSON.parse(event.data);
  if (message.sdp) {
    // set the remote description
  } else if (message.candidate) {
    // add the ICE candidate
  }
});
```

In this example, we use a WebSocket connection to send and receive signaling messages between the participants. The `socket.addEventListener("open")` event is used to send the offer or answer to the other participant, and the `socket.addEventListener("message")` event is used to receive the signaling messages and update the remote description or add the ICE candidate.

1. ### Conclusion and full code
    

In this article, we have seen the steps to create a simple video conferencing app using JavaScript, WebRTC, and a web server. With these technologies, it's now easier than ever to create real-time communication applications in the browser. I hope you found this article helpful and informative, and I encourage you to try building your own video conferencing app! Here's the full code:

```javascript
// client code
// get the local video and audio stream
navigator.mediaDevices
  .getUserMedia({
    audio: true,
    video: true
  })
  .then(stream => {
    const localVideo = document.getElementById("local-video");
    localVideo.srcObject = stream;

    // create a RTCPeerConnection
    const peerConnection = new RTCPeerConnection();

    // add the local stream to the peer connection
    stream.getTracks().forEach(track => {
      peerConnection.addTrack(track, stream);
    });

    // handle the icecandidate event
    peerConnection.onicecandidate = event => {
      if (event.candidate) {
        socket.send(JSON.stringify({ candidate: event.candidate }));
      }
    };

    // handle the negotiationneeded event
    peerConnection.onnegotiationneeded = async () => {
      try {
        // create an SDP offer
        const offer = await peerConnection.createOffer();
        await peerConnection.setLocalDescription(offer);

        // send the offer to the other participant
        socket.send(JSON.stringify({ sdp: peerConnection.localDescription }));
      } catch (error) {
        console.error(error);
      }
    };

    // handle the track event
    peerConnection.ontrack = event => {
      const remoteVideo = document.getElementById("remote-video");
      remoteVideo.srcObject = event.streams[0];
    };

    // connect to the signaling server
    const socket = new WebSocket("wss://example.com");

    // handle the open event
    socket.addEventListener("open", event => {
      // send the offer or answer to the other participant
    });

    // handle the message event
    socket.addEventListener("message", event => {
      const message = JSON.parse(event.data);
      if (message.sdp) {
        // set the remote description
        peerConnection.setRemoteDescription(new RTCSessionDescription(message.sdp));

        // create an SDP answer if we received an offer
        if (peerConnection.remoteDescription.type === "offer") {
          peerConnection.createAnswer().then(answer => {
            peerConnection.setLocalDescription(answer);
            socket.send(JSON.stringify({ sdp: peerConnection.localDescription }));
          });
        }
      } else if (message.candidate) {
        // add the ICE candidate
        peerConnection.addIceCandidate(new RTCIceCandidate(message.candidate));
      }
    });
  })
  .catch(error => {
    console.error(error);
  });
```

```javascript
// server code
const WebSocket = require("ws");
const server = new WebSocket.Server({ port: 8080 });

server.on("connection", (socket, request) => {
  console.log("New WebSocket connection");

  socket.on("message", message => {
    const data = JSON.parse(message);

    // broadcast the message to all connected clients
    server.clients.forEach(client => {
      if (client !== socket && client.readyState === WebSocket.OPEN) {
        client.send(JSON.stringify(data));
      }
    });
  });
});
```

> ⚠️ **Warning!** This is just a simple example and in a real-world scenario, you would need to implement additional security measures, such as authentication and encryption, to ensure the confidentiality and privacy of the signaling data.