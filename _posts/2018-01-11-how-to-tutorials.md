---
layout: post
title: 'How To Tutorials'
date: 2018-01-11 12:49:03 -0600
categories: tutorials
---

Everyone loves tutorials, especially when they are very pertinent to a new
thing you want to learn. I find most times its quicker to read a simple
blog post on how to setup a new framework/library/package than it is to read
the official documentation.

However, sometimes when following along with even the most basic of
tutorials, you are bound to encounter some erroneous errors. These can
stem from conflicting package versions, changed/deprecated apis, etc. With
some trial and error and/or googling, there's a good chance you could fix the
problem without too much effort.

This scenario is perfectly alright in regards to an experienced
developer, but what about a new, budding one, whose every encounter with an
error message can be quite daunting and a turnoff. The question is then:
how do we remove these and similar problems for newcomers?

I've seen the four following ways:

1. Use exact versions when installing packages:

   > npm install react@16.0.0 --save

2. Include the entirety of your package.json files (or at least the
   dependencies) as either a codeblock or a gist for easy copy and pasting.


    ```json
    "dependencies": {
      "react": "16.0.0",
      "react-dom": "16.0.0"
    }
    ```

3. Add a `save-exact` config set for your npm settings to ensure
   that any packages you install uses the exact current version if you don't want to have to remember step 1. Simply turn turn the config on:

   > npm config set save-exact=true

4. Specify which version of node you are using. NodeJS has a rapid release
   rate and most likely a reader will be on a later version than when you
   initially wrote the tutorial. To make your post future-proof, state the
   version you are currently using and include links to node version
   managers, preferably your own post on how to setup one. Something simple, like my
   [own](https://cdrainxv.github.io/blog/node/2017/11/11/node-version-management.html), is will suffice.

Of course, there are many other small tips and tricks to make your posts
stand the test of time, but nothing beats the above suggestions and maybe,
once in a while, going down memory lane and doing incremental update.
