---
layout: post
title:  "Semantic Versioning"
date:   2017-10-16 20:14:31 -0600
categories: semver
---

# Semantic Versioning
I have been creating node projects for a few months now, but I've noticed that the version number in my package.json is always at 1.0.0, regardless of any new changes, fixes, or functionality I add to the projects. An evergrowing, maintainable project should have different versions to log the journey.

## So... How Do We Keep Track Of Our Projects?
The solution is semantic versioning, which is defining a project version as `X.Y.Z`, where:
> X is major version

> Y is minor version

> Z is patch version

What does this mean? Let's take a look at express for example. Currently it's at version 4.16.2:
> 4 is major version

> 16 is minor version

> 2 is path version

Changes to the version number are dependent on the changes to the project. The patch number (Z) is increased for any bug fixes. The minor version (Y) is increased for any new functionality and/or featuresadded to the project.

***NOTE:*** For minor and patch versions, there must be no breaking changes, i.e the current version cannot not have any conflicts with any previous versions on the same major version.

Lastly, the major version is increased if, and only if, a breaking change is introduced, such as changing the APIs.

## What Version Should We Start Our Projects Then?
Hmmm... I usually generate my package.json using `npm init -y` to bypass all the initial configurations as I know I'll come back to it later. The default version in package.json is `1.0.0` and that's the one I will use as my start-off point. However, I have also seen versions starting at `0.0.0`. This means it's up to the project owner/company to decide when/where to introduce new versions depending on their release schedule.

## Example Please!
Of course! Let's have a simple one to explain the concepts above. You can follow along if you like, but the example should be simple enough just as a visual - in either case you can always refer to the repo [semver-example](https://www.github.com/cdrainxv/semver-example).

1) Make a directory to place your project in and initial it as a node project.
> mkdir semver-example && cd semver-example

> npm init -y


You should now have a package.json somewhat like this (note: mine has been configured):

    {% highlight javascript linenos %}
    {
      "name": "semver-example",
      "version": "1.0.0",
      "description": "simple semver example",
      "main": "index.js",
      "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
      },
      "keywords": ["semver"],
      "author": "cdrainxv",
      "license": "ISC"
    }
    {% endhighlight %}

2) Next we need an **index.js** and **math.js**. In **math.js** we will add a Math class with add and substract methods, whereas in **index.js** we will import that Math class and console the results of the add and sub methods to the console.

__math.js__


    {% highlight javascript linenos %}
    'use strict'
    /**
    * Math class

    * @constructor
    * @param {Number} x - operand
    * @param {Number} y - operand
    */

    function Math (x, y) {
      this.x = x
      this.y = y

      this.add = function(x, y) { return this.x * this.y }

      this.sub = function(x, y) { return this.x - this.y }
    }

    module.exports = new Math(9, 3)
    {% endhighlight %}


__index.js__


    {% highlight javascript linenos %}
    const Math = require('./math')
    const add = Math.add()
    const sub = Math.sub()

    console.log('add:', add)
    console.log('sub:', sub)
    {% endhighlight %}

3) Hmmm... strange. If we run our current project in the node console (`node index.js`), we get this:

> add: 27

> sub: 6


Surely the addition of `9 + 3` is ***12*** and not 27! It seems like we have a bug here! Looking at this line: 
`this.add = function(x, y) { return this.x * this.y }`, we have used the wrong operator. Let's fix this bug by making this change (i.e replacing * with +):

{% highlight javascript %}
this.add = function(x, y) { return this.x + this.y }
{% endhighlight %}

Since this is a bug fix (aka patch), semver requires that this change is reflected in our version, therefore we should change our version to `1.0.1` in our package.json.

4) The add and sub methods are bored of each other - they want more company. Let's add a div and mult function in the Math class.
    
    
__index.js__


    {% highlight javascript linenos %}
    const Math = require('./math')
    const add = Math.add()
    const sub = Math.sub()
    const div = Math.div()
    const mult = Math.mult()

    console.log('add:', add)
    console.log('sub:', sub)
    console.log('div:', div)
    console.log('mult:', mult)
    {% endhighlight %}


__math.js__


    {% highlight javascript linenos %}
    'use strict'
    /**
    * Math class

    * @constructor
    * @param {Number} x - operand
    * @param {Number} y - operand
    */

    function Math (x, y) {
      this.x = x
      this.y = y

      this.add = function(x, y) { return this.x + this.y }

      this.sub = function(x, y) { return this.x - this.y }

      this.div = function(x, y) { return this.x / this.y }

      this.mult = function (x, y) { return this.x * this.y }
    }

    module.exports = new Math(9, 3)
    {% endhighlight %}

Wait a minute! New functions?! Aren't these new features in the Math class? Yes they are, which means a minor version bump is necessary. In the package.json, change the version to this: `1.1.1` to reflect the new functionality/features added to the project. Just as a check that no new bugs were introduced, run `node index.js` in your terminal to verify you get this response:
    
    {% highlight javascript %}
    add: 12
    sub: 6
    div: 3
    mult: 27
    {% endhighlight %}

