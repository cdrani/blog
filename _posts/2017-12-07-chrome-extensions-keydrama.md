---
layout: post
title: 'Chrome Extensions: KDRAMA'
date: 2017-12-07 22:35:19 -0600
categories: chrome-extensions
---

# INTRO

I **_love_** Korean Dramas the same way some people enjoy telenovellas or soap
operas. I don't know the exact specifics for this love, but it might be the fact
that they have limited 16 episode runs, much like a miniseries, with no second
seasons (or very rarely). Another facet of kdramas that I love is how funny it
is (in my perspective), regardless if it's portrayed as a comedy or drama.

Anyways, I came upon this site,
[https://goplay.anontpp.com/](https://goplay.anontpp.com/), while traversing
through reddit. I love the site, but the one knock against it from me is the
fact that it makes use of access tokens that expires weekly. I hate this screen:
![kdrama_access_denied]({{ "/assets/images/kdrama_access_denied.png" |
absolute_url }})

When the token expires, I then have to go to the reddit page, click a link to
redirects me to a page with a button to generate a new token key, and then
**_finally_** be granted access to the full site. I understand the reasoning
behind this, but I immediately thought there must be a way to circumvent these
steps that I have to take weekly. What if I could go to the site and, upon
discovering that my access token has expired and my access is denied, just press
a **"button"** which would just execute the above steps and land me on the site,
with a new access token and full site accessibility? Well, there is a way:
chrome extensions.

# CHROME EXTENSIONS

[Chrome extensions](https://developer.chrome.com/extensions) are "small software
programs that enhance the functionality of the Chrome browser". You probably
have a few of your own decorated on your omnibar and toolbar:

![kdrama_exts]({{ "/assets/images/kdrama_exts.png" | absolute_url }})

These extensions are written just using html, css, and javascript. In our case,
we will exclusively use javascript because we don't require any ui interface at
all.

> NOTE: To follow along and/or test your own during development, visit
> [chrome://extensions](chrome://extensions) and verify that you have "Developer
> mode" checkbox ticked. You can load your extension to chrome by on "Load
> unpacked extension" and selecting the path to your extension. As we will be
> using a background script, you can view any errors and/or logs by clicking on
> "background page".

## MANIFEST.JSON

Any and all chrome extensions require a
[manifest.json](https://developer.chrome.com/extensions/manifest) file. This is
a metadata file, much like a package.json, Gemfile, etc, that contains pertinent
information about the extension.

### REQUIREMENTS

The most pertinent fields are the "manifest\*version", "name", and "version".
Let's start off with those. The name and version are up to you (although must be
string), but the manifest_version \**_MUST**\* be **2\**.

> mkdir kdrama-ext && cd kdrama-ext touch manifest.json

**./manifest.json**

```json
{
  "manifest_version": 2,
  "name": "KDRAMA",
  "version": "0.1.0"
}
```

### RECOMMENDED

Google also recommends including metadata about the "default_locale",
"description", and "icons". These fields are for supporting multiple languages,
telling the intent of the extension, and the icons that will displayed on the
toolbar, chrome webstore, and chrome extensions settings page, respectively. You
can provide any size icon to be used in Chrome, and Chrome will select the
closest one and scale it to the appropriate size to fill the 16-dip space.
However, if the exact size isn't provided, this scaling can cause the icon to
lose detail or look fuzzy. Add these fields in as well:

```json
{
  "manifest_version": 2,
  "name": "Keyrama",
  "version": "0.1.0",
  "author": "cdrainxv",
  "description": "Bypass token generation for https://goplay.anontpp.com",
  "icons": {
    "16": "images/corvo_16.png",
    "32": "images/corvo_32.png",
    "48": "images/corvo_48.png",
    "128": "images/corvo_128.png"
  }
}
```

### OPTIONALS

Here is where we get to the heart of the functionality we want to implement.

**BROWSER OR PAGE ACTIONS**

Chrome gives us the option of using
[browser_actions](https://developer.chrome.com/extensions/browserAction),
[page_actions](https://developer.chrome.com/extensions/browserAction), or none
at all. I decided to go with a **browser_action** because I do want to implement
badges (later on) to notify users that their token has expired. Read the tips at
the bottom of either page to differentiate between their recommended use cases.

**CONTENT SCRIPTS**

[Content scripts](https://developer.chrome.com/extensions/content_scripts) are
javascript files that run in the context of the webpage. They are a necessity
for this extension as we will need to access the DOM **twice** to redirect tab
from page to page via clicking on a link and a button. A key component of
content scripts is that you can specify on which urls they can be injected into
by adding a "matches" key with array values.

**BACKGROUND**

The [background](https://developer.chrome.com/extensions/background_pages) field
can contain a long-running script to manage some task or state. Since the
majority of our extension runs in the background, except for the two instances
where we touch the DOM, the background script is integral for all the other
tasks, such as updating the tab page, executing the content scripts, and adding
notifications like badges, just to list a few.

**PERMISSIONS**

When we download apps on our smart devices (phones/tablets), we are often asked
if we consent to the application accessing our mail, contact list, photos, etc.
Likewise, chrome makes it a requirement that users are notified what the
extension has access to before they grant the extension "permission" to run on
their browser. In our extension we need access to the same urls that the content
scripts will run in. Additionally, we need permission to access the current tab
that the user has open.

Now that all of those options have been overviewed, it's time to add them to our
manifest.json. This will be the state of our file after all the above have been
included:

```json
{
  "manifest_version": 2,
  "name": "KDRAMA",
  "version": "0.1.0",
  "author": "cdrainxv",
  "default_locale": "en",
  "description": "Bypass token generation for https://goplay.anontpp.com",
  "icons": {
    "16": "images/corvo_16.png",
    "32": "images/corvo_32.png",
    "48": "images/corvo_48.png",
    "128": "images/corvo_128.png"
  }
  "browser_action": {
      "default_title": "KDRAMA",
      "default_icon": "images/corvo_32.png"
  },
  "content_scripts": [
    {
      "matches": [
        "https://goplay.anontpp.com/",
        "https://www.reddit.com/r/KDRAMA/*"
      ],
      "js": ["js/link.js", "js/gen_token.js"],
      "all_frames": false
    }
  ],
  "background": {
    "scripts": ["js/background.js"],
    "persistent": false
  },
  "permissions": [
    "https://goplay.anontpp.com/",
    "https://www.reddit.com/r/KDRAMA/*",
    "tabs"
  ]
}
```

Writing the manifest.json file was the hardest part for me, mainly because I had
to research what permissions the extension would need, how I could access the
DOM (initially thinking I would need to send messages back and forth between the
background and content scripts), and how poorly written the documentation is (at
least in comparison to the ones at mdn). Regardless of those initial setbacks, I
persisted through countless stackoverflow questions and docs before I got a
working, albeit rough extension.

## BACKGROUND.JS

The background script is the first then run when our application starts. We will
set out extension to only "activate" when our icon is clicked in the toolbar.
Chrome has a specific api for this:
[onClicked](https://developer.chrome.com/extensions/browserAction#event-onClicked).
This event action gives us access to the current tabs, but particularly to the
tab with the url we have permission to access.

**./js/background.js**

```js
var url =
  'https://www.reddit.com/r/KDRAMA/comments/5o2lld/i_created_this_website_that_streams_kdramas_in_hd/?st=jan66p4y&sh=3c953db8'

chrome.browserAction.onClicked.addListener(function(tab) {
  //
})
```

Within this event function we will run all our tasks, with the first one being
to "[query](https://developer.chrome.com/extensions/tab#method-query)" all the
current tabs in the browser and filtering the "active" ones and the ones in the
"curentWindow".

```js
var url =
  'https://www.reddit.com/r/KDRAMA/comments/5o2lld/i_created_this_website_that_streams_kdramas_in_hd/?st=jan66p4y&sh=3c953db8'

chrome.browserAction.onClicked.addListener(function(tab) {
  chrome.tabs.query({ currentWindow: true, active: true }, function(tab) {
    //
  })
})
```

Next, we want to
"[update](https://developer.chrome.com/extensions/tab#method-update)" our
current tab url to the reddit page. In the kdrama reddit page there is a link
which will take us to the page where we can then generate a new token. The
"update" method takes in the **id** of the tab we wish to update, an object
where we can set a new url for the tab, and an optional callback. After the
earlier filtering of the tabs in the "query" method, the filtered tabs are in an
array, with the one we seek at position 0, as only one matches the urls to which
we have access. The [tab](https://developer.chrome.com/extensions/tabs#type-Tab)
parameter is an object provided by chrome, which gives certain information about
the current tabs in the window.

```js
var url =
  'https://www.reddit.com/r/KDRAMA/comments/5o2lld/i_created_this_website_that_streams_kdramas_in_hd/?st=jan66p4y&sh=3c953db8'

chrome.browserAction.onClicked.addListener(function(tab) {
  chrome.tabs.query({ currentWindow: true, active: true }, function(tab) {
    updateTab(tab)
  })
})

function updateTab(tab) {
  chrome.tabs.update(tab[0].id, { url: url, active: true }, function(tab) {
    //
  })
}
```

Okay, our tab is in the process of being updated, but to ensure that it has
"complete[ed]" fully instead of still being in the "loading" phase, we want to
add an event listener,
[onUpdated](https://developer.chrome.com/extensions/tabs#event-onUpdated), that
can listen in on if the tab has been truly updated to the new url.

```js
var url =
  'https://www.reddit.com/r/KDRAMA/comments/5o2lld/i_created_this_website_that_streams_kdramas_in_hd/?st=jan66p4y&sh=3c953db8'

chrome.browserAction.onClicked.addListener(function(tab) {
  chrome.tabs.query({ currentWindow: true, active: true }, function(tab) {
    updateTab(tab)
  })
})

function updateTab(tab) {
  chrome.tabs.update(tab[0].id, { url: url, active: true }, function(tab) {
    onTabUpdated(tab.id, { status: tab.status }, tab)
  })
}

function on onTabUpdated(tabId, changeInfo, tab) {
  chrome.tabs.onUpdated.addListener(function(tabId, changeInfo, tab) {
      //
  })
}
```

We don't want to perform any actions until the reddit page status is "complete",
therefore we can have the listener wait specifically for that event using the
"status" property found on on all tab objects. When the status is "complete", we
want to execute the **link.js** script.

```js
var url =
  'https://www.reddit.com/r/KDRAMA/comments/5o2lld/i_created_this_website_that_streams_kdramas_in_hd/?st=jan66p4y&sh=3c953db8'

chrome.browserAction.onClicked.addListener(function(tab) {
  chrome.tabs.query({ currentWindow: true, active: true }, function(tab) {
    updateTab(tab)
  })
})

function updateTab(tab) {
  chrome.tabs.update(tab[0].id, { url: url, active: true }, function(tab) {
    onTabUpdated(tab.id, { status: tab.status }, tab)
  })
}

function on onTabUpdated(tabId, changeInfo, tab) {
  chrome.tabs.onUpdated.addListener(function(tabId, changeInfo, tab) {
     if (changeInfo.status === 'complete') {
         chrome.tabs.executeScript(null, { file: 'js/link.js' })
     }
  })
}
```

In the alluded **link.js** script, the context will be the reddit page. On that
page there's a specific link we want to simulate a click on. Upon the "clicking"
on it, the link will redirect us to the "Generate Token" page.
![kdrama_gen_link]({{ "/assets/images/kdrama_gen_link" | absolute_url }})

**js/link.js**

```js
link = document.querySelector('a[href="https://kdrama.anontpp.com/"]')
link.click()
```

Now back to the **background.js** script. At this point we are at the "Generate
Token" page. However, to be certain of that, we only want to execute the
**gen_token.js** script if and _only_ if the title of the tab matches the title
of the document.

```js
var url =
  'https://www.reddit.com/r/KDRAMA/comments/5o2lld/i_created_this_website_that_streams_kdramas_in_hd/?st=jan66p4y&sh=3c953db8'

chrome.browserAction.onClicked.addListener(function(tab) {
  chrome.tabs.query({ currentWindow: true, active: true }, function(tab) {
    updateTab(tab)
  })
})

function updateTab(tab) {
  chrome.tabs.update(tab[0].id, { url: url, active: true }, function(tab) {
    onTabUpdated(tab.id, { status: tab.status }, tab)
  })
}

function onTabUpdated(tabId, changeInfo, tab) {
  chrome.tabs.onUpdated.addListener(function(tabId, changeInfo, tab) {
    if (changeInfo.status === 'complete') {
      chrome.tabs.executeScript(null, { file: 'js/link.js' })
    }

    if (tab.title === 'Token Generation - Korean Shows HD Streaming') {
      chrome.tabs.executeScript(null, { file: 'js/gen_token.js' })
    }
  })
}
```

On the "Generate Token" page, there's a prominent button that we can simulate a
"click" event on. Clicking on the button will submit a form that will both
generate a new token and refresh the page, thus granting us full site access.

![kdrama_gen_btn]({{ "/assets/images/kdrama_gen_btn.png" | absolute_url }})

```js
if (document.title === 'Token Generation - Korean Shows HD Streaming') {
  var genButton = document.querySelector('button')
  genButton.click()
}
```

## CONCLUSION

That's the end of the extension for now. There's some areas for improvement
however, including this bug where you to click the extension button **_twice_**
before a new token is generated. I will ultimately fix this issue and some
others that I have highlighted on the
[KDRAMA](https://github.com/cdrainxv/KDRAMA/issues) issues page.

---

I have yet to place it on the chrome webstore, but as soon as I fix the niggling
issues I would love to publish it. Moreover, I want to port it to other browsers
(at least safari and mozilla) just to get the experience in making it universal.
