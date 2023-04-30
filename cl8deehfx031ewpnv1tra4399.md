---
title: "Sending WhatsApp messages with Python and PyAutoGUI"
datePublished: Thu Sep 22 2022 18:39:15 GMT+0000 (Coordinated Universal Time)
cuid: cl8deehfx031ewpnv1tra4399
slug: sending-whatsapp-messages-with-python-and-pyautogui
canonical: https://tiagorangel2011.medium.com/how-to-send-a-whatsapp-message-with-python-75a69eb695c6
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1675256835800/f35f39b2-ad57-4e4c-9b12-fdb2055683fb.png
tags: python, python3, whatsapp, pyautogui

---

> This post was originaly posted on my [medium](https://tiagorangel2011.medium.com/how-to-send-a-whatsapp-message-with-python-75a69eb695c6)

# The project

Hi everyone! Today I’m building a text-to-whatsapp python app

Bellow, I have the code, and I will explain everything in the comments.

Oh, but first run the following commands:

```bash
$ pip3 install web-browser
$ pip3 install pyautogui
```

And here we have our code:

```python
import webbrowser # import the webbrowser library, see line bellow
import pyautogui #import the library that will simulate clicks
import time # for sleep
msg = input("Message: ")
phone = input("Telephone number: +")
sim = input("Press enter to send") # Verify
webbrowser.open("whatsapp://send?text=" + msg + "&phone=" + phone) # Open whatsapp's URL protocol. This will open the Whatsapp app and open the send dialog
time.sleep(9) #Give time for the program to execute
pyautogui.click(1870, 984) # Send message (simulate click on send button)
time.sleep(1) # wait 1 sec
pyautogui.click(1900, 4) # simulate click to close whatsapp
```

To use this code, you must have Whatsapp installed and a 1536x864 (width x height screen. If you don’t have a screen of that dimensions, open WhatsApp, execute the following code, and immediately position your mouse over the close button and the send button of WhatsApp. The program sleeps 5 seconds after execution. Then, change the X and Y coordinates of the *pyautogui.click()* functions to the results.

```python
import pyautoguiimport
timetime.sleep(5)
print(pyautogui.position()) # Gives you: _Point(x=XXX, y=XXX)_
```

Thanks for reading!