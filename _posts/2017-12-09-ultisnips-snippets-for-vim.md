---
layout: post
title: 'UlitSnips: Snippets For VIM'
date: 2017-12-09 14:02:34 -0600
categories: vim
---

# INTRO

Most editors, even the most basic of ones, come equipped with autocomplete
and/or text expanding features. Most likely you would just download a packages
with the preloaded snippets for the language/framework/filetype that you are
working on. However, with a text editor like VIM, I want my snippets to be as
personal as my **.vimrc** from the onset, with everything configured just so.
This makes the triggers for the autocompletion easier to remember and greatly
improve productivity as whole code layouts could just be triggered by a few
keys.

# INITIALIZATION

Using your favorite vim plugin, install
[ultisnips](https://vimawesome.com/plugin/ultisnips):

**.vimrc**

```bash
call plug#begin('~/.vim/plugged')
  Plug 'sirver/ultisnips'
call plug#end()
```

Make sure you to source your **.vimrc** file prior to installing it.

Since we will setup our own snippets we have to let UltiSnips know where they
are stored. We can do this by adding the directory to our runtimepath.

**.vimrc** `set runtimepath+=~/.vim/snips/`

Now create that same directory in the runtimepath. Additionally, **inside** of
the snips folder UltiSnips requires an "UltiSnips" folder from which it can read
your snippets.

```bash
cd .vim
mkdir -p snips/UltiSnips
```

## CREATING A SNIPPET

I write blog posts in markdown and at the head of every blog post is a template
similar to this:

```
---
layout: post
title: 'Applescript: Spotify Muter'
date: 2017-12-01 13:15:20 -0600
categories: applescript
---
```

I do not want to type that up **everytime** I write a post, especially since
only the title, date, and categories values change. Let's create a snippet that
will allow us to TAB to the title and categories fields add some text and exit.
The date field should be automatically generated using the current date and
time.

```bash
vim .vim/snips/UltiSnips/markdown.snippets
```

Snippets for specific filetypes must be named after the filetype (all lowercase)
and followed with the extension **.snippets**.

**./vim/snips/UltiSnips/markdown.snippets**

```bash
# blog post header
snippet head "blog posts header" b
---
layout: post
title: '$1'
date: `date '+%Y-%m-%d %H:%M:%S'` -0600
categories: $0
---
endsnippet
```

### Here's the breakdown:

* snippet: Start of snippet
* head: Keyword that will trigger the snippet. In this case it's "head".
* "blogs posts header": Descriptor of the snippet. Useful if you have several
  snippets with the same name.
* b: One of several options. "b" means that the trigger word **_must_** on its
  own line, i.e can't be in the middle of a word or sentence, etc.
* $1: TAB stop. When you press TAB (or your configured trigger key), this is
  where the cursor will land. You can add your input here.
* `date '+%Y-%m-%d %H:%M:%S'` -0600: This uses shell interpolation to get the
  current date and time.
* $0: Another TAB stop, but after inputting data you will exit out of the
  snippet completion.

Here's a demo of it in action: ![snippets_demo]({{
"/assets/images/snippets_demo.gif" | absolute_url }})

## REFERENCES

That was just a basic setup and example. To learn more of the features and
additional configurations reference these sources:
[ultisnips repo](https://github.com/sirver/ultisnips)

[vimcasts: ultisnips](http://vimcasts.org/episodes/meet-ultisnips/)

Also of note is [vim-snippets](https://github.com/honza/vim-snippets) if you
want to use a premade community-driven snippets library configured for various
programming languages.

You can also check out my sparse snippets library,
[snips](https://github.com/cdrainxv/snips).
