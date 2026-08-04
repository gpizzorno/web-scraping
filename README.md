# Web Scraping

<img align="right" src="assets/thumbnail.png" width=300  />

This workshop teaches participants how to automate the extraction of data from websites and other online repositories into a well-formatted, locally stored dataset, for later analysis. Web scraping tools make the process of collecting large amounts of online information more efficient, and help automate an otherwise tedious, time-consuming, and error-prone process.

The workshop opens with an introduction to how web pages are structured and how selectors are used to target data within them, then moves into direct, hands-on experience with a series of scraping techniques that run the gamut from simple to complex: batch downloading, a full no-code workflow using the [Web Scraper](https://webscraper.io/web-scraper-extension) browser extension, and a set of Python techniques—from [pandas](https://pandas.pydata.org)' `read_html()` shortcut, to full HTML parsing with [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/), working with APIs, and DOM parsing with [Selenium](https://www.selenium.dev) for JavaScript-heavy pages.

## Workshop Plan

This is a two-day workshop with three hours of instruction on each day. You can find links to all the tools and resources mentioned during the workshop [here](/documents/Resources.md), and the exercises are listed in [this page](/documents/Exercises.md).

### Day 1

>[!IMPORTANT]
>You'll need a recent version of [Chrome](https://www.google.com/chrome/) and the [Web Scraper](https://webscraper.io) extension.

- #### [Introduction](/documents/Introduction.md)
    - [The structure of a web page](/documents/Structure-of-a-web-page.md)
    - [Using *selectors* to target data](/documents/Selectors.md)
    - [The legal fine print](/documents/Legal-fine-print.md)
- #### Scraping on the Browser
    - [Inspecting websites](/documents/Inspecting-websites.md)
    - [Batch downloading](/documents/Batch-downloading.md)
    - Introduction to [Web Scraper](https://webscraper.io) ([reference](/documents/Web-Scraper.md))
- #### [Designing a scraper](/documents/Web-Scraper.md)
    - [Structure of a scraper](/documents/Web-Scraper.md#structure-of-a-scraper)
    - [Basic selectors](/documents/Web-Scraper.md#basic-selectors): text, link, image, HTML, table
    - [Advanced selectors](/documents/Web-Scraper.md#advanced-selectors): groups, generic elements, scroll, click, pagination

### Day 2

>[!IMPORTANT]
>Please follow the [instructions here](/documents/Setup-instructions.md) to ensure you have the necessary software for day 2.
>

- #### Shortcut: Using pandas [`read_html`](/documents/Pandas-read-html.md)
    - [What it does](/documents/Pandas-read-html.md#what-it-does)
    - [Looping over multiple pages](/documents/Pandas-read-html.md#looping-over-multiple-pages)
    - [When this isn't enough](/documents/Pandas-read-html.md#when-this-isnt-enough)
- #### HTML Parsing with [BeautifulSoup](/documents/BeautifulSoup.md)
    - [Downloading a page](/documents/BeautifulSoup.md#downloading-a-page)
    - [Finding a single element](/documents/BeautifulSoup.md#finding-a-single-element) and [finding every match](/documents/BeautifulSoup.md#finding-every-match)
    - [Turning it into a reusable scraper](/documents/BeautifulSoup.md#turning-it-into-a-reusable-scraper)
- #### Using [APIs](/documents/Using-APIs.md)
    - [Why prefer an API](/documents/Using-APIs.md#why-prefer-an-api)
    - [Worked example: Harvard's LibraryCloud API](/documents/Using-APIs.md#worked-example-harvards-librarycloud-api)
    - [Finding an API](/documents/Using-APIs.md#finding-an-api)
- #### DOM Parsing with [Selenium](/documents/Selenium.md)
    - [Basic workflow](/documents/Selenium.md#basic-workflow)
    - [Following pagination](/documents/Selenium.md#following-pagination)
    - [Combining Selenium with requests](/documents/Selenium.md#combining-selenium-with-requests)

## Repository Structure

- [`documents/`](/documents) — reference material for every topic covered in the workshop, linked throughout this README
- [`examples/`](/examples) — Jupyter notebooks with hands-on, runnable examples, primarily for Day 2
- [`assets/`](/assets) — images embedded in the reference material
- [`images/`](/images), [`output/`](/output) — empty, gitignored scratch folders that some of the example notebooks write their downloaded/generated files into

## License

The project is licensed under the [MIT License](LICENSE), allowing free use, modification, and distribution.
