---
layout: post
title: "NPM Configuration"
date: 2017-10-30 14:41:11 -0600
comments: true,
categories: npm-config
---

# NPM Configuration

Most of my projects require the use of node and very often some packages from the NPM registry. A package.json file is mandatory for these projects filled with such metadata as name, version, author, etc. To initialize a node project you would usually go through the following steps:

1. `npm init`
2. Fill in the command prompts

You would then end up with a package.json file like this:

```json
{
  "name": "example",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```

Initially I wouldn't put too much forethought in how to fill it in, resulting in me just pressing **ENTER** and having the defaults filled in for me. However, even that became cumbersome, thus leading me to seek out a faster method: `npm init -y`. This approach is the same as above, but with the option of foregoing all the prompts and creating the json file with some preset values.



## DEM DEFAULTS THO

The defaults generated are alright I suppose, but what if we want to personalize them? There is a way - through the `.npmrc` file. There are many configurations available and for many environments such as global, user, and per project (include a `.npmrc` file in your root project directory), to name a few. What we'll focus on is the user defaults for the **package.json**.

## HOW TO FIND THE FILE

In your terminal: `npm config list -l | grep config`

You should be outputted with this:

```shell
; cli configs
; userconfig /Users/cerulean/.npmrc
; builtin config undefined
globalconfig = "/usr/local/etc/npmrc"
userconfig = "/Users/cerulean/.npmrc"
```

Keying in on the userconfig line, we can see that the config file we want is a dot file on the root user directory. Furthermore, upon opening it (or just `cat .npmrc`) , the file is just  `key: value` pairs. There are some values pertaining to the **package.json** file we can change easily:

> init.author.name
>
> init.author.email
>
> init.author.url
>
> init.author.license
>
> init.version



Now we can use a text editor to edit the file by changing the key values or we can use the terminal:

`npm config get <key>` to retrieve the current value of key

`npm config set <key> <value> [-g|--global]` to set the value for a key (with a global option)

EX:

`npm config get init.author.url` will output **https://github.com/cdrainxv** as it's the default I set.

`npm config set init.license MIT` will default your license field to **MIT** instead of the default **ISC**.



## IMPLEMENTATION #1

> npm config set init.version 0.0.1
>
> npm config set init.author.name cdrainxv
>
> npm config set init.author.email cdrainxv@users.noreply.github.com
>
> npm config set init.author.url https://github.com/cdrainxv
>
> npm config set init.author.url MIT

Resulting in this default **package.json** file when we `npm init -y`:

```json
{
  "name": "example",
  "version": "0.0.1",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "cdrainxv <cdrainxv@users.noreply.github.com> (https://github.com/cdrainxv)",
  "license": "MIT"
}
```



This is incrementally better. Note that we have setup the default version, author, and license fields to be particular to us, however we can do much better. Let's add defaults to things we almost certainly have to add and/or edit before we get any further in our projects.



## IMPLEMENTATION #2

Suppose we are a specific type of developer who specializes in creating specific types of projects using a specific framework which requires specific packages, being specifically lazy we don't want to start with the default json file generated for us, but we want to make it unique to us with the ability still to adjust the defaults. How can we do that? By way of an **.npm-init.js** file in our root directory we can write up our entire **package.json** file manually and/or include some prompts with defaults values which we can overwrite if we so choose.

You should be provided with this file, but if not then just create it in your root directory: `touch ~/.npm-init.js` and open it using your favorite text editor. As per per [npm](https://docs.npmjs.com/getting-started/using-a-package.json#customizing-the-init-process), you can customize your JS file using this approach:

```javascript
module.exports = {
  customField: 'Custom Field',
  otherCustomField: 'This field is really cool'
}
```

which outputs this **package.json**:

```json
{
  "customField": "Custom Field",
  "otherCustomField": "This field is really cool"
}
```



Questions can also be customized using **prompts**:

> module.exports = prompt("what's your favorite flavor of ice cream, buddy?", "I LIKE THEM ALL")



We will use a combination of both methods. Essentially we want the following fields:

+ name
+ version
+ main
+ description
+ scripts
+ keywords
+ dependencies
+ devDepencies
+ repository
+ author
+ license
+ bugs
+ homepage

Here is what I came up with for a sample template:

![npm-init]({{ "/assets/images/npm-init.png" | absolute_url }})

and here is the output file:

![npm-json]({{ "/assets/images/npm-json.png" | absolute_url }})


Here is a live demo:

![npm-init-demo]({{ "/assets/images/npm-init-demo.gif" | absolute_url }})

For further configurations or to extend your npm init experience reference [promzard](https://github.com/npm/promzard), [init-package-json](https://github.com/npm/init-package-json), and/or [npm](https://docs.npmjs.com/).

As always (well... this is only the second time), all examples files are provided on github. Here is this post's repo:  [npm-config-example](https://github.com/cdrainxv/npm-config-example).

