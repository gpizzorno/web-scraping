# Inspecting websites

Before you can target an element with a [selector](Selectors.md), you need to find out what that element actually looks like in the page's HTML. That's what your browser's built-in inspector is for.

> [!TIP]
> Open the inspector in Chrome with **Option (⌥) + Command (⌘) + I** on macOS, or **F12** / **Ctrl+Shift+I** on Windows. You can also right-click any element on the page and choose **Inspect**.

## The Elements panel

The Elements panel shows you the live HTML of the page, structured as a tree—the same nested structure covered in [The structure of a web page](Structure-of-a-web-page.md). As you hover over a line in the panel, the browser highlights the corresponding element on the page, and vice versa.

This is the fastest way to answer the question every scraper starts with: *what tag, class, or id is this piece of data actually wrapped in?*

## Point-and-click selection

Rather than hunting through the tree by hand, click the cursor/arrow icon in the top-left of the Elements panel (or right-click → **Inspect**), then click directly on the piece of content you want. The panel jumps straight to the matching HTML, with the tag, classes, and attributes visible.

This is exactly the same idea the [Web Scraper](Web-Scraper.md) extension uses for building selectors—you point at the data on the rendered page, and it works out the underlying selector for you.

## Sources: finding the actual files

The **Sources** (or **Network**) panel shows you every file the page has loaded—HTML, CSS, JavaScript, images, and any data files (JSON, XML) fetched in the background. This matters for scraping because:

- Data that looks like it's "on the page" is sometimes actually loaded separately as JSON/XML by JavaScript after the page loads. If so, it's often much easier to request that file directly than to scrape the rendered HTML.
- It's how you can confirm whether a site is serving data through an API you could call directly instead of scraping (see [Using APIs](Using-APIs.md)).

## Workflow

A good habit before writing any scraping code: open the inspector, click around on the data you want, and note down the tags/classes/ids involved *before* touching a keyboard for code. Confirm the pattern holds across a few different examples on the page—most scraping bugs come from a pattern that looked consistent but wasn't.
