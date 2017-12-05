---
layout: post
title: 'Creating A CLI App: Part II'
date: 2017-12-05 00:23:48 -0600
categories: cli
---

This is a continuation of the
[Creating A CLI App](https://cdrainxv.github.io/blog/cli/2017/11/18/creating-a-cli-app.html).
At that point I stopped at only implementing functionality to get either a
**specific** comic or the latest one. I wanted to able able to also allow a user
to be able to view a **previous** and **next** comic based on the current comic
they are are viewing. Additionally, an added feature would be to get a random
comic ranging from the initial comic to the most current one (1924 at the time
of this post).

It seemed like it would be easy to implement (and it proved to be), but at that
point I realized all those new options (random, next, and previous) were
stringent on the current comic number being somehow referenced. As the most
current number is updated 3 times a week, I had to find a way to be able to save
it, store it, and update it quite easily. I thought about using an environment
variable, however that would mean creating environment variables for users,
which I did not want to do. Another approach would be using a database like
postgreSQL, but I don't believe that database is OS agnostic.

Today, while I was just perusing npmjs, I came across the perfect package:
[data-store](https://www.npmjs.com/package/data-store). It was exactly what I
needed! It was so easy to add the new functionality -- just like I believed it
would be.

## IMPLEMENT PREVIOUS, NEXT, AND RANDOM

1. Require **data-store** package: `npm i data-store -S`

   > const store = require('data-store')

2. Add the new options in:

   ```js
   program
     .version('0.1.0')
     .option('-c, --current', 'most current comic')
     .option('-p, --previous', 'previous comic from current')
     .option('-n, --next', 'next comic from current')
     .option('-s, --specific <value>', 'specific comic')
     .option('-r, --random', 'random comic')
     .parse(process.argv)
   ```

3. In fetchComic function, get the num value from json. Use the num value to
   **set** the next and previous keys in the data store.

   ```js
   async function fetchComic(url) {
     const response = await fetc(url)
     const data = await response.json()
     const { num, alt, img, title } = data

     store
       .set('current', num)
       .set('previous', num - 1)
       .set('next', num + 1)
   }
   ```

4. Add the cases for the new options:

   ```js
   switch(process.argv[2]) {
   ...

   case '-p': {
       return fetchComic(`https://xkcd.com/${store.get('previous')}/info.0.json`)
   }

   case '-n': {
       return fetchComic(`https://xkcd.com/${store.get('next')}/info.0.json`)
   }

   ...
   }
   ```

5. As for the random comic feature, we could use a basic `Math.random()`,
   however we don't want it to be **_just_** random, but also **_unique_**.
   Thankfully there's a package for that:
   [unique-random](https://www.npmjs.com/package/unique-random). This package
   ensures that you don't get the same random comic twice in a row.

   `npm i unique-random -S`

   > const rand = require('unique-random')

Add the random option in your switch statement:

    ```js
     ...

     case '-r': {
       return fetchComic(`https://xkcd.com/${rand(1, store.get('current'))}/info.0.json`)
     }
     ...

    ```

## NEXT STEPS

Prezto! We have the core functionalities completed, although there's no testing
included. Ideally we would start testing and work backwards to get the core
features, but I am trying to find a new test runner and/or framework to
experiment with, other than just being content with mocha and chai. Hopefully,
by the end of the week, I will have made my decision and added some units tests
in. As for now, I added a git tag on it (v0.1.0) and published it on npm as
[xkcli](https://www.npmjs.com/package/xkcli).
