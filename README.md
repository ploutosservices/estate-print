# estate-print

A printed web page should hold its content and nothing that only works on a screen. A top bar, a side nav and a
footer of buttons cannot be clicked on paper. estate-print leaves them out, and fills each sheet's margins with what
a reader holding the paper needs:

| where | what |
|---|---|
| top left | the site or tenant, then the page's title: `Acme · Cash movements` |
| top right | the page's web address |
| bottom left | `Printed 29 Oct 2026, 14:05 by Asha Rao` |
| bottom right | `Page 2 of 5` |

It is one stylesheet and one small script. There is no framework and no build step.

## Use it

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@ploutosservices/estate-print@1/print/estate-print.css">
<script src="https://cdn.jsdelivr.net/npm/@ploutosservices/estate-print@1/print/estate-print.js" defer></script>
```

(or `https://unpkg.com/@ploutosservices/estate-print@1/print/…`, or `npm install @ploutosservices/estate-print`).

## The contract

- **Left out on paper:** `.estate-bar` (the top bar), `.estate-sidenav` (the side nav), and anything you mark
  `data-estate-print="hide"`, such as your footer, a filter bar's buttons or a toast. The content then takes the
  full width of the sheet. Backgrounds are not printed. A link prints as its text, with no address after it.
- **Top left:** the text of `.estate-bar-context`, or else `.estate-bar-app`, then the page's first `<h1>`, or
  else `document.title`.
- **Bottom left:** the reader's clock, plus `data-estate-print-who="<full name>"` on `.estate-bar` when you know
  who is reading.
- **Bottom right:** `Page N of M`, from the browser's page counters. The stylesheet draws it, with 15mm margins.
- **The script** writes the three text corners into an `@page` rule on `beforeprint` and removes it on
  `afterprint`. A stylesheet cannot read the clock or the page's heading, so this part needs a script. Without the
  script, the sheet still leaves out the frame and counts its pages, and the other three corners are empty.
- **Browsers:** the corners use CSS page-margin boxes. Chromium draws them. A browser that does not draw them
  prints the content without the corners, and the frame is still left out.

## Check it

`example.html`, beside the stylesheet, is a page with a top bar, a side nav, a footer marked `hide`, and three sheets of rows.
Open it from a clone and print it, or save it as PDF. Every sheet should carry the four corners and none of the
frame. Remove the script and only `Page N of M` remains. Remove the stylesheet and the frame prints: that is the
check going red.

Questions and issues: https://github.com/ploutosservices/estate-print/issues
