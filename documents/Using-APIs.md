# Using APIs

Scraping is what you do when a site *doesn't* offer any better way to get its data (see [The legal fine print](Legal-fine-print.md)—"scraping is a last resort"). Before writing a scraper, it's always worth checking whether the site exposes an **API**—a purpose-built way for programs to request its data directly, without touching the rendered page at all.

## Why prefer an API

The same data can often be gotten two ways: by scraping the rendered page, or by calling an API that serves it directly. The API route is almost always better:

- It returns clean, structured data (JSON or XML) instead of HTML you have to parse.
- It's stable—an API contract changes far less often than a page's HTML/CSS layout.
- It's sanctioned—you're using the access the site intended, rather than working around its interface.
- It's usually faster and lighter, for you and for the server.

The tradeoff is that an API only gives you what it was built to expose—if the field you need isn't in the response, you may still need to scrape for it.

## Worked example: Harvard's LibraryCloud API

Harvard Library exposes a JSON/XML API called [LibraryCloud](https://wiki.harvard.edu/confluence/display/LibraryStaffDoc/LibraryCloud+APIs) for querying its collections—the same trade card images that would otherwise require a full [Selenium](Selenium.md)-driven login/scroll/click workflow to collect from the HOLLIS Images website are available here with a single request.

### Making a request

```python
import requests
from xml.etree import ElementTree as ET

limit = '10'
BASE_URL = f"https://api.lib.harvard.edu/v2/items?genre=trade+cards&limit={limit}"

api_call = requests.get(BASE_URL).text
```

### Parsing the response

The API returns XML here (JSON is also available), so we parse it into an `ElementTree` we can search:

```python
root = ET.fromstring(api_call)
```

Namespaces need to be declared to search MODS XML like this:

```python
namespaces = {
    'mods': "http://www.loc.gov/mods/v3",
    'default': "http://api.lib.harvard.edu/v2/item"
}
```

An XPath search finds every URL element whose `displayLabel` attribute is `"Full Image"`—this is how full-sized image URLs are coded in the MODS XML that comes back:

```python
urls = root.findall('.//mods:url[@displayLabel="Full Image"]', namespaces)
```

### Downloading the results

From here, either download directly with `requests`:

```python
for url in urls:
    time.sleep(1)  # so we don't download too fast—see "be nice" in Legal-fine-print.md
    img_data = requests.get(url.text).content
    with open(f'{url.text.rsplit("/", 1)[-1]}.jpg', 'wb') as f:
        f.write(img_data)
```

...or, matching the [Batch downloading](Batch-downloading.md) approach, just collect the links into a text file and hand the actual downloading off to `wget`:

```python
links = [url.text for url in urls]

with open("links.txt", "w") as file:
    file.write("\n".join(links))
```

```bash
wget -i links.txt
```

### Paginating through results

A single request like the one above only returns `limit` results. Most APIs paginate large result sets—LibraryCloud uses a `cursor` parameter that starts at `*` and gets updated from each response, so you keep requesting "the next page" until there isn't one:

```python
limit = '75'  # hits per page, can be up to 250
next_cursor = "*"  # needs to be * to start
BASE_URL = f"https://api.lib.harvard.edu/v2/items?genre=trade+cards&limit={limit}&cursor="

while next_cursor:
    api_call = requests.get(f'{BASE_URL}{next_cursor}').text
    root = ET.fromstring(api_call)
    urls = root.findall('.//mods:url[@displayLabel="Full Image"]', namespaces)

    for url in urls:
        time.sleep(1)
        img_data = requests.get(url.text).content
        # ... save img_data as before
```

## Finding an API

Not every site advertises its API prominently—check for developer documentation, or watch the **Network** panel in your browser's [inspector](Inspecting-websites.md) while the page loads: if data appears as a separate JSON/XML request rather than baked into the HTML, that request is often callable directly.
