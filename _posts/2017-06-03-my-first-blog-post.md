---
layout: post
title:  "My First Blog Post"
date:   2017-06-03 04:05:21 -0700
categories: jekyll update
---

No seriously, this is my very first official blog post!  I'm still figuring out how things work so I'll be making updates often in the upcoming days.

The purpose of this blog is to share what I've learned throughout my coding journey.  With this, I'm hoping I can help fellow coders out there with their challenges as many of the existing blogs have helped be with mine.  A pay-it-forward if you will.

# Jekyll

I made this blog with [Jekyll](https://jekyllrb.com) and it was _extremely_ easy.  The hardest part was figuring out what I wanted.  There were a couple of tweaks I had to make so if you're starting a blog using [Jekyll](https://jekyllrb.com) I hope this will help you.  I will assume that you have read the [Jekyll docs](https://jekyllrb.com/docs/home) to have the basic understanding of how it works.

I went with [jekyll-swiss](https://github.com/broccolini/swiss) theme which is very simple and elegant.  However, I ran into one issue.  Since this is a coding blog, I needed syntax highlighting to work out of the box which this theme doesn't have good support for.

Before

![Old code block]({{ site.url }}/assets/images/old-code-block.png)

After
```ruby
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
```

# Pygments

The first step is to choose the theme of syntax highlighting from [pygments-css](https://github.com/richleland/pygments-css).  I went with **[vs.css](https://github.com/richleland/pygments-css/blob/master/vs.css)**.  This stylesheet will need to be referenced by your main page to enable syntax highlighting.

Since I may need to add other stylesheets in the future, I don't want to keep updating my main page to include the particular stylesheet.  [Jekyll](https://jekyllrb.com) has support for processing partial `sass` files into one stylesheet so we will take this route.

# Sass

Create a directory called `_sass` from the root of your project.  Add a file called `_syntax.scss` and copy the styles of the pygment theme you chose.  We'll need to import this into our `main.scss` next.

Create a directory called `assets` from the root of your project.  Add a file called `main.scss` and add the following:

```sass
---
---

@import 'syntax';
```

This will import `_syntax.scss` and when `main.scss` get processed, `main.css` will be available in `_site/assets` for us to use.

Here's what my project looks like:

```shell
├── _assets
|   └── main.scss # where partial sass files get imported
├── _layouts
├── _posts
├── _sass
|   ├── _syntax.scss # copy the content of the pygment styles
├── _site
├── _config.yml
├── about.md
└── index.md 
```

The last step is to reference `main.css` in the `head` of my main page.

# Overriding <head>

The main page `<head>` is in a file called `head.html` where [jekyll-swiss](https://github.com/broccolini/swiss) contents are.  Since I just need to add a line to the existing `<head>`, I'm just going to reuse the theme's `head.html`.  You can read more about overriding theme defaults in [Jekyll docs](https://jekyllrb.com/docs/themes).

Open terminal and run the following:

```shell
$ open $(bundle show jekyll-swiss)
```

This will open the content of the theme in finder.  Copy `_includes/head.html` into `_includes` directory of your project.  Your project structure should look like this:

```shell
├── _assets
|   └── main.scss 
├── _includes
|   └── head.html # head override
├── _layouts
├── _posts
├── _sass
|   ├── _syntax.scss 
├── _site
├── _config.yml
├── about.md
└── index.md 
```

Edit `head.html` and add the following in the `<head>` tag:

{% raw %}
<pre>
  <link rel="stylesheet" href="{{ "/assets/main.css" | prepend: full_base_url }}">
</pre>
{% endraw %}

We're finally done.  You can run your app and ogle at the results.

```shell
$ bundle exec jekyll serve
```

# Liquid Tags vs Triple Backticks

In [Jekyll docs](https://jekyllrb.com/docs/posts), they suggest writing code blocks using Liquid tags.  This works in `html` files but I personally think it's too verbose when writing posts using markdown.  You can accomplish the same thing using triple backticks followed by a language name.

<pre>
```javascript
let x = 1;
```
</pre>

I hope this helped you out some.  I'm really liking [Jekyll](https://jekyllrb.com) so far and I hope you will give it a try.

Happy coding!