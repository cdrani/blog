---
layout: post
title: "Favicons"
comments: true
category: favicons
---

# FAVICONS

## NOTE TO THE EDITOR

> What kind of record-keeping is this if it has already been a week since your last post? I don't expect a post everyday, but from now on a minimum of 2 is required per week (circumstances withstanding). Keep in mind this is just a log of things you have learned and/or want to share along your software journey - it does not have to be revelatory (to anyone besides yourself). ___Anything___ you find interesting or just want to add is __your__ prerogative.

## OK... SO FAVICONS... HUH

Something I noticed a while ago, but didn't have a chance to explore untiil recently are favicons. What are they and what purpose do they serve?

## REGALE US WITH YOUR FINDINGS
favicons (aka tab icon, and others) are files with one or more small icons. You may have seen them on the left side of your tab icons, in your bookmarks, or even used as the app icon on a mobile device using the Web Clip feature (aka ___Add to Home Screen___):

![github]({{ "/assets/images/github-favicon.png" | absolute_url }})

![bookmark]({{ "/assets/images/bookmark-favicon.png" | absolute_url }})

Now I could talk about what I found in my indepth research of this topic, but the real reason for this writeup is a selfish one, namely that I did not have one prior to this post:

![cdrainxv]({{ "/assets/images/cdrainxv-favicon.png" | absolute_url }})

In my search to find out how to add my own favicons to this site, I came across some of the following tidbits:

* Introduced in 1999 with IE 5
* Originally used to estimate no. visitors by requests of favicon
* W3C standardized in Decemeber in HTML 4.01 recommendation
* favicon can support multiple file formats (ico, png, gif, jpg, etc)
* icon files should be square (16x16, 128x128, etc)
* most browsers prefer the favicon file to be in the website's root

### Usage:

A link element with a rel attribute of "icon" in the head section.
Example: `<link rel="icon" type="image/[ext]" href="/favicon.[ext]">`
[ext] should be replaced with appropriate file formats in the above list
NOTE: __etc__ is not a file extension for favicons. :smirk:

As you can see, they are not interesting enough to retain this info in the recesses of your mind, although how to include them in your own sites is noteworthy. So lets's do that.

# Implementation
However, nary a blog post or the jekyll documentation explicitly mentioned how to include one inside the site. Nevertheless, I tried many implementations (with a lot of failures) before coming upon the current solution.

First, either generate favicon files using [favicon generator](https://realfavicongenerator.net) or create your own. Next include the favicon file or folder in the root directory. The file will then be added to the `_site ` directory when you run `jekyll serve` .

The third step (assuming that you are in your site directory) is a multi-part operation where we override the minima theme using the terminal:

1. `mkdir _includes`
2. `cd $(bundle show minima)`
3. `cp ./_includes/head.html ~/blog/_includes/`
4. copy and paste the lines of code **favicon generator** provided inside the `/blog/_includes/head.html` file or just add a link element referencing the favicon file there.

NOTE: The root directory assumed is `blog`  and `minima` is the theme I am currently using - adjust to your personal settings.

And that's about it! In the process I got a little better idea of how to modify jekyll and will try to consistently personally update the site from the base minima theme. In fact I hope to create my own theme and add additional pages apart from the current ones.



In the upcoming posts I want to build the following:

- In my extensive use of git there are many times where I have to search how to do a certain command which are not second nature to myself. I want to list them and add them as aliases  to my `.zshrc` file. Additionally I should put some of my dot files into version control.
- simple rest api with a verbose writeup just to evaluate the extent of how much I understand rest principles and the express framework
- slightly more complex rest api with views using the express-generator and a database
- update the more complex api to use graphql instead of rest and maybe introduce a front-end framework like React or Vue ---> MERN or MEVN

