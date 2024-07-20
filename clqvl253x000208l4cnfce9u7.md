---
title: "Cheating in Kahoot with AI"
datePublished: Tue Jan 02 2024 00:00:10 GMT+0000 (Coordinated Universal Time)
cuid: clqvl253x000208l4cnfce9u7
slug: cheating-in-kahoot-with-ai
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703941708949/bea8da11-ed3c-40f4-aacc-318c44836076.png
tags: ai, javascript, google, userscript, google-ai, cheating, gemini, kahoot

---

Kahoots are stressing. You're always afraid of losing and must be as fast as possible. But what if it wasn't like that? What if you could <s>cheat</s>, err..., **get help** from an AI like ChatGPT?

That's exactly what I made. (happy new year btw! if you just want the full code, scroll to the end)

<div data-node-type="callout">
<div data-node-type="callout-emoji">🙋</div>
<div data-node-type="callout-text">I was advised to warn you to please don't try this at school. This was just a fun experiment and I'm not actually using it to cheat.</div>
</div>

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">This bot only supports True/False and Quiz questions (with or without images)</div>
</div>

## Part 1: The plan

So here is the plan: I would build a userscript that would automatically detect when I was playing Kahoot, read the questions, ask Gemini Pro for the answers, and click the button.

Optionally, <s>if you didn't want your teacher to notice that you were cheating</s>, the script would change a bit the appearance of the button in a way that your teacher wouldn't notice but you would. For example, change the border-radius, or remove/add a button shadow

So, let's get coding.

## Part 2: The AI

I decided to use Google's new Gemini Pro AI for this, as it supported reading images and was smart. We need an API key for this, so follow the instructions:

