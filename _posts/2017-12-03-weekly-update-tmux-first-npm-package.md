---
layout: post
title: 'Weekly Update: Tmux & First NPM Package'
date: 2017-12-03 22:33:48 -0600
categories: weekly
---

# WEEKLY UPDATE: TMUX & FIRST NPM PACKAGE

---

## FOCUS

I created my first npm package. It taught me how write, package, and publish
projects to npm. The joy of it was almost equal to the joy I got from pushing
getting my first PR merged.
[eslint-config-cdrainxv](https://github.com/cdrainxv/eslint-config-cdrainxv#readme)

One thing that I always wanted to create was my own library, albeit a small one,
that was easily maintainable. It seemed out of reach considering all the
libraries I had seen on github had countless files and extensions I had
previously never seen before. However, I watched this amazing course,
[Creating an Open Source JavaScript Library](https://www.lynda.com/JavaScript-tutorials/Creating-Open-Source-JavaScript-Library/604269-2.html),
which proved to be a wealth of knowledge. Following along with it, but
implementing my own version, I learned a lot relatively quickly about how to
create, maintain, update, and contribute. The driving message was about making
your libraries, projects, etc inviting so that others can contribute to it. I
have seen some projects with that same kind of welcoming view to first-time
contributors which I also want to incorporate in my own libraries, starting with
this one.

---

## CHANGELOG

---

### vim

---

I have picked up some interesting aspects of vim that I either didn't know
existed or just didn't know how to implement. Here are a few of them:

<details>
<summary>Things learned in vim</summary>
<br>Usage and difference b/t tabs and buffers.
<br>Autoreload files -- looking at you package.json :rage.
<br>Write custom snippets using UltiSnips plugin (tutorial upcoming?).
<br>Use vim spell to check, add, and fix spelling.
<br>Configure some plugins. Reading the docs has gotten considerably easier.
</details>

I found a great resource to learn
[**vimscript**](http://learnvimscriptthehardway.stevelosh.com/)! :smile

### tmux

---

Additionally I dived even deeper into tmux. I love how easy it is to integrate
it with vim. I read another book to help reinforce my understanding of some of
the commands, but also what is happening in the background, such as how the
servers are spun and where they exist. One thing that I did come across was
[gpakosz][https://github.com/gpakosz/.tmux] when I was looking into customizing
my tmux status-line from the default bland one. That repo boasts extensive
configurability, coming fully loaded with battery status, time, date, powerline
symbols, etc, however the added bonus is the builtin binds. These are very easy
to learn and memorize, considering that they are set to be familiar to vim
users, making use of the same directional, and vi-copy/paste commands. This
inspired me to write my own plugin to add to the tmux status-line, a spotify
song playing displayer using applescript. That post is
[here](https://cdrainxv.github.io/blog/applescript/2017/12/01/applescript-spotify-muter.html).

## WEEKLY GOALS

---

* I didn't get to much of what I listed as goals in last week's
  [Weekly Update](https://cdrainxv.github.io/blog/weekly-update/2017/11/25/weekly-update-dev-setup.html#changelog).
  It's disappointing considering that I want to be able to start seeking out
  jobs by the end-of-the-year/beginning-of-the-new-year time frame. It's an
  ambitious goal, but I have to set some kind of goals if I want to jump-start
  my career in software development early in 2018. It's not that I don't know
  react (I do), it's a matter of having a better grasp of it (and learning
  testing and incorporating redux (or some form of state management)). This week
  I will strive to create and post (here and a live version) of projects
  (regardless of size and difficulty), which both incorporate redux and some
  testing.

* Finish the **vimscript** book and create something from the knowledge attained
  from it to reinforce the learning.

* Finish the [anml](https://github.com/cdrainxv/anml). By finish I mean have it
  ready for initial release on npm. Furthermore I noticed some more test cases I
  have not accounted for as of yet, but I can create PRs for them. I somehow
  have to get others to contribute to them, based on the merit that it will be a
  great and easy first-commit.

* I have several drafts of which I have yet to post. This week I hope to have
  all of them fully written up and posted.

---

### GOOD LUCK!
