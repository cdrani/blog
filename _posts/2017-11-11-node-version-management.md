---
layout: post
title: Node Version Management
date:   2017-11-11 18:16:31 -0600
comments: true
categories: node
---

> NOTE TO EDITOR: Once again your output rate isn't as you promised. It seems you are reluctant to post anything unless it involves some kind of tutorial. Let's reiterate  -- the whole point of this blog is to record snapshots of your learning at as many intervals as possible not just significant ones. A post can be as simple as what you've learned, read, or done in the week related to software development. Posts don't need to be only about things you've fully digested -- things you don't quite understand or are in the process of understanding are welcome as well. See to it!



## NODE VERSION MANAGEMENT

The Node development pace is quite quick as you can see from their [changelogs](https://github.com/nodejs/node/blob/master/doc/changelogs/CHANGELOG_V8.md#8.9.1), therefore at any moment in time we might have different projects in development which we need to test on multiple versions of node or the codebase could rely on a specific node version. This and other scenarios necessitate the need for having a version manager to switch between multiple node versions.



There are two node version managers I know, [nvm](https://github.com/creationix/nvm) and [n](https://github.com/tj/n). For quite a while I've been using **nvm** as it was the only one I knew and I liked it enough, but I transitioned to **n** this week and I love it.

### INSTALLATION

There are many ways to install it, but I recommend the third-party install method [n-install](https://github.com/mklement0/n-install). I love this approach because it allows for easy updating and uninstallation of **n**. I recommend you to purge all instances of node and npm in your system prior to installing **n**. Follow this SO question: [Uninstall Node](https://stackoverflow.com/questions/11177954/how-do-i-completely-uninstall-node-js-and-reinstall-from-beginning-mac-os-x).



> ```bash
> curl -L https://git.io/n-install | bash
> ```



And that's it! The latest stable node version and npm will be setup for you. Confirm it like below:

> ```bash
> node -v && npm -v
> v9.0.0
> 5.5.1
> ```

NOTE: **node** and **npm** versions may vary at time you read this.

To update **n** (if there are any updates) just run `n-update`, and to uninstall node, npm, and n, `n-uninstall`.



### COMMON COMMANDS

- `n --> list versions installed`


- `n latest --> Install or activate latest node release `
- `n <version> --> install specific node version`
- `n prune --> delete all versions except the current version`
- `n rm <version ...> --> remove specific node version(s)`



Reference the links above for indepth installation steps and CLI commands for **n**.