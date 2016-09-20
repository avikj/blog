---
layout: post
title: Discovery&#58; Syntax Highlighting on Facebook Messenger
categories: discovery
---

If you didn't already know, [messenger.com](https://messenger.com), Facebook Messenger's official web client, has syntax highlighting functionality. I stumbled upon this feature rather unexpectedly a few months ago, and I'm pretty sure it was one of Facebook's best-kept secrets until then (aside from influencing public opinion with politically biased Trending headlines). Here's how I discovered it, and how you can use this feature. 

![Syntax Highlighting on Messenger.com](/images/messenger-highlighting.png)

## The Discovery

It was a cold April morning, and I was testing the basic functionality of my latest project [fbash](http://avikjain.me/fbash), a command line tool which essentially lets you run terminal commands on your computer remotely through Facebook Messenger. One of the first commands I tested was, of course, `cat`, which prints the contents of a file to the terminal.

I wanted to try printing the contents of a larger file to see how it would appear on Messenger, so I ran `cat README.md`. I expected a bunch of plain text to show up, but here's what actually appeared:

![Messenger trying to display my README file](/images/messenger-readme.png)

To say that this output was shocking would be an understatement.

The areas which I expected to be displayed as code on Github were somehow being displayed as code in cool yellow bubbles on Messenger itself. I knew I definitely didn't hack Messenger and implement that UI feature, so it had to have been built into Messenger. After some experimentation, I figured out the syntax to display arbitrary text as code, with specified syntax highlighting. It turns out that the syntax is identical to that of markdown, which is why blocks from my README were displayed as code. I actually took advantage of this feature later to add a [showcode](https://github.com/avikj/fbash/blob/master/DOCS.md#showcode) command to [fbash](http://avikjain.me/fbash).

## So how do you do it?

If you've used markdown and written README's for code projects before, the syntax will be familiar:
````
```
Whatever text is placed between sets 
of 3 backticks is displayed as code
```
````
And if you want to add syntax highlighting, just specify the language after the first set of backticks:
````
```javascript
// This will be highlighted as Javascript code
```
````

I don't know why the engineers at Facebook decided to implement this feature in a product targeted at non-developers, or why it isn't mentioned anywhere on the internet. I'm just glad that I discovered it, because it's made talking about code and sending code snippets over Messenger a much better experience.

*If you find this functionality useful and use Messenger frequently, check out [fbash](http://avikjain.me/fbash); it uses this as well as other Messenger features to provide an enhanced SSH experience through Facebook's messaging service.*


