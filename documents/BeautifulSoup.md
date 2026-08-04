# HTML Parsing with BeautifulSoup

Not every piece of data you want lives inside a `<table>`—so [the pandas `read_html()` shortcut](Pandas-read-html.md) won't always get you there. When the data has *some* structure (thanks to HTML) but isn't in table form, you need to parse the page yourself. That's what [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) (`bs4`) is for.

Scraping with BeautifulSoup happens in two steps:

1. **Download** the page's HTML with `requests`.
2. **Parse** that HTML with BeautifulSoup, so you can navigate and search it like the tree structure it actually is (see [The structure of a web page](Structure-of-a-web-page.md)).

The examples below use [Books to Scrape](http://books.toscrape.com/index.html), a site built specifically for practicing scraping.

## Downloading a page

```python
import requests
from bs4 import BeautifulSoup

url = "http://books.toscrape.com/index.html"

# Download the HTML
page = requests.get(url)
page.text  # the raw HTML, as text
```

## Parsing the page

```python
soup = BeautifulSoup(page.text, "html.parser")
print(soup.prettify()[:750])  # first 750 characters, nicely indented
```

`soup` now behaves like a navigable tree of the page—the same nested tag structure you'd see in the browser's [inspector](Inspecting-websites.md).

## Finding a single element

Say each book on the page is wrapped in an `<article>` tag. `find()` returns the *first* match:

```python
book = soup.find("article")
```

From there, keep drilling in. Elements can have classes (an element can have more than one, separated by spaces), and `.get()` retrieves an attribute's value:

```python
book.find("p", class_="star-rating").get("class")   # ['star-rating', 'Three']
book.find("p", class_="star-rating").get("class")[1]  # 'Three'
```

Putting a few fields together for one book:

```python
title = book.find("h3").find("a").get("title")
rating = book.find("p", class_="star-rating").get("class")[1]

# .text extracts the text from an element; .strip() removes leading/trailing whitespace
price = book.find("p", class_="price_color").text.strip()
availability = book.find("p", class_="availability").text.strip()

url = book.find("h3").find("a").get("href")
```

## Finding every match

`find_all()` (or `findAll()`) returns every matching element instead of just the first, so you can loop over them:

```python
books = soup.find_all("article")

titles, ratings, prices, availabilities, urls = [], [], [], [], []

for book in books:
    titles.append(book.find("h3").find("a").get("title"))
    ratings.append(book.find("p", class_="star-rating").get("class")[1])
    prices.append(book.find("p", class_="price_color").text.strip())
    availabilities.append(book.find("p", class_="availability").text.strip())
    urls.append("http://books.toscrape.com/" + book.find("h3").find("a").get("href"))
```

From here, it's a short step to a pandas data frame:

```python
import pandas as pd

book_info = pd.DataFrame({
    "title": titles,
    "rating": ratings,
    "price": prices,
    "availability": availabilities,
    "url": urls,
})
```

## Turning it into a reusable scraper

Wrap the extraction logic in a function so it can run on any page with the same layout:

```python
def book_scraper(page_url):
    page = requests.get(page_url)
    soup = BeautifulSoup(page.text, "html.parser")
    books = soup.find_all("article")

    titles, ratings, prices, availabilities, urls = [], [], [], [], []
    for book in books:
        titles.append(book.find("h3").find("a").get("title"))
        ratings.append(book.find("p", class_="star-rating").get("class")[1])
        prices.append(book.find("p", class_="price_color").text.strip())
        availabilities.append(book.find("p", class_="availability").text.strip())
        urls.append("http://books.toscrape.com/" + book.find("h3").find("a").get("href"))

    return pd.DataFrame({
        "title": titles, "rating": ratings, "price": prices,
        "availability": availabilities, "url": urls,
    })
```

### Crawling an entire site

Books to Scrape's catalog pages follow a predictable pattern after the first page—`http://books.toscrape.com/catalogue/page-{PAGE-NUMBER}.html`—with 50 pages total. That's the same "spot the pattern, generate the list of targets" idea from [Batch downloading](Batch-downloading.md), just applied to pages instead of files:

```python
catalog_urls = ["http://books.toscrape.com/index.html"]
for i in range(2, 51):
    catalog_urls.append("http://books.toscrape.com/catalogue/page-" + str(i) + ".html")

every_book = [book_scraper(url) for url in catalog_urls]

df_final = pd.concat(every_book, ignore_index=True)
```

That gives you a single data frame with all ~1000 books on the site.

## Quick reference

Once you have a `soup` (or any tag pulled from one), these are the methods you'll reach for most:

**Tags**
```python
tag = soup.body
type(tag)
tag.name
```

**Attributes**
```python
tag = soup.body.div
tag['id']
tag['class']
tag.attrs        # all attributes, as a dict
```

**Navigation**
```python
tag = soup.body.div.div
tag.contents            # direct children, as a list
tag.contents[0]
tag.children             # direct children, as an iterator
tag.descendants          # all nested children, recursively

tag.strings               # every string of text under this tag
tag.stripped_strings      # same, with whitespace stripped

tag.parent
tag.parents

tag.next_sibling, tag.previous_sibling
tag.next_siblings, tag.previous_siblings
tag.next_element, tag.previous_element
tag.next_elements, tag.previous_elements
```

**Search**
```python
soup.find(...)              # first match
soup.find_all(...)          # every match

soup.find_all(["a", "b"])              # match any of several tags
soup.find_all("a", class_="sister")    # match by tag + class
```

## Further reading

- [BeautifulSoup documentation](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
- [More in-depth tutorial](https://towardsdatascience.com/an-introduction-to-web-scraping-with-python-a2601e8619e5)
- [Discussion on the legality/ethics of scraping](https://benbernardblog.com/web-scraping-and-crawling-are-perfectly-legal-right/)—see also [The legal fine print](Legal-fine-print.md)
