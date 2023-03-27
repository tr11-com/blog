---
title: "Making a auth system in SECONDS with Authflow!"
datePublished: Mon Mar 27 2023 18:40:39 GMT+0000 (Coordinated Universal Time)
cuid: clfr6cpz101ilnanvejb84967
slug: making-a-auth-system-in-seconds-with-authflow
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/dk4en2rFOIE/upload/b5c7ee832589c761951fac2b5a483376.jpeg
tags: authentication, javascript, nodejs, auth, authflow

---

**Let's be honest, making secure authentication systems is HARD.** Really hard. You need to think of security, compatibility, ease-of-use, and much more. It can take days, if not weeks, to develop an authentication system that meets all these requirements. *But what if I told you there was a way to implement a secure and easy-to-use authentication system in just a few <s>minutes</s>* ***seconds****?*

> Using node? I have a project for you to fork! Scroll down a bit

Introducing Authflow – a simple and straightforward way to add authentication to your website. With Authflow, you can implement a login system in just a few steps, without worrying about the security and compatibility issues that often arise when developing your own system from scratch.

How does Authflow work? First, you redirect your users to the Authflow login flow. Then, Authflow loads the "Continue with Google" button on their website. The user logs in, and their information is sent back to an intermediary page on your website, where it is stored in cookies. Finally, the user is redirected to the "logged in" page. It's that simple.

To get started with Authflow, all you need to do is add the following script to your website:

```xml
<script src="https://jscdn.glitch.me/cdn/authflow.js" crossorigin="anonymous"></script>
```

And then, initiate the login process using the following code:

```javascript
authflow.login({
  app_name: "App name",
  redirect: "your_redirect_url",
});
```

That's it! With just a few lines of code, you can implement a secure and easy-to-use login system on your website.

But that's not all. Authflow also offers some additional features that you can use to further enhance your authentication system. For example, with the Authflow Button API, you can create a simple login button on your website. And with Authflow Instant, you can quickly and easily add a login with Google button to your page, providing a convenient way for users to log in.

**<mark>And this is really useful:</mark>** If you're using Node.js, you can use the "Zarran API" to create your own authentication and authorization server. It has many features built-in, plus there's also a password-username login if your users don't have a Google account. <s>(btw, who doesn't?)</s>

Get started now at [https://authflow.glitch.me/](https://authflow.glitch.me/)!