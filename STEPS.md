# _Eleventy_ the static site generator that we use for this project.

## _Step-by-step:_

### 1. Create a folder for the Eleventy project.

### 2. Inside the folder create a folder named `src`.

### 3. Create the following folders inside the `src` folder:

- images
- people
- posts
- work

### 4. Create a new file in your project folder called `.eleventy.js` and add the following to it:

```
module.exports = config => {
  return {
    dir: {
      input: 'src',
      output: 'dist'
    }
  };
};
```

This is our **Eleventy Config file**.

### 5. Set up the _package.json_. Inside project folder, run the following in the terminal:

`npm init -y`

The output should be the following:

```
{
  "name": "eleventy-from-scratch",
  "version": "1.0.0",
  "description": "",
  "main": ".eleventy.js",
  "scripts": {
    "start": "npx eleventy --serve"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

### 6. Install _Eleventy_ inside project folder. In the terminal, run the following command:

`npm install @11ty/eleventy`

### 7. Inside _src_ folder, add a new file called `index.md` and add the following to it:

`Hello, world`

### 8. Run `npm start` to the terminal and view the server in http://localhost:8080 in the browser.

### 9. Add the following lines, after the `return {` statement, inside `.eleventy.js`:

```
markdownTemplateEngine: 'njk',
dataTemplateEngine: 'njk',
htmlTemplateEngine: 'njk',
```

The file should now look like this:

```
return {
  markdownTemplateEngine: 'njk',
  dataTemplateEngine: 'njk',
  htmlTemplateEngine: 'njk',
  dir: {
    input: 'src',
    output: 'dist'
  }
};
```

The code added should now be processed by Nunjucks **.njk**.

### 10. Run the following code in the terminal to create subfolders into the _src_ folder.

```
mkdir -p src/_includes/layouts
```

### 11. Inside `src/\_includes/layouts`, add a new file called `base.html` and paste the following:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>{{ title }}</title>
  </head>
  <body>
    {% block content %}{% endblock %}
  </body>
</html>
```

### 12. Inside `src/\_includes/layouts`, add a new file called `home.html` and paste the following:

```
{% extends "layouts/base.html" %} {% block content %}
<article>
  <h1>{{ title }}</h1>
  {{ content | safe }}
</article>
{% endblock %}
```

### 13. Open the `index.md` and replace all the codes with the following _Front Matter_:

```
---
title: 'Hello, world'
layout: 'layouts/home.html'
intro:
  eyebrow: 'Digital Marketing is our'
  main: 'Bread & Butter'
  summary: 'Let us help you create the perfect campaign with our multi-faceted team of talented creatives.'
  buttonText: 'See our work'
  buttonUrl: '/work'
  image: '/images/bg/toast.jpg'
  imageAlt: 'Buttered toasted white bread'
---

This is pretty _rad_, right?
```

### 14. Open `src/\_includes/layouts/home.html` and replace the existing `<article>` element with the following codes:

```
<article class="intro">
  <div class="[ intro__header ] [ radius frame ]">
    <h1 class="[ intro__heading ] [ weight-normal text-400 md:text-600 ]">
      {{ intro.eyebrow }}
      <em class="text-800 md:text-900 lg:text-major weight-bold">{{ intro.main }}</em>
    </h1>
  </div>
  <div class="[ intro__content ] [ flow ]">
    <p class="intro__summary">{{ intro.summary }}</p>
    <a href="{{ intro.buttonUrl }}" class="button">{{ intro.buttonText }}</a>
  </div>
  <div class="[ intro__media ] [ radius dot-shadow ]">
    <img
      class="[ intro__image ] [ radius ]"
      src="{{ intro.image }}"
      alt="{{ intro.imageAlt }}"
    />
  </div>
</article>
```

### 15. Open up `.eleventy.js` and on the first line of the function (right below `module.exports = config => {` ) add this line:

```
// Set directories to pass through to the dist folder
config.addPassthroughCopy('./src/images/');
```

### 16. Inside `src/\_includes/layouts/base.html`, delete the line `{% block content %}{% endblock %}`, and add the following:

```
{% include "partials/site-head.html" %}

<main tabindex="-1" id="main-content">{% block content %}{% endblock %}</main>
```

### 17. Run the following command in the terminal inside the project folder:

```
mkdir src/_includes/partials
```

### 18. Inside the _partials_ folder, create a new file named `site-head.html`, and add the following:

```
<a class="[ skip-link ] [ button ]" href="#main-content">Skip to content</a>
<header role="banner" class="site-head">
    <div class="wrapper">
    <div class="site-head__inner">
        <a href="/" aria-label="Issue 33 - home" class="site-head__brand">
        {% include "partials/brand.svg" %}
        </a>
        <nav class="[ nav ] [ site-head__nav ] [ font-sans ]" aria-label="Primary">
        <ul class="nav__list">
            <li>
            <a href="/">Home</a>
            </li>
            <li>
            <a href="/work">Work</a>
            </li>
        </ul>
        </nav>
    </div>
    </div>
</header>
```

### 19. In _partials_ folder, create a new file called `brand.svg` and add the following:

```
<svg aria-hidden="true" focusable="false" width="163" height="49" viewBox="0 0 163 49" xmlns="http://www.w3.org/2000/svg">
  <g fill-rule="nonzero" fill="none">
    <path d="M3.296 6.739c.914 0 1.691-.328 2.333-.984.642-.657.963-1.452.963-2.386 0-.933-.32-1.728-.963-2.385A3.143 3.143 0 003.296 0C2.383 0 1.605.328.963.984.32 1.641 0 2.436 0 3.37c0 .934.321 1.73.963 2.386.642.656 1.42.984 2.333.984zm3 21.882V8.594h-6v20.027h6zM17.74 29c1.259 0 2.431-.17 3.518-.511 1.086-.34 2.018-.814 2.796-1.42a6.737 6.737 0 001.833-2.158 5.623 5.623 0 00.667-2.688c0-1.565-.525-2.858-1.574-3.88-1.05-1.023-2.537-1.672-4.463-1.95l-3.889-.606c-.839-.126-1.432-.309-1.777-.549-.346-.24-.519-.561-.519-.965 0-.43.235-.776.704-1.041.47-.265 1.086-.398 1.852-.398.963 0 1.926.145 2.888.436.963.29 1.976.75 3.037 1.381l2.852-3.937a14.99 14.99 0 00-3.944-1.798 15.003 15.003 0 00-4.24-.625c-2.618 0-4.68.58-6.185 1.742-1.507 1.16-2.26 2.75-2.26 4.77 0 1.64.519 2.972 1.556 3.994 1.037 1.022 2.518 1.685 4.444 1.988l3.889.605c.69.101 1.197.272 1.518.511.321.24.481.55.481.928 0 .48-.29.864-.87 1.155-.58.29-1.364.435-2.352.435-.987 0-1.987-.17-3-.511-1.012-.34-2.135-.89-3.37-1.647l-2.851 4.127c1.136.858 2.5 1.508 4.092 1.95 1.593.441 3.315.662 5.167.662zm18.85 0c1.26 0 2.432-.17 3.518-.511 1.087-.34 2.019-.814 2.796-1.42a6.737 6.737 0 001.834-2.158 5.623 5.623 0 00.666-2.688c0-1.565-.524-2.858-1.574-3.88-1.049-1.023-2.537-1.672-4.462-1.95l-3.889-.606c-.84-.126-1.432-.309-1.778-.549-.345-.24-.518-.561-.518-.965 0-.43.234-.776.704-1.041.469-.265 1.086-.398 1.851-.398.963 0 1.926.145 2.889.436.963.29 1.975.75 3.037 1.381l2.851-3.937a14.99 14.99 0 00-3.944-1.798 15.003 15.003 0 00-4.24-.625c-2.617 0-4.679.58-6.185 1.742-1.506 1.16-2.259 2.75-2.259 4.77 0 1.64.518 2.972 1.555 3.994 1.037 1.022 2.519 1.685 4.445 1.988l3.888.605c.691.101 1.198.272 1.519.511.32.24.481.55.481.928 0 .48-.29.864-.87 1.155-.58.29-1.364.435-2.352.435-.988 0-1.987-.17-3-.511-1.012-.34-2.135-.89-3.37-1.647l-2.852 4.127c1.136.858 2.5 1.508 4.093 1.95 1.592.441 3.314.662 5.166.662zm18.777 0c.987 0 1.932-.151 2.833-.454a8.172 8.172 0 002.425-1.288v1.363h6V8.594h-6v13.554a4.016 4.016 0 01-1.48 1.173c-.593.278-1.272.417-2.038.417-1.061 0-1.92-.335-2.574-1.004-.654-.669-.981-1.546-.981-2.63V8.593h-6v12.342c0 2.347.729 4.278 2.185 5.792C51.194 28.243 53.07 29 55.367 29zm24.739 0c1.63 0 3.092-.24 4.388-.72 1.296-.479 2.562-1.249 3.796-2.309l-3.962-3.596a4.86 4.86 0 01-1.704 1.117c-.667.265-1.407.397-2.222.397-1.136 0-2.123-.297-2.963-.89a5.27 5.27 0 01-1.852-2.29H89.55v-1.514c0-1.565-.247-3.023-.74-4.373-.494-1.35-1.18-2.505-2.056-3.464a9.635 9.635 0 00-3.13-2.272c-1.209-.555-2.53-.833-3.962-.833-1.432 0-2.771.265-4.018.795a9.895 9.895 0 00-3.24 2.196 10.398 10.398 0 00-2.167 3.313 10.479 10.479 0 00-.796 4.07c0 1.438.277 2.795.833 4.07a10.435 10.435 0 002.259 3.312c.95.934 2.08 1.666 3.389 2.196a11.04 11.04 0 004.185.795zm3.555-12.57h-8.185c.297-1.009.803-1.791 1.519-2.346.716-.556 1.567-.833 2.555-.833.963 0 1.809.29 2.537.87.728.58 1.253 1.35 1.574 2.31z" fill="#263147"/>
    <path d="M109.363 49c2.475 0 4.763-.308 6.865-.924 2.102-.617 3.909-1.472 5.421-2.565 1.513-1.093 2.701-2.395 3.566-3.906.864-1.511 1.296-3.161 1.296-4.95 0-2.068-.619-3.897-1.856-5.487-1.238-1.59-2.917-2.803-5.039-3.638 1.808-1.034 3.222-2.396 4.243-4.085 1.022-1.69 1.532-3.549 1.532-5.577 0-1.75-.392-3.36-1.178-4.83a11.35 11.35 0 00-3.241-3.758c-1.375-1.034-3.045-1.839-5.01-2.415-1.964-.577-4.105-.865-6.423-.865-3.142 0-6.207.646-9.192 1.938-2.986 1.292-5.48 3.072-7.484 5.338l6.482 5.904c1.728-1.789 3.31-3.031 4.744-3.727 1.433-.696 3.133-1.044 5.097-1.044 1.925 0 3.506.428 4.744 1.282 1.237.855 1.856 1.959 1.856 3.31 0 1.511-.629 2.754-1.886 3.728-1.257.974-2.868 1.461-4.832 1.461h-2.887v7.693h3.889c2.121 0 3.781.358 4.98 1.074 1.198.716 1.797 1.69 1.797 2.922 0 1.471-.698 2.644-2.092 3.52-1.395.874-3.232 1.311-5.51 1.311-2.24 0-4.115-.318-5.628-.954-1.512-.636-3.054-1.79-4.626-3.46l-6.423 5.965c1.925 2.107 4.37 3.757 7.337 4.95 2.966 1.193 6.118 1.789 9.458 1.789zm36.489 0c2.475 0 4.763-.308 6.865-.924 2.102-.617 3.909-1.472 5.421-2.565 1.513-1.093 2.701-2.395 3.566-3.906.864-1.511 1.296-3.161 1.296-4.95 0-2.068-.619-3.897-1.856-5.487-1.238-1.59-2.917-2.803-5.039-3.638 1.807-1.034 3.222-2.396 4.243-4.085 1.022-1.69 1.532-3.549 1.532-5.577 0-1.75-.393-3.36-1.178-4.83a11.35 11.35 0 00-3.241-3.758c-1.375-1.034-3.045-1.839-5.01-2.415-1.964-.577-4.105-.865-6.423-.865-3.142 0-6.207.646-9.193 1.938-2.985 1.292-5.48 3.072-7.484 5.338l6.483 5.904c1.728-1.789 3.31-3.031 4.743-3.727 1.434-.696 3.134-1.044 5.098-1.044 1.925 0 3.506.428 4.744 1.282 1.237.855 1.856 1.959 1.856 3.31 0 1.511-.629 2.754-1.886 3.728-1.257.974-2.868 1.461-4.832 1.461h-2.888v7.693h3.89c2.121 0 3.781.358 4.98 1.074 1.198.716 1.797 1.69 1.797 2.922 0 1.471-.698 2.644-2.092 3.52-1.395.874-3.232 1.311-5.51 1.311-2.24 0-4.115-.318-5.628-.954-1.512-.636-3.054-1.79-4.626-3.46l-6.423 5.965c1.925 2.107 4.37 3.757 7.336 4.95 2.967 1.193 6.12 1.789 9.459 1.789z" fill="#513AA6"/>
  </g>
</svg>
```

### 20. Run this command in the terminal:

```
mkdir src/_data
```

### 21. Inside `\_data` folder, create a new file named `site.json`, and add the following code:

```
{
    "name": "Issue 33",
    "url": "https://issue33.com"
}
```

### 22. Open up _src/\_includes/partials/site-head.html_. Find the line `<a href="/" aria-label="Issue 33 - home" class="site-head__brand">` and replace it with this:

```
<a href="/" aria-label="{{ site.name }} - home" class="site-head__brand">
```

### 23. In your `\_data` folder, create a new file called `navigation.json` and add the following:

```
{
    "items": [
        {
            "text": "Home",
            "url": "/"
        },
        {
            "text": "About",
            "url": "/about-us/"
        },
        {
            "text": "Work",
            "url": "/work/"
        },
        {
            "text": "Blog",
            "url": "/blog/"
        },
        {
            "text": "Contact",
            "url": "/contact/"
        }
    ]
}
```

### 24. Open up _src/\_includes/partials/site-head.html_. Delete the `<ul class="nav__list">` element, with all of its children, and add the following:

```
<ul class="nav__list">
    {% for item in navigation.items %}
    <li>
        <a href="{{ item.url }}">{{ item.text }}</a>
    </li>
    {% endfor %}
</ul>
```

### 25. In `\_data` folder, create a new file called `helpers.js` and add the following:

```
module.exports = {
  /**
   * Returns back some attributes based on whether the
   * link is active or a parent of an active item
   *
   * @param {String} itemUrl The link in question
   * @param {String} pageUrl The page context
   * @returns {String} The attributes or empty
   */
  getLinkActiveState(itemUrl, pageUrl) {
    let response = '';

    if (itemUrl === pageUrl) {
      response = ' aria-current="page"';
    }

    if (itemUrl.length > 1 && pageUrl.indexOf(itemUrl) === 0) {
      response += ' data-state="active"';
    }

    return response;
  }
};
```

### 26. Open up _site-head.html_, delete the `<ul class="nav__list">` element and all of its children, and replace it with the following:

```
<ul class="nav__list">
  {% for item in navigation.items %}
  <li>
    <a href="{{ item.url }}" {{ helpers.getLinkActiveState(item.url, page.url) | safe }}
      >{{ item.text }}</a
    >
  </li>
  {% endfor %}
</ul>
```

### 27. Create a new file in your `\_data` folder called `cta.json` and add the following:

```
{
    "title": "Get in touch if we seem like a good fit",
    "summary": "Vestibulum id ligula porta felis euismod semper. Praesent commodo cursus magna, vel scelerisque nisl consectetur et. Cras justo odio, dapibus ac facilisis in, egestas eget quam. Donec ullamcorper nulla non metus auctor fringilla.",
    "buttonText": "Start a new project",
    "buttonUrl": "/contact/"
}
```

### 28. In your _partials_ folder, create a new file called `cta.html` and add the following:

```
{% set ctaPrefix = cta %}

{% if ctaContent %}
  {% set ctaPrefix = ctaContent %}
{% endif %}

<article class="[ cta ] [ dot-shadow panel ] [ bg-dark-shade color-light ]">
    <div class="wrapper">
        <div class="[ cta__inner ] [ flow ]">
            <h2 class="[ cta__heading ] [ headline ]" data-highlight="quaternary">{{ ctaPrefix.title }}</h2>
            <p class="[ cta__summary ] [ measure-short ]">{{ ctaPrefix.summary }}</p>
            <div class="cta__action">
                <a class="button" data-variant="ghost" href="{{ ctaPrefix.buttonUrl }}">{{ ctaPrefix.buttonText }}</a>
            </div>
        </div>
    </div>
</article>
```

### 29. Open up `src/\_includes/layouts/home.html` and after the `</article>`, add the following:

```
{% set ctaContent = primaryCTA %}
{% include "partials/cta.html" %}

{% set ctaContent = cta %}
{% include "partials/cta.html" %}
```

### 30. Open `src/index.md` and at the _bottom_ of the Front Matter (the last _---_ bit), add the following:

```
primaryCTA:
    title: 'This is an agency that doesn’t actually exist'
    summary: 'This is the project site you build when you take the “Learn Eleventy From Scratch” course so it is all made up as a pretend context. You will learn a lot about Eleventy by building this site though. Take the course today!'
    buttonText: 'Buy a copy'
    buttonUrl: 'https://piccalil.li/course/learn-eleventy-from-scratch/'
```

### 31.
