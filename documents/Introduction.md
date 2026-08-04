# Introduction

## What is scraping?

**Web scraping** (also called *web harvesting* or *web data extraction*) means automating the extraction of large amounts of data from websites into a structured format:

- **automate**: avoid the tedious, error-prone manual approach; make it repeatable
- **large amounts**: i.e. more than you could reasonably cut and paste by hand
- **structured format**: ready, or close to ready, for analysis (a data structure, not a wall of text)

You do this by building a **scraper**: a piece of software designed to grab whatever data you want.

## What can you scrape?

In principle, anything. Generally speaking, if you can see it on your screen, you can scrape it.

In practice: more complicated. Scraping tasks range from the very simple—two clicks—to the hugely complex: hours of writing code in Python.

## A note on workflow

Before you start writing code—don't. For a number of reasons, debugging browser automation is kind of painful:

- It takes a long time.
- There's a lot that can go wrong, and if it turns out to be a misspelled selector, you'll be mad at yourself.
- If you run a script over and over on the same website, you risk the site detecting that behavior as a bot (it kind of is) and blocking you.

So before touching a keyboard for code, start with pseudo-code: get a clear idea of the steps in your head, open the site logged out with a clean cache/cookies, and walk through the process by hand—clicking, and finding the selectors as you go—before automating any of it. (This is the same idea covered under [Inspecting websites](Inspecting-websites.md): explore first, script second.)

Keep in mind the balance of effort involved, too—scraping vs. cleaning up what you scraped. A slightly messier scrape that's fast to build is often a better trade than a highly precise one that takes far longer to write. See [The legal fine print](Legal-fine-print.md) for more on this, along with what you're allowed to scrape in the first place.

## Plan for workshop

- The basic workflow is the same regardless of tool—only the tools themselves change.
- **Day 1:** the fundamentals, using a point-and-click approach ([Web Scraper](Web-Scraper.md)).
- **Day 2:** the same ideas, applied programmatically in Python.

We'll flag a number of things along the way (legal considerations, selectors, workflow) and bring them back up when they become directly relevant.