1. Go to [https://makersuite.google.com/](https://makersuite.google.com/)
    
2. Tap "Get API key" at the left corner
    
3. Tap "Create new API key in new project"
    
4. Grab the API key
    
5. Profit!
    

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">If you're not able to access makersuite in your region, just turn on a VPN. I like ProtonVPN.</div>
</div>

Now, we have the API key and we can continue to the next step.

## Part 3: Coding

### 3.1: Detecting when a question is shown

Now for the difficult part: coding. I decided to code this in JS, as it was easy and fast.

I started by creating a function that would detect when a question is shown on the kahoot quiz:

```javascript
const main = function () {
    setInterval(function () {
        if (document.querySelector("button[class*='choice']") && !(document.querySelector("div[class*='extensive-question-title']").getAttribute("done") == "yes")) {
            getAnswer(); // TODO: Use AI to answer question
            document.querySelector("div[class*='extensive-question-title']").setAttribute("done", "yes")
        }
    }, 100)
}

main();
```

This will start an interval that will detect when a choice button is shown and start the whole process.

### 3.2: Getting the info from the question

Now, we need to extract all the information needed for the AI to process the question and generate an answer. After playing with Kahoot's HTML design, I came up with this:

```javascript
const getAnswer = async function () {
    var question, image = "";
    var options = [];

    if (document.querySelector("img[class*='question-base-image']")) {
        image = document.querySelector("img[class*='question-base-image']").src;
    }

    question = document.querySelector("span[class*='question-title__Title']").innerText.trim();

    document.querySelectorAll("button[class*='choice']").forEach(function (e) {
        var text = e.innerText;
        options.push(text)
    })

    return await getAI({ question, image, options }); // TODO: Ask AI for the answer and click the button
}
```

Nice! Everything works perfectly. Here, we grab the question, the image URL (if an image exists), and the available options, and return everything to a getAI function.

### 3.3: Getting the answer and clicking the button

This step is a bit more tricky. Because Google's Gemini Vision (the model I'm using for when an image is present, else I'm using normal Gemini Pro) requires the image to be in base64, I needed to convert the URL present in Kahoot's HTML and convert it to base64. I found some code in StackOverflow and it worked perfectly:

```javascript
const getAI = async function (data) {
    /* data: {
        question: string,
        image: url,
        options: array
    }
    */

    var url, body;

    function toDataUrl(url) {
        return new Promise(function (resolve) {
            var xhr = new XMLHttpRequest();
            xhr.onload = function () {
                var reader = new FileReader();
                reader.onloadend = function () {
                    resolve(reader.result);
                }
                reader.readAsDataURL(xhr.response);
            };
            xhr.open('GET', url);
            xhr.responseType = 'blob';
            xhr.send();
        })
    }

/* ... */
}
```

Now, let's ask Google's AI for the answer. We'll need to fill the `<insert token here>` fields with the actual token we got from step 3.1.

```javascript
    /* ... */

    if (data.image) {
        const b64 = (await toDataUrl(data.image)).replace("data:", "").split(";base64,");

        url = 'https://generativelanguage.googleapis.com/v1beta/models/gemini-pro-vision:generateContent?key=<insert token here>'; // [IMPORTANT] Add your token here

        body = JSON.stringify({ "contents": [{ "parts": [{ "text": `Answer the following question using the image context provided: ${data.question}. If there are multiple answers and there is no 'All of above/multiple' option, answer a random one. Reply ONLY with the answer that needs to be one of: ${data.options.join("; ")}.` }, { "inline_data": { "mime_type": b64[0], "data": b64[1] } }] }] });
    } else {
        url = 'https://generativelanguage.googleapis.com/v1beta/models/gemini-pro:generateContent?key=<insert token here>'; // [IMPORTANT] Add your token here

        body = JSON.stringify({ "contents": [{ "parts": [{ "text": `${data.question}? Reply only with your option. If there are multiple answers and there is no 'All of above/multiple' option, answer a random one.
Options: [${data.options.join(", ")}]` }] }] });
    }


    var response = (await (await fetch(url, {
        method: 'POST',
        headers: {'content-type': 'application/json'},
        body: body
      })).json());
      
    try {
        response = response.candidates[0].content.parts[0].text.trim().toLowerCase().replaceAll(".", "").replaceAll(",", "");
    } catch {
        return alert("An error occured. Make sure you're in a supported country. Response:\n\n" + response)
    }

    /* ... */
```

Now, we just need to click the correct button!

```javascript
    /* ... */

    document.querySelectorAll("button[class*='choice']").forEach(function (e) {
        var text = e.innerText.trim().toLowerCase().replaceAll(".", "").replaceAll(",", "");
        if (text == response) { e.click() }
    })

    return response;
}
```

Done! Now let's run everything together in a test Kahoot quiz and the bot will automatically answer all the questions.

### 3.4: Full code and some warnings

First, note that:

1. This code can sometimes be a bit glitchy.
    
2. This demo won't work in some countries in the EU. If you're in the EU, make sure to turn on a VPN to the US.
    
3. Don't use this too much or your teacher will notice
    

As promised, here's the full code:

```javascript
const getAI = async function (data) {
    var url, body;

    function toDataUrl(url) {
        return new Promise(function (resolve) {
            var xhr = new XMLHttpRequest();
            xhr.onload = function () {
                var reader = new FileReader();
                reader.onloadend = function () {
                    resolve(reader.result);
                }
                reader.readAsDataURL(xhr.response);
            };
            xhr.open('GET', url);
            xhr.responseType = 'blob';
            xhr.send();
        })
    }

    // [REQUIRED] Remember to change your API keys.
    if (data.image) {
        const b64 = (await toDataUrl(data.image)).replace("data:", "").split(";base64,");

        url = 'https://generativelanguage.googleapis.com/v1beta/models/gemini-pro-vision:generateContent?key=<token>';

        body = JSON.stringify({ "contents": [{ "parts": [{ "text": `Answer the following question using the image context provided: ${data.question}. If there are multiple answers and there is no 'All of above/multiple' option, answer a random one. Reply ONLY with the answer that needs to be one of: ${data.options.join("; ")}.` }, { "inline_data": { "mime_type": b64[0], "data": b64[1] } }] }] });
    } else {
        url = 'https://generativelanguage.googleapis.com/v1beta/models/gemini-pro:generateContent?key=<token>';

        body = JSON.stringify({ "contents": [{ "parts": [{ "text": `${data.question}? Reply only with your option. If there are multiple answers and there is no 'All of above/multiple' option, answer a random one.
Options: [${data.options.join(", ")}]` }] }] });
    }


    var response = (await (await fetch(url, {
        method: 'POST',
        headers: {'content-type': 'application/json'},
        body: body
      })).json());
      
    try {
        response = response.candidates[0].content.parts[0].text.trim().toLowerCase().replaceAll(".", "").replaceAll(",", "");
    } catch {
        return alert("An error occured. Make sure you're in a supported country. Response:\n\n" + response)
    }

    document.querySelectorAll("button[class*='choice']").forEach(function (e) {
        var text = e.innerText.trim().toLowerCase().replaceAll(".", "").replaceAll(",", "");
        if (text == response) { e.click() }
    })

    return response;
}

const getAnswer = async function () {
    var question, image = "";
    var options = [];

    if (document.querySelector("img[class*='question-base-image']")) {
        image = document.querySelector("img[class*='question-base-image']").src;
    }

    question = document.querySelector("span[class*='question-title__Title']").innerText.trim();

    document.querySelectorAll("button[class*='choice']").forEach(function (e) {
        var text = e.innerText;
        options.push(text)
    })

    return await getAI({ question, image, options })
}

const main = function () {
    alert("Cheat mode activated");

    setInterval(function () {
        if (document.querySelector("button[class*='choice']") && !(document.querySelector("div[class*='extensive-question-title']").getAttribute("done") == "yes")) {
            getAnswer();
            document.querySelector("div[class*='extensive-question-title']").setAttribute("done", "yes")
        }
    }, 100)
}

main();
```

<center><h2>Should this be banned from Kahoot?<br />Post your opinions in the comments.</h2><small>Thanks for reading! Remember to leave a like</small></center>