---
title: "Mastodon Bots series: Part 2: More complex interactions."
datePublished: Sun Apr 30 2023 11:37:51 GMT+0000 (Coordinated Universal Time)
cuid: clh3c7z04000009iegidh74xv
slug: mastodon-bots-series-part-2-more-complex-interactions
tags: python, mastodon, mastodon-bot, mastodonpy

---

Welcome back to part two of our Mastodon bot tutorial series! In the [first part](https://blog.tiagorangel.com/creating-a-mastodon-bot-with-python), we went through the basics of creating a Mastodon bot using Python and Mastodon.py. We created a simple bot that posted a new status update and replied to mentions in real time. Now, it's time to take things up a notch and explore more complex interactions.

> **Note:** If you haven't seen part 1, I hightly recommend [checking it out here](https://blog.tiagorangel.com/creating-a-mastodon-bot-with-python). You will need the starter setup (libraries etc.) to run any snippet

In this article, we'll dive deeper into the Mastodon API and explore some advanced features, including interacting with timelines, searching for content, and posting media. So grab your favorite beverage and let's get started!

## Interacting with timelines

A timeline in Mastodon is a stream of status updates from a particular user or a group of users. There are several types of timelines available in Mastodon, including the home timeline, which displays status updates from users that you follow, and the local timeline, which displays status updates from users on your Mastodon instance.

Let's say you want your Mastodon bot to retweet all the status updates from a particular user. To do this, we'll need to interact with the Mastodon API's timelines endpoint.

1. ### Boosting all the status updates from a particular user
    

```python
def retweet_user_timeline(username):
    timeline = mastodon.account_statuses(username)
    for status in timeline:
        mastodon.status_reblog(status.id)
```

In this code, we've defined a function called `retweet_user_timeline` that takes a username as its only argument. The function retrieves the status updates from the user's timeline using the `account_statuses` method and loops through each status update, using the `status_reblog` method to retweet the status update.

## Searching for content

The Mastodon API also allows you to search for content on Mastodon using keywords and hashtags. Let's say you want your Mastodon bot to search for all the status updates that contain the hashtag #python and like them. To do this, we'll need to interact with the Mastodon API's search endpoint.

### Like all the status updates that contain the hashtag #python

```python
def like_python_hashtag():
    results = mastodon.search('q=#python')
    for status in results['statuses']:
        mastodon.status_favourite(status.id)
```

In this code, we've defined a function called `like_python_hashtag` that searches for all the status updates that contain the hashtag `#python` using the search method and loops through each status update, using the `status_favourite` method to like the status update.

## Posting media

Finally, let's explore how to post media to Mastodon using Python and Mastodon.py. The Mastodon API allows you to attach images, videos, and other media types to your status updates.

### Post a new status update with an image

```python
def post_status_with_image(image_path):
    media = mastodon.media_post(image_path)
    mastodon.status_post('Check out this cool image!', media_ids=[media['id']])
```

Here, we've defined a function called `post_status_with_image` that takes an image path as its only argument. The function uses the `media_post` method to upload the image to Mastodon and obtains the media ID. Then, it uses the `status_post` method to post a new status update with the media ID attached to it.

### Polls

Another way to make your bot more interactive is to use user input to trigger certain actions. For example, you could create a poll bot that allows users to vote on various topics. To do this, you could use the Mastodon API's polling feature, which allows users to create polls with up to four options.

```python
def handle_mention(status):
    # Check if the status update contains a mention of our bot username
    if '@your_bot_username' in status.content:
        # Split the status update into individual words
        words = status.content.split()

        # Check if the first word is "poll"
        if words[1].lower() == 'poll':
            # Get the poll options from the remaining words in the status update
            poll_options = words[2:]

            # Create the poll
            poll = mastodon.polls_create(
                question=' '.join(poll_options),
                options=poll_options,
                expires_in='1w'
            )

            # Post a status update with the poll attached
            mastodon.status_post(
                '@{} Here\'s your poll!'.format(status.account.username),
                in_reply_to_id=status.id,
                poll=poll['id']
            )

# Start streaming for mentions
mastodon.stream_user(handle_mention)
```

This has more interactivity, as it looks for words and mentions. Run the code and your bot will be able to create polls in response to mentions! Just make sure to mention your bot's username followed by the word "poll" and the poll options, separated by spaces.

For example, if your bot's username is `my_bot` and you want to create a poll with the options "cats", "dogs", and "birds", you would send a mention to `@my_bot poll cats dogs birds`. Your bot will then create a poll with those options and reply to your mention with the poll attached.

---

And there you have it! With these advanced features, you can create more complex and interactive Mastodon bots using Python and Mastodon.py.

### Tips and tricks

Before we wrap up, let's go over some tips and tricks to help you create a successful Mastodon bot:

* Use descriptive status updates: When posting status updates, make sure they're descriptive and relevant to your bot's purpose. This will help users understand what your bot is doing and engage with it more effectively.
    
* Use emojis and memes: Mastodon users love emojis and memes, so don't be afraid to use them in your bot's posts. Just make sure they're appropriate and relevant to your bot's purpose.
    
* Engage with users: Interact with Mastodon users by responding to their posts and mentions of your bot. This will help build a community around your bot and encourage more people to use it.
    
* Be consistent: Make sure your bot is consistently posting updates and responding to users. This will help establish your bot's presence on Mastodon and keep users interested.
    
* Test your bot thoroughly: Before releasing your bot to the public, make sure to test it thoroughly to catch any bugs or issues. This will help ensure that your bot is reliable and effective.
    
* Stay within Mastodon's guidelines: Make sure to review Mastodon's guidelines and stay within them when creating your bot. This will help ensure that your bot is allowed on the platform and doesn't violate any rules.
    
    By following these tips and tricks, you can create a successful Mastodon bot that users will love to interact with.