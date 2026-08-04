# Batch downloading

Sometimes you don't need a scraper at all—you just need to download a large number of *files* (images, PDFs, pages) that a site already serves directly. When the files live at predictable URLs, you can generate the full list of URLs yourself and download them in one batch, no HTML parsing required.

## The structure of URLs

Every URL is made of the same three parts, and batch downloading is really about exploiting patterns in one of them:

- **Domain** — the site itself, *e.g.*, `pds.lib.harvard.edu`
- **Path** — remember, this is usually just a file location on the server, *e.g.*, `/pds/viewtext`
- **Querystring** — the part after `?`, used to pass data, filters, instructions, or pagination, *e.g.*, `?op=t&n=181`

## Exploration

Before generating anything, look for the pattern:

- **Sitemap and URL structure**: browse a handful of the pages/files you want and compare their URLs. Is there a piece (a page number, an ID, a date) that changes predictably while everything else stays the same?
- **Look at the page code**: use your browser's [inspector](Inspecting-websites.md) to confirm how links to the underlying files are actually constructed, rather than guessing from what's displayed.

## Generating a list of targets

Once you know which part of the URL varies, you can generate every URL you need without visiting each page by hand. A spreadsheet is often the fastest way to do this: put the varying values (page numbers, IDs, etc.) in one column, then use string concatenation to build the full URL in the next column—*e.g.*, in Excel/Google Sheets: `="http://example.com/page/"&A1`.

### Worked example

Harvard's Page Delivery Service (PDS) serves individual pages of a digitized item at URLs like:

```
http://pds.lib.harvard.edu/pds/viewtext/6796688?op=t&n=181
```

Here, `6796688` identifies the item, and `181` is the **sequence** (i.e. the internal page number)—everything else in the URL stays fixed. For an item with 1480 sequences, you can generate all 1480 URLs by counting from 1 to 1480 and substituting each value for `181`.

Turn that list of URLs into a batch-downloading script by prefixing each one with `curl -O` (`-O` tells curl to save the file locally using its remote name):

```bash
curl -O 'http://pds.lib.harvard.edu/pds/viewtext/6796688?op=t&n=1'
curl -O 'http://pds.lib.harvard.edu/pds/viewtext/6796688?op=t&n=2'
curl -O 'http://pds.lib.harvard.edu/pds/viewtext/6796688?op=t&n=3'
...
curl -O 'http://pds.lib.harvard.edu/pds/viewtext/6796688?op=t&n=1480'
```

Save that as a file (*e.g.*, `curlscript`), make it executable, and run it:

```bash
chmod 755 curlscript
./curlscript
```

## Batch downloading tools

- [Wget](https://www.gnu.org/software/wget/): can also take a text file of URLs directly (`wget -i urls.txt`), so you don't need to build a full script of individual commands.
- [cURL](https://curl.se): used above; good for one-off scripts and when you need more control per-request (headers, cookies, etc.)

### Another example: IIIF image URLs

Harvard's image delivery service follows the same predictable-URL pattern. A manifest page like:

```
https://iiif.lib.harvard.edu/manifests/view/drs:6796688$9i
```

lists out individual image URLs such as:

```
https://ids.lib.harvard.edu/ids/iiif/6793740/full/1200,/0/default.jpg
https://ids.lib.harvard.edu/ids/iiif/6793742/full/1200,/0/default.jpg
https://ids.lib.harvard.edu/ids/iiif/6793744/full/1200,/0/default.jpg
```

Only the image ID (`6793740`, `6793742`, ...) changes between them—the same generate-then-batch-download approach applies.
