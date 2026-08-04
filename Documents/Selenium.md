# DOM Parsing with Selenium

`requests` only gets you the HTML a server sends back on the initial load. Plenty of sites build (or update) their content afterward, in the browser, with JavaScript—search results that populate after you type, "load more" buttons, login-gated pages. `requests` never sees any of that, because it never runs the JavaScript.

[Selenium](https://www.selenium.dev) solves this by driving an actual browser: it opens a real (or headless) Chrome window, lets the page's JavaScript run normally, and lets you interact with it—click, type, scroll, wait—exactly like a person would. Once the content you want has actually rendered, you can read the live DOM (`driver.page_source`) and parse it the normal way with BeautifulSoup or `lxml`.

> [!TIP]
> Setup instructions for Selenium and ChromeDriver are in [Setup instructions](Setup-instructions.md). ChromeDriver's version has to match your installed Chrome version.

## Basic workflow

```python
from selenium import webdriver

driver_path = 'chromedriver'  # or the full path to your chromedriver install
driver = webdriver.Chrome(driver_path)

driver.implicitly_wait(10)  # seconds to wait for elements to appear before giving up
driver.get('https://example.com')
```

From there, find elements the same way you would in the browser's [inspector](Inspecting-websites.md)—by id, name, CSS selector, or XPath—and interact with them:

```python
driver.find_element_by_name('search').click()
driver.find_element_by_id('query').send_keys('Iron Age')
```

## Reading the rendered page

Once the page has done whatever it needed to do, grab its current HTML and parse it like normal:

```python
from bs4 import BeautifulSoup

page_source = driver.page_source
soup = BeautifulSoup(page_source, 'html.parser')
```

(Some notebooks in this repo use `lxml.etree` instead of BeautifulSoup for this step—the same idea applies either way: take `driver.page_source`, hand it to a parser, and extract data as covered in [HTML Parsing with BeautifulSoup](BeautifulSoup.md).)

## Looping over search criteria

A common pattern: fill in a search form, loop over every combination of filters you care about, and collect results for each:

```python
for period in periods:
    period_input = driver.find_element_by_id('advancedSearchForm:temporal')
    period_input.clear()
    period_input.send_keys(period)

    for county in counties:
        # ... perform the search, then extract/save results for this combination
        pass
```

## Following pagination

Where [Web Scraper](Web-Scraper.md) follows a "next page" Link selector automatically, in Selenium you do that step yourself: click the "next" control, re-read `page_source`, and repeat until there's no next page left:

```python
is_last = False
while not is_last:
    soup = BeautifulSoup(driver.page_source, 'html.parser')
    # ... extract this page's data from soup ...

    try:
        driver.find_element_by_xpath('//a[@title="Go forward 20 records"]').click()
    except Exception:
        is_last = True
```

## Waiting for content to load

`driver.implicitly_wait(10)` above is a blanket wait applied to every element lookup. For pages where a specific element takes a while to appear (or you want to wait for a specific condition rather than a fixed timeout), use an explicit `WebDriverWait` instead:

```python
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By

wait = WebDriverWait(driver, 30)
wait.until(EC.presence_of_element_located((By.CSS_SELECTOR, 'button[aria-label="Go to page 2"]')))
```

## Combining Selenium with requests

Selenium is slower than `requests`, since it's driving a full browser. A useful hybrid for sites that need a login: use Selenium *only* to log in, then hand its session cookies off to `requests` for the actual (much faster) scraping:

```python
import requests

s = requests.session()

# copy the authenticated session's cookies from Selenium into requests
for cookie in driver.get_cookies():
    s.cookies.update({cookie['name']: cookie['value']})

# match the user agent so the server sees a consistent client
user_agent = driver.execute_script("return navigator.userAgent;")
s.headers.update({"User-Agent": user_agent, "Referer": driver.current_url})

# now s.get(...) requests pages as the logged-in user, without needing Selenium for each one
response = s.get("https://example.com/some-authenticated-page")
```

## Closing the browser

Don't forget to close the driver when you're done—otherwise the browser window (and its process) just sits there:

```python
driver.close()
```
