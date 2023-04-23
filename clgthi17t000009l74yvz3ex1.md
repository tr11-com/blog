---
title: "Creating a Mastodon Bot with Python"
datePublished: Sun Apr 23 2023 14:07:57 GMT+0000 (Coordinated Universal Time)
cuid: clgthi17t000009l74yvz3ex1
slug: creating-a-mastodon-bot-with-python
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/Dj1EPDH5_h4/upload/46e7c36c9bb46a9992f692b7bc224ded.jpeg
tags: python, mastodon, mastodon-bot

---

Mastodon is a decentralized social network that allows users to create their own instances and communicate with users on other instances. With a Mastodon bot, you can automate tasks such as posting updates, replying to mentions, or even creating interactive chatbots. In this tutorial, we'll be using Python and the Mastodon.py library to create a Mastodon bot.

> I no longer recommend you making a Twitter bot, as now you need to pay to get access to their API.

Before we get started, let's go over a few prerequisites. You'll need to have Python installed on your system, as well as the [Mastodon.py](https://github.com/halcy/Mastodon.py) library. You can install Mastodon.py using pip:

```plaintext
pip install Mastodon.py
```

You'll also need to have a Mastodon account and access to an instance. If you don't have an account yet, you can create one on any instance that allows new registrations. For bot accounts, I highly recommend [https://botsin.space/](https://botsin.space/), managed by BotWiki.

Once you have everything set up, let's dive into the code.

First, we'll need to import the Mastodon.py library and create an instance of the Mastodon class. We'll also need to authenticate with the Mastodon API using our access token, which can be obtained by going to the "Settings" page on your Mastodon instance and clicking "Development".

```python
from mastodon import Mastodon

# Create an instance of the Mastodon class
mastodon = Mastodon(
    access_token='your_access_token_here',
    api_base_url='https://your.instance.url'
)
```

With our Mastodon instance set up, we can now start interacting with the Mastodon API. Let's start by posting a new status update.

```python
# Post a new status update
mastodon.status_post('Hello, Mastodon!')
```

That's it! You've just posted a new status update to Mastodon. Let's take a closer look at how this works.

The `status_post` method takes a single argument, which is the text of the status update. When you call this method, Mastodon.py sends a POST request to the Mastodon API's `/api/v1/statuses` endpoint with the status update data. The Mastodon API then creates a new status update on your behalf and returns the updated status data, which Mastodon.py uses to generate a Status object.

Now let's say you want your Mastodon bot to reply to mentions. To do this, we'll need to use the Mastodon API's streaming API, which allows us to receive real-time updates for various events, including mentions.

```python
# Define a function to handle mentions
def handle_mention(status):
    if '@your_bot_username' in status.content:
        mastodon.status_post('@' + status.account.username + ' Hello there!')

# Start streaming for mentions
mastodon.stream_user(handle_mention)
```

In this code, we've defined a function called `handle_mention` that takes a Status object as its only argument. We check if the status update contains a mention of our bot username, and if so, we use the `status_post` method to reply to the user who mentioned us.

Finally, we start streaming for mentions using the `stream_user` method. This method takes a callback function as its only argument, which is called every time the Mastodon API sends us a new event.

That's it! You've just created a Mastodon bot using Python and Mastodon.py. With a little bit of creativity, you can use this bot to automate tasks and create interactive experiences on Mastodon. Here's the final code:

```python
from mastodon import Mastodon

# Create an instance of the Mastodon class
mastodon = Mastodon(
    access_token='your_access_token_here',
    api_base_url='https://your.instance.url'
)

# Post a new status update
mastodon.status_post('Hello, Mastodon!')

# Define a function to handle mentions
def handle_mention(status):
    if '@your_bot_username' in status.content:
        mastodon.status_post('@' + status.account.username + ' Hello there!')

# Start streaming for mentions
mastodon.stream_user(handle_mention)
```

In the next blog post, we'll make a bot with much more powerful capabilities, and test one in the wild!

Before we wrap up, let's take a look at a few tips and tricks for creating a successful Mastodon bot:

* Respect users' privacy: Mastodon is built on a foundation of privacy and security. Make sure your bot respects users' privacy by only accessing data that's necessary for its functionality.
    
* Stay within the API limits: Like any other API, the Mastodon API has limits on how often you can make requests. Make sure your bot stays within these limits to avoid being rate-limited or banned.
    
* Be responsive: If your bot is designed to respond to users, make sure it does so in a timely manner. Users on Mastodon expect a fast and responsive experience, so make sure your bot is up to the task.
    
* Have fun!: Mastodon is a community-driven network that values creativity, humor, and fun. Make sure your bot reflects this by injecting some personality and humor into its interactions.
    

Creating a Mastodon bot with Python is a fun and rewarding experience that allows you to automate tasks, provide unique value to users, and engage with the Mastodon community. With [Mastodon.py](http://Mastodon.py) and a little bit of creativity, the possibilities are endless. So go ahead, create your own Mastodon bot and see where it takes you!

Happy bot-making! 🤖