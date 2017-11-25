---
layout: post
title: 'Weekly Update: Dev Setup'
date: 2017-11-25 15:42:51 -0600
categories: weekly-update
---

# WEEKLY UPDATE: DEV SETUP

> NOTE TO EDITOR:
>
> There will be some weeks where it seems I have not made any progress - most likely because there are not multiple posts within the week. To assuage your fears and uphold our contract, I have decided to implement weekly update posts. These will serve to summarize what I learned and/or what has been my focus. Most likely these small detours, as they might not all be focused in progressing my software career, will result in a small project  or a writeup that showcases my understanding of the subject(s) I pursued during the week.



## FOCUS

My focus this week was on updating/configuring some of my development environment files and pursuing my curiosity in how chrome extensions work and also how to create my own. Fortunately both endeavors have not been too difficult and hopefully I can spin up a tutorial or two on creating such extensions in the upcoming weeks. 



## CHANGELOG

* I looked into version controlling some of the dotfiles imperative to my development environment, such as my **.zshrc** and **.vimrc**, among others. There was a plethora of [strategies](https://dotfiles.github.io/), but alas they seemed too complex for my current situation. My immediate solution is save them to a version controlled **dotfiles** folder in my root directory and then symlink it to the root user files using a shell script in the cases I am not on my home system. This worked as intended, however I removed that **dotfiles** folder as it was made obsolete with my switch from **oh-my-zsh** to **zprezto** framework for my zsh shell. Instead I have put my zprezto configuration under source control, but I do still have my previous dotfiles saved under **.backups** just in case.



* I have recommited to using **vim** as my default editor/ide. Previously I would revert back to editors like **atom** and **sublime-text** if something in vim annoyed me and/or I didn't know the immediate command keys for certain actions and had to look them up. At this point, I believe I have become proficient at vim (albeit still at the beginner stage). I have a **.vimrc** configured just how I like it and I have even included some plugins to flesh out the vim. The plugins aren't a crutch, but I eventually want to remove some of them as my vim knowledge increases and I can recreate those same commands when I learn vimscript (eventually).



* I have previously read up on [**tmux**](https://leanpub.com/the-tao-of-tmux/read) and tried it out in a one-toe-in-water manner, but much like my renewed commitment to vim, I dove fully in. I have setup up a **.tmux.conf** with some configs that mesh well with my **.vimrc**, and additionally I setup some default sessions and windows that I now use frequently.



* Switched ruby version managers from **rvm** to **rbenv**. Self-explanatory -- at least to me. :smile



## WEEKLY GOALS

* I know **react**. However, that's not a truly revealing statement as I still have yet to truly learn testing react and/or redux components - in fact redux is still not second-nature to me as of yet. This week - and probably the next as well - I will solidify my understanding of the above and other more advanced concepts relating to the library and try to incorporate them into the react projects strewn in my root directories. Hopefully I am successful and can not only post the apps on github, but also have live versions running on **surge** or **heroku**.



* I feel like I need to learn how to automate something things on my map. I know I can make use of scripts, but I want those scripts to run on their own. Previously on my ubuntu system I used cron jobs, however they are deprecated in mac, therefore I want to look into [**launchd**](http://www.launchd.info/) to run simple scripts timely such as updating **brew** and it's packages. 



### GOOD LUCK!
