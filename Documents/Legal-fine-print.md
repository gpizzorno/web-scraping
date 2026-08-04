# The legal fine print

Before scraping anything, it's worth pausing on what you're allowed to take, and how you should go about taking it. These are things we'll flag here and then bring back up when they become relevant during the workshop.

## Copyright, licenses, and terms of use

Just because data is visible on a page doesn't mean it's free to copy and reuse. Check the site's terms of use and the license attached to the underlying content before you build anything around it.

> [!TIP]
> Not sure whether something is covered by copyright, or what you're allowed to do with it? Harvard Library's [Copyright First Responders](https://library.harvard.edu/services-tools/copyright-first-responders) program can help.

## Anonymity, countermeasures, and workarounds

- **Throttling and IP blocking**: sites can and do rate-limit or block traffic that looks automated. **Be nice**: space out your requests, and don't hammer a server harder than a human user would.
- **Proxies, VPNs, and overlay networks**: these exist to route around blocks, but they don't change whether scraping a given site is appropriate; they're a technical workaround, not a legal one.

## Scraping is a last resort

Before writing a scraper, check whether there's an easier and more sanctioned way to get the data:

1. **Downloading**: if the data is offered as a direct download (CSV, JSON dump, etc.), just use that.
2. **APIs**: many sites expose a purpose-built API for programmatic access (see [Using APIs](Using-APIs.md)). This is almost always more stable and more welcome than scraping the same data out of the rendered page.
3. **Ask nicely**: institutions and site owners will sometimes just give you what you need if you ask.

Scraping is what's left once those options are unavailable.

## Exploration is essential

Before you write any code (or build any Web Scraper sitemap):

- **Know what's there.** Explore the site the way you would explore a source before an exam—what pages exist, what data repeats across them, where the structure is (and isn't) consistent.
- **Know what you want to get.** A clear target makes it much easier to spot the right elements/selectors once you start building.
- **Weigh scraping effort against cleanup effort.** A slightly messier scrape that's fast to build and easy to clean up afterward is often a better trade than a highly precise scraper that takes much longer to write.
