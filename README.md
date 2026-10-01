## My personal website. You can see it at [https://benjaminvatter.com](https://benjaminvatter.com)

To launch locally: hugo server

## Working-paper figures

Keep each figure beside its page in `content/project/<slug>/`:

```text
index.md
featured.jpeg  # Existing thumbnail for listings and social previews
plot.pdf       # Original vector figure, retained for future conversion
plot.svg       # Vector image displayed on the project page
```

Select the detail image in the page's front matter:

```yaml
image:
  focal_point: Center
  detail: plot.svg
  alt_text: "Describe what the figure shows."
```

Export SVG directly from the plotting software when possible. For a standalone,
single-page vector PDF, convert it with Poppler:

```sh
pdftocairo -svg plot.pdf plot.svg
```

This preserves vector paths and converts font glyphs to paths, keeping labels
sharp without requiring the reader to have the original fonts. Keep the SVG's
`viewBox` so it scales correctly. A screenshot or a PDF containing a bitmap
cannot recover vector detail through conversion.

Use `plot.svg`, not `featured.svg`: the theme automatically resizes files named
`featured*` for thumbnails and social previews, which requires a raster image.
The detail-page template serves `image.detail` directly when it is SVG. Without
that setting, it serves the existing featured image with up to 2x resolution
and higher JPEG quality, without upscaling the original. For new raster plots,
prefer a lossless PNG at least 1440 pixels wide for the default 720-pixel display.

The Charity Care and Cross-Market source figures are stored with their pages.
Vertical Integration currently uses its existing JPEG until a vector original
is available. Converting a figure is a local preparation step; the site build
does not require Poppler.

## Working-paper dates

For the Working Papers section (`content/project/`), set `date` to the revision
date of the main PDF linked by `url_pdf`, not the website edit or upload date.
Use the title-page date when it specifies a day. If it only gives a month and
year, use the PDF's `ModDate` for the day after checking that they agree:

```sh
pdfinfo static/uploads/charity_care.pdf
pdftotext -f 1 -l 1 static/uploads/charity_care.pdf -
```

Use a quoted ISO date such as `date: "2026-10-01T00:00:00Z"` and keep exactly one
`date` field per page. Recheck it whenever replacing the paper. Filesystem
modification times are unreliable after copying or checking out files. Published
articles keep their publication dates; work in progress without a linked draft
does not provide a paper revision date to verify.
