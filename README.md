# Report Card Generator website #
A report card generator for robots' manipulation tasks - website

## How to post ##

Put a `Markdown` formatted file named `YYYY-MM-DD-post-name.md` in `_posts` folder.  
The content should be:

```Markdown
---
layout: post
title:  "Post title"
date:   YYYY-MM-DD hh:mm:ss
categories: post categories <--! optional field -->
external-url: http://ext.url/loc/ <--! in case the post redirects to external website -->
---

# Content of the post #
Body of the post.

# Posts support advanced syntax colouring #

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

```
