# Reading the latest Whatsapp messages using python and pyautogui

![Whatsapp logo](https://miro.medium.com/max/875/0*VSjseDkLPGooQg60 align="left")

(Photo by [Dima Solomin](https://unsplash.com/@solomin_d?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral))

> This post was originaly posted on my [medium](https://tiagorangel2011.medium.com/reading-the-latest-whatsapp-messages-with-python-79d7f754b36)

# Introduction

Hi everyone! I had built this some time ago this code and now I want to share it with everyone. My idea was to open WhatsApp, select the phone number, and read the messages, but then I needed to do this:

* Open WhatsApp using WhatsApp's web protocol
    
* Wait 10 seconds
    
* Focus on WhatsApp's window
    
* Drag the cursor to select the latest messages (unfortunatly, can't use ctrl+a to select everything)
    
* Copy everything
    
* Get the clipboard’s content
    

In the full code, the coordinates below may not be correct in your case, so remember to update them:

```python
import pyautogui  
import time  
time.sleep(5)  
print(pyautogui.position())
```

But first, you need to install the pyautogui (for automating) and the webbrowser (to open whatsapp) libraries:

```plaintext
$ pip3 install webbrowser  
$ pip3 install pyautogui
```

# The code

Finally, here’s the full code:

```python
import webbrowser
import pyautogui
import time
import pyperclip
phone = pyautogui.prompt('Telephone number')
webbrowser.open("whatsapp://send?phone=" + phone)
time.sleep(10)
pyautogui.click(1803, 916)
time.sleep(0.2)
pyautogui.dragTo(587, 127, 0.2, button='left')
pyautogui.hotkey('ctrl', 'c')
ret = pyperclip.paste()
pyautogui.alert(ret, "results")
print("\n\n*******************\n\n" + ret + "\n\n")
pyautogui.click(1900, 4)
```

Thank you for reading!