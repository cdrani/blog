---
layout: post
title: Creating A CLI App
date:   2017-11-18 16:16:31 -0600
comments: true
categories: cli
---

# NODE CLI

I use the terminal a lot (specifically iTerm with a zsh shell). Therefore I would consider myself quite adept at using the command line interface (CLI) - at the least the most known ones and a few that require reading the manpages. However, suppose I wanted to create my own, then what? Well, there are a few scripting languages I could use - and a few of them are already preinstalled on my computer - such as Python, Ruby, etc. Nevertheless, I am most comfortable with JavaScript and lo and behold we can write out own CLI programs using NODE.

## IDEA

I love reading [xkcd](https://www.xkcd.com) comics online, but sometimes it's too arduous a task to open a browser to view a single comic. What if we could view it on our terminal instead? For the sake of simplicity, all we want is to get the title, the image (the comic panels), and the alternate text provided for each comic. Additionally, we want to limit our options to get either only the most current comic or a specific comic based on the comic number.



## IMPLEMENTATION: SETUP

Since this is a NODE app, we will need to configure it as such (using npm or yarn):

1. mkdir xkcd && cd xkcd
2. npm init 
3. touch xkcd-cli.js



Now we will need to add some packages from npm:

`npm i commander imgcat chalk node-fetch -S`

* [commander](https://www.npmjs.com/package/commander) - To create the CLI
* [imgcat](https://www.npmjs.com/package/imgcat) - To display the comic image on the terminal
* [chalk](https://www.npmjs.com/package/) - To log coloured text to terminal
* [node-fetch](https://www.npmjs.com/package/node-fetch) - To fetch the comic using fetch



Configure your **package.json** to resemble something like this:

```json
{
  "name": "xkcd-cli",
  "version": "0.1.0",
  "description": "xkcd comic viewer on terminal",
  "bin": {
    "xk": "xkcd.js"
  },
  "keywords": [
    "xkcd",
    "cli",
    "node"
  ],
  "dependencies": {
    "chalk": "^2.3.0",
    "commander": "^2.11.0",
    "imgcat": "^2.3.0",
    "node-fetch": "^1.7.3"
  },
  "repository": "git+https://github.com/cdrainxv/xkcd-cli.git",
  "author": "cdrainxv <cdrainxv@users.noreply.github.com> (https://github.com/cdrainxv)",
  "license": "MIT"
}
```



NOTE: The [**bin**](https://docs.npmjs.com/files/package.json#bin) field. We will use **xk** as our command name. EX: `xk -c` 



## IMPLEMENTATION: xkcd-cli.js

The very first line is required to inform that this file is an executable, i.e a script file.



1.The very first line is required to inform that this file is an executable, i.e a script file.

​	`!/usr/bin/env node`



2. Require all the dependencies:

   ```javascript
   const program = require('commander')
   const fetch = require('node-fetch')
   const chalk = require('chalk')
   const imgcat = require('imgcat')
   ```

   ​

3. Setup the options/arguments for the CLI:

   ```javascript
   program
     .version('0.1.0')
     .option('-c, --current', 'most current comic')
     .option('-s, --specific <value>', 'specific comic')
     .parse(process.argv)
   ```

   ​

   ***Current*** : **-c** is the shorthand options, **--current** is the longhand, and **most current comic** is the descriptive text for the option.

   ***Specific***: **<value>** means an argument is required. EX: `xk -s 123`

   (Reference [commander](https://www.npmjs.com/package/commander) for further options and explanations)

   ​

4. Function to fetch the comics:

   ```javascript
   async function fetchComic(url) {
     const response = await fetch(url)
     const data = await response.json()
     const { alt, img, title } = data
       console.log(chalk.blue(title))
       imgcat(img, { log: true })
       console.log(chalk.green(alt))
   }
   ```

   ​

5. Fetch comic based on the option and/or arguments supplied:

   ```javascript
   switch(process.argv[2]) {
     case '-c': {
       return fetchComic(`https://xkcd.com/info.0.json`)
     }

     case '-s': {
       return fetchComic(`https://xkcd.com/${program.specific}/info.0.json`)
     }

     default: {
       console.log('For available commands/options, reference `xk -h`')
     }
   }
   ```



## IMPLEMENTATION: TEST

To test the CLI locally, run **npm link** and try it out anywhere in your terminal with the options provided:

### current: 

`xk -c` or `xk --current`

![current]({{ "/assets/images/xkcd_current.png" | absolute_url }})

### specific: 

`xk -s 199` or `xk --specific 145`

![specific]({{ "/assets/images/xkcd_specific.png" | absolute_url_}})

### help: 

`xk -h` or `xk --help`

![help]({{ "/assets/images/xkcd_help.png" | absolute_url_}})



##UPCOMING FEATURES

Everything seems to be in order now and we are essentially finished based on the constraints I setup at the beginning. However I hope to add the following functionality soon:

- next option to get the next comic
- previous option to get the previous comic
- random option to get a random comic
- add some tests
- publish on npm



As always the project can be found on github here: [xkcd-cli](https://github.com/cdrainxv/xkcd-cli).