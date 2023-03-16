---
title: "5 Cool Javascript APIs you didn't know 🤯"
datePublished: Thu Mar 16 2023 13:12:39 GMT+0000 (Coordinated Universal Time)
cuid: clfb4sjtf02ediinv4zb76af4
slug: 5-cool-javascript-apis-you-didnt-know
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/NPFfuGST5YA/upload/fb5b4661e24a71471f9f04e9c4a457f6.jpeg
tags: javascript, web-api, web-apis

---

Hey there, coding enthusiasts! Are you tired of using the same old JavaScript APIs? Well, we've got some great news for you! We're here to introduce you to five awesome JavaScript APIs that you might not have heard of before.

Let's get started!

1. ### The SpeechRecognition API 🗣️
    

Are you tired of typing out long commands or code lines? Well, you can now take a break from the keyboard and use your voice with the SpeechRecognition API! This API enables you to control your web application using voice commands, making it easier and faster for you to get things done. It's like having your own personal assistant!

```javascript
const recognition = new webkitSpeechRecognition();
recognition.onresult = function(event) {
  const transcript = event.results[0][0].transcript;
  console.log(transcript);
};
recognition.start();
```

1. ### The Intersection Observer API 👀
    

Do you struggle with detecting when an element is in view on your website? The Intersection Observer API is here to save the day! This API simplifies the process of detecting when an element enters or exits the viewport. Say goodbye to complex code and hello to a simple solution!

Look at this:

```javascript
const options = {
  root: null,
  rootMargin: '0px',
  threshold: 1.0
};

const observer = new IntersectionObserver((entries, observer) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      console.log('Element is in view!');
    } else {
      console.log('Element is not in view.');
    }
  });
}, options);

observer.observe(document.querySelector('#myElement'));
```

1. ### The Web MIDI API 🎹
    

Do you love music and coding? The Web MIDI API combines both of these passions, allowing you to connect your web application to MIDI devices such as keyboards and synthesizers. You can even send and receive MIDI messages to control and manipulate sound in real-time.

Here's an example of how you can use the Web MIDI API:

```javascript
navigator.requestMIDIAccess()
  .then(midi => {
    console.log('MIDI access granted!');
    const inputs = midi.inputs.values();
    for (let input = inputs.next(); input && !input.done; input = inputs.next()) {
      console.log('MIDI input device detected:', input.value.name);
    }
  })
  .catch(err => {
    console.log('MIDI access denied:', err);
  });
```

1. ### The DeviceOrientation API 🧭
    

Do you want to create a web-based game that uses the device's orientation to control the gameplay? The DeviceOrientation API is here to help! This API enables you to access the device's gyroscope and accelerometer data, giving you precise information about the device's orientation and movement. You can use this information to create immersive and interactive gaming experiences that are sure to impress.

```javascript
window.addEventListener('deviceorientation', event => {
  const alpha = event.alpha;
  const beta = event.beta;
  const gamma = event.gamma;
  console.log(`Device orientation: alpha = ${alpha}, beta = ${beta}, gamma = ${gamma}`);
});
```

1. ### The Payment Request API 💳
    

Do you run an e-commerce website and want to provide your users with a seamless and secure checkout experience? The Payment Request API is here to help! This API enables you to request and process payments directly within your website, making the checkout process quick and easy for your customers.

![Payment Plans Payment Plans Every Where - | Make a Meme](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fmedia.makeameme.org%2Fcreated%2Fpayment-plans-payment.jpg&f=1&nofb=1&ipt=c95f3adbd3e504a92d6c0981875296b37763a281c46ff7f6b7aee50efffc2ae6&ipo=images align="center")

Here's an example *(don't worry, you won't be sending money to no one)*:

```javascript
const supportedPaymentMethods = [
  {
    supportedMethods: 'basic-card',
    data: {
      supportedNetworks: ['visa', 'mastercard', 'amex'],
      supportedTypes: ['credit', 'debit']
    }
  }
];

const paymentDetails = {
  total: {
    label: 'Total',
    amount: {
      currency: 'USD',
      value: '10.00'
    }
  }
};

const options = {};

const request = new PaymentRequest(supportedPaymentMethods, paymentDetails, options);

request.show()
  .then(paymentResponse => {
    // Process the payment
  })
  .catch(err => {
    console.log('Payment request failed:', err);
  });
```

In conclusion, these five JavaScript APIs are awesome tools that can help you create better and more efficient web applications. So go ahead and give them a try! With these APIs in your toolbox, you're sure to impress your colleagues and friends alike. **Happy coding!**