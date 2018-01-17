---
layout: post
title: 'EcmaScript: Tagged Template Literals'
date: 2018-01-17 02:54:53 -0600
categories: ecmascript
---

I have been using ES6+ (meaning EcmaScript 2015 and beyond) for a while
now, not just to use the most latest features in JavaScript, but also
because most blog posts, documentations, examples, etc assume that you
are/will be using them as well. Therefore, I have come across and used most
of them (especially with regards to libraries like react and vue or in building npm packages), but some of them have either flown over my head or the use case never arose. Lets examine of them below.

## Tagged Template Literals

I love template literals, but I only just realized that I haven't been using it
to its full potential; my uses have been limited to creating multi-line
strings, expression interpolation, and nesting templates. Currently I have been
trying to learn graphql (which in turn means I have to either learn apollo or
relay) and in the relay docs I noticed that they used something
quite similar to template literals for the 'Relay.QL' language, but it was
called **tagged** template literals. Now, I have heard of tagged template
literals before, but at that point I didn't fully understand it, but I decided
to take a second look at it, not just for a new understanding, but for usage as well
because I felt that I was missing out on it.

# TAG ME

```js
const tag = str => console.log(str)
tag`Hello World!` // ['Hello World']
```

First of all, take notice of how we take the template literal with the function name. Additionally how our string is placed inside of an array. How about if we had variables we wanted to inject into
our string?

```js
const year = 1756
const age = (str, year) => console.log(str, year)
tag`Hello World! I was born in ${year}.` // `[ 'Hi World! I was born in ', '.' ] 1756`
```

Now the output is: `[ 'Hi World! I was born in ', '.' ] 1756`. The strings are separated at each point where an expression is injected.

Now we can use this knowledge to change what is returned when we call a tagged
function.

```js
const year = 1756
const age = (str, year) => {
  const myAge = new Date().getFullYear() - year
  return `Str.join(year) I am ${myAge} years of age.`
}

const myAge = age`Hello World! I was born in ${year}.`
console.log(myAge) // 'Hello World! I was born in 1756. I am 262 years of age.'
```

This was a rather simple example, but as we saw, tagged template literals give us access
to the strings (in a array) and the expressions we pass into them. Now we
have at our disposal the ability to 'step in' and modify a string before the
function gets returned.
