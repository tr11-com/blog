---
title: "Connecting my website with Google Sheets"
datePublished: Sat Sep 24 2022 09:05:55 GMT+0000 (Coordinated Universal Time)
cuid: cl8fosvwt0389rhnv8ekp0s2v
slug: connecting-my-website-with-google-sheets
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1675256443693/ee95532e-23bf-4f6e-a36c-e0b9b6c74118.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1675256280661/48b82c50-4d65-40bb-9b91-825775d10460.png
tags: javascript, google-sheets, fetch

---

When I used my old website, there was something I remember I didn't like about it:

> Why do I have to edit the HTML code every time I add a project?

> Why do I have to copy and paste a lot of code every time I want to add a social media link to my website?

When making my new website, I was going to make the same error, but then I thought:

> What about connecting mojects with Google Sheets?

Easy, right? Well,difficult part was getting a service that actually worked. Some were paid, others had a limit.  
So I decided to go with [opensheet](https://github.com/benborgers/opensheet). It's quite easy to use, only some fetch requests.

```plaintext
/* ⭐ Project sheet handling ⭐ */
fetch(
  "https://opensheet.elk.sh/1pSsxwJLuQ1oSTp2eqj34u8w88aWCF0h1jMskfjY58VY/Projects"
)
  .then((res) => res.json())
  .then((data) => {
    document.getElementById("projects").innerHTML = "";
    data.forEach((row) => {
      addProject(
        row.project,
        row.url,
        row.tags,
        row.description
      );
    });
  });

function addProject(name, url, tags, description, json) {
  // add project to DOM here
}
```

Easy, right? Then I did the same thing for my social media links:

```plaintext
/* ⭐ Social Media Links handling ⭐ */
fetch(
  "https://opensheet.elk.sh/1pSsxwJLuQ1oSTp2eqj34u8w88aWCF0h1jMskfjY58VY/Social"
)
  .then((res) => res.json())
  .then((data) => {
    data.forEach((social) => {
      addSocial(social.name, social.url, social.image);
    });
    document.querySelector("section[name='social']").innerHTML = socialHTML;
  });

function addSocial(name, url, image) {
  socialHTML = `${socialHTML} <a title="${name} profile" aria-hidden="true" href="${url}" target="_blank"><img alt="${name} logo" aria-hidden="true" src="${image}" draggable="false"></a> `;
}
```

Add a very simple loader effect, and it's done! It is quite useful and a huge time-saver, but we always have problems with loading speed.