# Using selectors to target data

Once you know a page is built out of nested HTML elements (see [The structure of a web page](Structure-of-a-web-page.md)), the next question is: how do you point at the *one* element you actually want, out of everything else on the page? That's what a **selector** does—it's a pattern that targets specific tags, so you can pull out (or style) just those.

Selectors were originally built for CSS, so that a stylesheet could say "make every `<h1>` blue" without hand-editing every heading. Scrapers reuse that same targeting language to say "grab the text out of every element that looks like this" instead.

## Dotted notation

The shorthand `.class` / `#id` notation comes directly from CSS:

- `#id` targets the one element with that `id` attribute (ids are supposed to be unique on a page)
- `.class` targets every element with that class in its `class` attribute (classes are meant to be reused across many elements)

So for `<div id="header" class="site-nav">`, the selectors `#header` and `.site-nav` both match this element.

## Basic selector types

| Selector | Matches | Example |
|---|---|---|
| Tag name | every element of that tag | `p` matches every `<p>` |
| `.class` | every element with that class | `.price` matches every `<span class="price">` |
| `#id` | the element with that id | `#main-title` matches `<h1 id="main-title">` |
| `tag.class` | elements of that tag *and* class | `p.price` matches `<p class="price">` but not `<span class="price">` |
| `parent child` | descendants of a matched element | `table td` matches every `<td>` inside a `<table>` |
| `[attribute=value]` | elements with a matching attribute | `[href="/about"]` matches any tag with that exact `href` |

## Pseudo-selectors

Pseudo-selectors target elements based on their *position* or *state* rather than their tag/class/id—useful for things like "the first row of a table" or "every other item in a list":

- `:first-child` / `:last-child`
- `:nth-child(n)`
- `:hover` (rarely relevant for scraping, since it's a live browser state)

## Trying it out

You don't need a scraper to practice selectors—any of these sandboxes let you write HTML/CSS and see immediately what a selector matches:

- [JSFiddle](https://jsfiddle.net)
- [PlayCode](https://playcode.io)
- [CodePen](https://codepen.io)

## Why this matters for scraping

Every scraping tool covered in this workshop—the [Web Scraper](Web-Scraper.md) extension on Day 1, and [BeautifulSoup](BeautifulSoup.md) in Python on Day 2—is fundamentally doing the same thing: you give it a selector, and it hands you back the matching element(s). The syntax differs slightly between tools (BeautifulSoup, for instance, mixes CSS-style selectors with its own `find()`/`find_all()` methods), but the underlying concept—"target elements by tag, class, id, or position"—is the same one covered here.

> [!TIP]
> Full selector reference: [W3Schools CSS Selectors](https://www.w3schools.com/cssref/css_selectors.php)