5) Everything appears to be well and dandy, but we have yet to update our major version. I think it's time to introduce a breaking change. Update your index.js and math.js files to resemble the following:

    
__index.js__


    {% highlight javascript linenos %}
    const Math = require('./math')

    const results = new Math(12, 3).results()

    const add = results.add
    const sub = results.sub
    const div = results.div
    const mult = results.mult

    console.log('results', results)
    console.log('add:', add)
    console.log('sub:', sub)
    console.log('div:', div)
    console.log('mult:', mult)
    {% endhighlight %}


__math.js__


    {% highlight javascript linenos %}
    'use strict'
    /**
    * Math class

    * @constructor
    * @param {Number} x - operand
    * @param {Number} y - operand
    */

    function Math (x, y) {
      this.x = x
      this.y = y
    }

    Math.prototype.results = function(x, y) {
      return {
        add: (function(x, y) { return this.x + this.y }).call(this, x, y),
        sub: (function(x, y) { return this.x - this.y }).call(this, x, y),
        div: (function(x, y) { return this.x / this.y }).call(this, x, y),
        mult: (function(x, y) { return this.x * this.y }).call(this, x, y)
      }
    }

    module.exports = Math
    {% endhighlight %}

What's new? We removed the four methods we had in our class and placed them in a results method. With the results method we can return the results of all the operations in an object or individually. Another thing to consider is this line in **math.js**: `module.exports = Math`. We are now just exporting the class without instantiating it. Before we instantiated it with values of 9 and 3. This was restrictive because the user then had to change the values in the **math.js** file before making use of the Math class in the **index.js** file.

Now the important question - is this a breaking change? We can determine this by trying to make use of the functionality we had in the previous versions. Recall we had a `add(), sub(), div(), mult()` methods on our Math class. Do they still work? Let's find out! Comment out the the entire **index.js** file except for the require line. Your file should have the following at the top:
   

__index.js__


    {% highlight javascript linenos %}
    const Math = require('./math')
    const add = Math.add()

    console.log('add:', add)
    {% endhighlight %}

Running `node index.js` outputs:
    
    {% highlight javascript %}
    TypeError: Math.add is not a function
    {% endhighlight %}

And rightly so as the add (and the other functions) are not methods on the Math class anymore. Furthermore, we haven't even instantiated the Math class with any values to perform any operations. Revert our changes backto what was shown in 5). Run `node index.js` and compare your results to this:
    
    {% highlight javascript %}
    results: { add: 15, sub: 9, div: 4, mult: 36 }
    add: 15
    sub: 9
    div: 4
    mult: 36
    {% endhighlight %}

Everything works now! We get a results object with the keys of the operations and the results of the operations as the values. This refactoring allowed us to include our own values to be operated on and also gave us options as to what to return, either a results objects or individual results from particular operations.

Oh right! Something is still amiss. We had decided that this was a breaking changing (i.e this current version is not backward-compatible with the previous version). We need to let the users of our project know that by changing the major version number to `2.0.0`.

## HOLD UP!!!

6) Something was bothering me during this project and I hope it bothered you as well - the Math class itself. There already exists a Math object in JS with its own prototypes, such as Math.random(), and it is not wise to add to it or any other defined objects JS unless we really, really, really require the extra functionality and want to extend the object. We want to be wary of adding our own methods to defined objects in JS because we don't know if in the future JS might include a function identical to ours (in name at least) to their objects and we could inadvertently override the default functionality with our own.

I think we should update the naming of the Math class to MathOps (Math Operations) in the math.js file and we might as well change Math to mathOps in the index.js file to match:
    

__math.js__


    {% highlight javascript linenos %}
    'use strict'
    /**
    * Math class

    * @constructor
    * @param {Number} x - operand
    * @param {Number} y - operand
    */

    function MathOps (x, y) {
      this.x = x
      this.y = y
    }

    MathOps.prototype.results = function(x, y) {
      return {
        add: (function(x, y) { return this.x + this.y }).call(this, x, y),
        sub: (function(x, y) { return this.x - this.y }).call(this, x, y),
        div: (function(x, y) { return this.x / this.y }).call(this, x, y),
        mult: (function(x, y) { return this.x * this.y }).call(this, x, y)
      }
    }

    module.exports = MathOps
    {% endhighlight %}


__index.js__


    {% highlight javascript linenos %}
    const mathOps = require('./math')

    const results = new mathOps(12, 3).results()

    const add = results.add
    const sub = results.sub
    const div = results.div
    const mult = results.mult

    console.log('results', results)
    console.log('add:', add)
    console.log('sub:', sub)
    console.log('div:', div)
    console.log('mult:', mult)
    {% endhighlight %}

Awesome! Now we can go to bed without any nightmares. Last step is to update the patch version to reflect the bug fix: `2.0.1`.


---


Great! That's just about the gist of semantic version. For an indepth look into it, reference [semver](http://semver.org/).

Just as a reminder, you can take a look at the example project here: [semver-example](https://www.github.com/cdrainxv/semver-example)

Thanks!!!