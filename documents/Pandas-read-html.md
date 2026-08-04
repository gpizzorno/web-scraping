# Shortcut: using pandas `read_html()`

Before reaching for `requests` + BeautifulSoup, it's worth checking whether the data you want is already sitting in an HTML `<table>` element. If it is, [pandas](https://pandas.pydata.org) has a built-in shortcut that skips HTML parsing entirely.

## What it does

`pandas.read_html()` takes a URL (or raw HTML), finds every `<table>` element on the page, and converts each one directly into a data frame. You get back a **list** of data frames—one per table found—even if the page only has one.

```python
import pandas as pd

nfl = pd.read_html("https://www.nfl.com/stats/team-stats/offense/passing/2023/reg/all")
nfl
```

Since it returns a list, grab the table you want with square brackets:

```python
teams_2023 = nfl[0]
teams_2023.head()
```

## Looping over multiple pages

This shortcut is most useful when a site's URL structure is simple and predictable—[the same pattern covered in Batch downloading](Batch-downloading.md), just applied to tables instead of files. If only one part of the URL changes (here, the year), you can loop over it, tag each result, and collect everything as you go:

```python
nfl = []
for year in range(2019, 2024):
    url = "https://www.nfl.com/stats/team-stats/offense/passing/" + str(year) + "/reg/all"

    # read_html returns a list of data frames
    stats = pd.read_html(url)

    # extract the table we want from the list
    stats = stats[0]

    # tag each row with the year it came from, before we lose that information
    stats["year"] = year

    nfl.append(stats)
```

Once you have a list of same-shaped data frames, combine them into one with `pd.concat()`:

```python
nfl_combined = pd.concat(nfl, ignore_index=True)
nfl_combined
```

From there, it's a normal pandas data frame—clean it up and analyze it like any other.

## When this isn't enough

`read_html()` only helps when the data is already inside a `<table>` tag. Plenty of pages present tabular-*looking* data using `<div>`s, lists, or other structures, or the data you want isn't tabular at all—a list of names, a set of image URLs, nested article metadata. In those cases, you need to walk the actual page structure yourself, which is where [BeautifulSoup](BeautifulSoup.md) comes in.

## Further reading

- [`pandas.read_html()` documentation](https://pandas.pydata.org/docs/reference/api/pandas.read_html.html)
