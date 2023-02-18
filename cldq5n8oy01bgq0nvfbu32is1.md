# The beginner's guide to encryption

*Encryption, the word that strikes fear into the hearts of many developers, but it's actually not as complicated as it sounds.* Think of it like a secret code that you used to send to your friends when playing hide and seek (except now it's used to protect your data). The idea is to scramble the original message into a coded form that only the intended recipient can read with the right key.

But what exactly is encryption and why should developers care? In simple terms, encryption is like locking up your diary with a key to keep your secrets safe from curious eyes. It's used to protect sensitive information from cyber criminals, hackers, and other malicious entities.

There are various forms of encryption:

| Name | How it works |
| --- | --- |
| Symmetric encryption | Uses the same key for both encryption and decryption |
| Asymmetric encryption | Uses a pair of keys (a public key for encryption and a private key for decryption). |
| Hashing | A one-way function used to protect the integrity of data by generating a unique value (hash) based on the original message |

In this beginner's guide, we'll focus on symmetric encryption, and specifically the Advanced Encryption Standard (AES) algorithm, one of the most widely used encryption standards in the world.

Here's a quick code example of symmetric encryption using AES in Python:

```python
import base64
import os
from cryptography.fernet import Fernet

def encrypt_message(message, password):
    # Generate a secure key from the password
    key = base64.urlsafe_b64encode(password.encode())
    f = Fernet(key)
    # Encrypt the message
    encrypted = f.encrypt(message.encode())
    return encrypted

def decrypt_message(encrypted, password):
    # Generate a secure key from the password
    key = base64.urlsafe_b64encode(password.encode())
    f = Fernet(key)
    # Decrypt the message
    decrypted = f.decrypt(encrypted)
    return decrypted.decode()

message = "I love pizza"
password = "password"
encrypted = encrypt_message(message, password)
decrypted = decrypt_message(encrypted, password)

print(f"Encrypted message: {encrypted}")
print(f"Decrypted message: {decrypted}")
```

In the code above, we first create a function `encrypt_message` that takes in a message and password and returns the encrypted message. We use the password to generate a secure key using the `base64` library, which encodes the password into a URL-safe string. This key is then used to initialize an instance of the Fernet class from the `cryptography` library, which provides the encryption and decryption functions.

Next, we create a function `decrypt_message` that takes in an encrypted message and password and returns the decrypted message. This function works similarly to the `encrypt_message` function, using the password to generate the secure key and using the Fernet class to decrypt the encrypted message.

So why is encryption important for developers? With the increasing number of data breaches and cyber attacks, encryption is essential in ensuring the confidentiality, integrity, and availability of sensitive information. It's like having a knight in shining armor to protect your precious data.