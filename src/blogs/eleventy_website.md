---
layout: 'layouts/blog.html'
tags: blog
date: 2025-04-29
length: 7 minutes
title: Making a Personal Website With 11ty
---

I've wanted to create a personal website for a while, but always seemed to find a reason to put off actually working on it. I decided the best time to start working on it would be two weeks out from the submission deadline of my Masters thesis, so here we are.

{% img "https://miro.medium.com/v2/0*ZjYSm_q36J4KChdn", "this is fine meme", 30 %}

I knew there were a couple non-negotiable conditions for my website (in order of importance):

1. Minimal code bloat
2. Powerful extensibility
3. Minimal design
4. Ability to blog

I personally care quite a bit about how the final website is going to look and feel aesthetically, but I know that aspect is easily customizable - my tech stack however, isn't. I didn't want to have to slam an entire framework into my pipeline, and also figured I would need something a little more powerful than rawdogging HTML by itself. Of course this led to the natural conclusion I should just work with a static site generator (SSG).

SSGs are tools that generate static HTML pages based on user provided data and templates. This usually means the user can use more convenient syntax when composing content, such as Markdown, while the SSG does the heavy lifting to port it to nice looking HTML. I've worked with Jekyll, Nuxt, Vite, and Sphinx before (either failed attempts at this project or for school), and always found myself having complaints. Usually, the problem boiled down to the project trying too hard to make my life easy, and as a result offering a subpar selection of features. If I wanted more features like code blocks or callouts, I would need to hack on the SSG itself, which felt like it defeated the purpose of using an SSG in the first place.

