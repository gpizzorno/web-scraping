# Using Web Scraper

[Web Scraper](https://webscraper.io) is a free Chrome extension that lets you build a point-and-click scraper without writing any code. It works by recording a set of *selectors* against the live page, then replaying them across every page you want to collect data from.

## Installation

1. Install a recent version of [Chrome](https://www.google.com/chrome/).
2. Install the [Web Scraper extension](https://webscraper.io) from the Chrome Web Store.
3. Open Chrome's developer tools ([the same inspector used to explore a page](Inspecting-websites.md)—**Option (⌥) + Command (⌘) + I** on macOS, **F12** on Windows) and select the new **Web Scraper** tab that appears alongside **Elements**, **Console**, etc.

## Structure of a scraper

In Web Scraper, what you build is called a **sitemap**. A sitemap is made of two things:

1. **A starting point**: the URL (or URLs) the scraper begins from.
2. **A tree of selectors**: each selector targets something on the page, and can have child selectors nested underneath it. Web Scraper runs them in the order they're organized in the tree: a parent selector narrows down to a section of the page (or navigates to a new page), and its children then operate only within whatever the parent matched.

That tree structure is what lets a single sitemap both **navigate** a site (via selectors that follow links) and **extract data** (via selectors that pull out text, images, table rows, etc.) in the same pass.

## Create a sitemap

The first thing you need to do when creating a sitemap is specifying the start url. This is the url from which the scraping will start. You can also specify multiple start urls if the scraping should start from multiple places.

In cases where a site uses numbering in pages URLs it is much simpler to create a range start url than creating Link selectors that would navigate the site. To specify a range url replace the numeric part of start url with a range definition - `[1-100]`.

Use a range url like this `http://example.com/page/[1-3]` for links like these:

```
http://example.com/page/1
http://example.com/page/2
http://example.com/page/3
```

> [!TIP]
> Sitemaps can be exported as JSON blobs, making it easier to share or backup.

## Basic selectors

These are the selectors that actually pull data off the page—each one returns a single piece of data from whatever element it's pointed at:

- **Text selector**: extracts the visible text of an element.
- **Link selector**: extracts the URL from a link's `href`. It doubles as a navigation tool: pointed at a "next page" link and nested as its own child, it will keep following the chain to walk through every page (this is how **pagination** is handled—there's no separate "pagination selector").
- **Image selector**: extracts the `src` URL of an image.
- **HTML selector**: extracts an element's raw inner HTML, when you need to preserve markup/formatting rather than plain text.
- **Table selector**: extracts an entire HTML `<table>` at once, mapping each row/column automatically instead of needing one selector per cell.

## Advanced selectors

These control *how* the scraper moves through and groups content, rather than what data it grabs directly:

- **Grouped selector**: combines multiple different child selectors into a single logical group, *e.g.* when a record's fields (title, date, author) each need their own selector but should be extracted together as one record. This is what "groups" refers to.
- **Element selector**: a general-purpose container selector: point it at a repeating block (*e.g.* one product card, one search result), and it hands off each matching element to its children, so the same set of child selectors run once per item. This is the mechanism behind "generic elements": repeated, structurally-identical blocks on a page.
- **Element scroll down selector**: for infinite-scroll pages: repeatedly scrolls the page to trigger more content loading before extraction runs.
- **Element click selector**: for pages that reveal more content via a button (*e.g.* "Load more"): clicks it, waits, and repeats before extraction runs.

## Scrape the site

After you have created selectors for the sitemap you can start scraping. Open the Scrape panel and start scraping. Optionally, you can change the request interval and page load delay. A new popup window will open in which the scraper will load pages and extract data from them. After the scraping is done, the popup window will close and you will be notified with a popup message. You can view the scraped data by opening the Browse panel, and export it via the Export data as CSV panel.

## Further reading

- [Web Scraper tutorials](https://webscraper.io/tutorials)
- [Web Scraper documentation](https://webscraper.io/documentation)
