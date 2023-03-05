---
title: "Diving into WebAuthn"
datePublished: Wed Feb 01 2023 13:25:02 GMT+0000 (Coordinated Universal Time)
cuid: cldlpau2l000009mihav40833
slug: diving-into-webauthn
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/RMIsZlv8qv4/upload/7c9c7028eece402c090136385a494858.jpeg
tags: authentication, javascript, webauthn

---

As a developer, you may have already heard about the new standard in online authentication - Web Authentication (WebAuthn). This new standard offers a more secure and user-friendly alternative to the traditional password-based authentication system.

So, let's dive into the steps of integrating WebAuthn into your website or application.

> 💡 **Feel tired?** Jump down to the bottom of the post to get the full code

---

**Setting up authentication credentials:** The first step is to set up authentication credentials on the user's device. This could be a security key or biometric factor, which will be used for authentication when accessing the website.

Here's an example of how to create a new public key credential:

```javascript
const publicKey = {
  challenge: new Uint8Array([...challenge data...]),
  rp: {name: 'Example RP', id: 'example.com'},
  user: {
    id: new Uint8Array([...user ID...]),
    name: 'Example User',
    displayName: 'Example User'
  },
  pubKeyCredParams: [{type: 'public-key', alg: -7}],
  timeout: 60000,
  attestation: 'direct'
};

navigator.credentials.create({ publicKey })
  .then((newCredentialInfo) => {
    // Send newCredentialInfo to the server for registration
  });
```

**Registering credentials with the website:** Once the user's authentication credentials have been set up, the next step is to register them with the website. This process creates a secure connection between the user's device and the website.

That's easy!

```javascript
navigator.credentials.create({ publicKey })
  .then((newCredentialInfo) => {
    // Send newCredentialInfo to the server for registration
  });
```

**Logging in with WebAuthn:** Finally, when the user logs in to the website, they will be prompted to use WebAuthn for authentication. The user will then use their registered biometric factor or security key to authenticate themselves.

```javascript
const publicKey = {
  challenge: new Uint8Array([...challenge data...]),
  rpId: 'example.com',
  timeout: 60000,
  allowCredentials: [{
    id: new Uint8Array([...credential ID...]),
    type: 'public-key',
    transports: ['usb', 'nfc', 'ble', 'internal']
  }]
};

navigator.credentials.get({ publicKey })
  .then((assertion) => {
    // Send the assertion to the server for verification
  });
```

And there you have it! By following these steps and code examples, you'll be able to integrate WebAuthn into your website or application in no time.

Here's the full code example of registering and logging in with WebAuthn:

```javascript
// Setting up authentication credentials
const publicKey = {
  challenge: new Uint8Array([...challenge data...]),
  rp: {
    name: 'Example RP',
    id: 'example.com'
  },
  user: {
    id: new Uint8Array([...user ID...]),
    name: 'Example User',
    displayName: 'Example User'
  },
  pubKeyCredParams: [
    {
      type: 'public-key',
      alg: -7
    }
  ],
  timeout: 60000,
  attestation: 'direct'
};

navigator.credentials.create({ publicKey })
  .then((newCredentialInfo) => {
    // Registering the authentication credentials with the website
    // Send newCredentialInfo to the server for registration

    // Logging in with WebAuthn
    const publicKey = {
      challenge: new Uint8Array([...challenge data...]),
      rpId: 'example.com',
      timeout: 60000,
      allowCredentials: [{
        id: new Uint8Array([...credential ID...]),
        type: 'public-key',
        transports: ['usb', 'nfc', 'ble', 'internal']
      }]
    };

    navigator.credentials.get({ publicKey })
      .then((assertion) => {
        // Send the assertion to the server for verification
      });
  });
```