I found my solution in [11ty](https://www.11ty.dev/) - an ultra lightweight SSG that leaves the majority of the customization to the user. It still had all the convenient aspects of using an SSG like a Markdown parser, file hierarchy management, and collections of content types, but allowed me to extend the functionality without ripping into the engine too much.

I found [this tutorial](https://learn-eleventy.pages.dev/) on 11ty to be extremely helpful. I prefer to see code by example, but somewhat ironically if the tutorial is guiding me through a project, I prefer to read and learn, **then** implement my own project. I find it a waste to reimplement something that I'm being shown the code for on the screen, especially considering it will probably have little relevance to my final project anyway.

# Setting up 11ty

Okay, that's enough yapping - let's get to some actual code. We first start out with project setup:

```bash
mkdir eleventy-website
cd eleventy-website
npm init -y
npm install @11ty/eleventy
```

We want to define an input and output folder, which by default are set to `.` and `_site` respectively. To do this we will create `.eleventy.js` at the root of our project. This is the main config file for 11ty, and we'll be coming back to it in the future.

```js
// .eleventy.js

module.exports = (eleventyConfig) => {
  return {
    dir: {
      input: "src",
      output: "dist",
    },
  };
};
```

Let's get something running! Make the `src` directory and add the following `index.md` (don't change the filename!):

```bash
mkdir src
touch src/index.md
```
```md
Hello World!
```

Now run the following command:

```bash
npx eleventy --serve
```

You should see the server running with your file at [`http://localhost:8080/`](http://localhost:8080/)!

# Templating

One of the most powerful features of SSGs is the ability to create reusable templates. In 11ty, this takes two forms - layouts and partials. **Layouts** act as scaffolding for the HTML generated from your Markdown files, while partials are reusable pieces of code that can be used in a layout. Let's see this in action!

First we need to choose a templating syntax and inform 11ty through our config file. I'm choosing `nunjucks`, which has a syntax very closely derived from `jinja2`:

```js
// .eleventy.js

module.exports = (eleventyConfig) => {
  return {
    markdownTemplateEngine: "njk",
    dataTemplateEngine: "njk",
    htmlTemplateEngine: "njk",

    dir: {
      input: "src",
      output: "dist",
    },
  };
};
```

Next, create the directories that will house our layouts and partials. Both are contained in the `_includes` folder.

```bash
mkdir -p src/_includes/layouts
mkdir -p src/_includes/partials
```

Let's first make a basic base template in `src/_includes/layouts/base.html`:

{% raw %}
```jinja2
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1.0" />
	</head>
	<body>
		{% block content %}{% endblock %}
	</body>
</html>
```
{% endraw %}

The most important thing to note here is the `content` block - this is where other layouts can inject themselves to create more powerful compositions. Now that we have a base layout, let's do just that and make a new **home** layout in `src/_includes/layouts/home.html`

{% raw %}
```jinja2
{% extends "layouts/base.html" %}
{% block content %}
<article>
	<h1>{{ title }}</h1>
	{{ content | safe }}
</article>
{% endblock %}
```
{% endraw %}

Note here how we request to use the base template, and define the `content` block the base template is looking for. `content` refers to the Markdown in the file we will connect this layout to, while `safe` is a function we pipe the content into to indicate it can be taken without escaping any text - perfectly fine when we write the content. The `title` comes from metadata we will shortly pass into the layout.

Before we connect this to our Markdown file, let's also create and use a partial. We might want a **header** partial that contains the site header which we can click on to go home. Let's create this in `src/_includes/partials/header.html`

```html
<a href="/">Site Title</a>
```

To use this, we can simply put the following code in any layout, or even another partial:

{% raw %}
```jinja2
{% include "partials/header.html" %}
```
{% endraw %}

Whew! That was a lot to take in - let's reap the rewards by hooking everything together. We can add YAML frontmatter to any Markdown file by enclosing it in `---`. Let's modify our `index.md` as follows:

```md
---
layout: 'layouts/home.html'
title: Awesome Title!
---

Hello World!
```

The fields in the frontmatter will be sent to our layouts for use - some special fields like `layout` or `date` are processed separately by the engine. Here we tell 11ty to use the `home.html` layout we just created to render this site. You should now see the title and header on the page when your project rebuilds!

# Passthrough

One very nice aspect of 11ty is the ability to take folders from your `src` directory, and spit it out exactly into the output directory. This is helpful when you want to easily reference images, css styles, or any other auxiliary data the same way in both development and production. We can do this simply by adding the following line in our config:

```js
module.exports = (eleventyConfig) => {
  eleventyConfig.addPassthroughCopy("./src/imgs/");
  ...
```

# Plugins

11ty offers many plugins like syntax highlighting that are very useful. To enable them, install the correct package, then change the config as follows:

```js
const syntaxHighlight = require("@11ty/eleventy-plugin-syntaxhighlight");

module.exports = (eleventyConfig) => {
  eleventyConfig.addPlugin(syntaxHighlight);
  ...
```

# Shortcodes

It is possible to hijack the Markdown to HTML parser by passing in your own subparser for some specific syntax, i.e. if you want admonitions / callouts. However I found this to require too much code for the minor benefit of nicer syntax in my Markdown, and instead I turned to using shortcodes. 

Shortcodes are escape sequences in Markdown that translate to a user defined function in our config. I wanted to make a shortcode that would format images nicely for me - centering them and allowing me to choose how much space they took up. To do this, I added the following to my config:

```js
module.exports = (eleventyConfig) => {
  eleventyConfig.addShortcode("img", function (src, alt, width) {
    return `<img src="${src}" alt="${alt}" loading="lazy" style="width:${width}em;">`;
  });
  ...
```

And using them is as simple as writing the defined shortcode in Markdown - the shortcode below produced the meme at the beginning of this blog!

{% raw %}
```md
{% img "https://miro.medium.com/v2/0*ZjYSm_q36J4KChdn", "this is fine meme", 30 %}
```
{% endraw %}

# Lines Numbers and Copy

Having line numbers in my code and a copy button were two big features that caused me to disregard or skip many templates and frameworks. I thought they were critical pieces of functionality if I wanted to do anything related to code on my website. However, I ended up having neither on this website after thinking about it enough for these reasons:

- Reduced JavaScript bloat on the copy functionality, plus (most) people can click and drag easily enough
- If my code needs line numbers, the snippet is probably too long
- If my snippet isn't too long, then my explanation probably needs work
- Styling both is a huge pain

# Closing

These are the building blocks I built my website from. The other parts were standard web development - finding some libraries for effects, choosing a nice colorscheme, and banging my head trying to figure out how to center a div. I'm pretty happy with the initial end result, and I'm excited to further use 11ty to add more functionality.
