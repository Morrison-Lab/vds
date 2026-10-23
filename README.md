
<!-- README.md is generated from README.Rmd. Please edit that file -->

# vds (Visualization Notes)

Notes on visualizing data, as a [Quarto](https://quarto.org/) website with R and Python code.
The site is at <https://morrison-lab.github.io/vds/>.

## Building the site

Install [Quarto](https://quarto.org/docs/get-started/) (version 1.9.36 or newer, for the `llms-txt` option), then run:

``` bash
git submodule update --init --recursive
quarto preview   # live reload while writing
quarto render    # the whole site, into _site/
```

## Adding a page

1.  Create a `.qmd` file in `chapters/`, with a distinct `output-file` for each format.
2.  Add it to the navbar and the sidebar in `_quarto-website.yml`, in the same order.
    The sidebar order sets the previous and next links.
3.  Put the data for the page in `chapters/data/` and list it under `resources:` in the page.
4.  Add each source to `references.bib` and cite it with `[@key]`.

## Layout

- `index.qmd`, `chapters/`: the pages, with the units of each page in `chapters/_subfiles/`.
- `_quarto.yml`, `_quarto-website.yml`: Quarto project and website settings.
- `latex-macros/`: the [`Morrison-Lab/macros`](https://github.com/Morrison-Lab/macros) submodule.
- `.github/`: workflows for publishing, previews, and checks.

## Why a separate repository

The `{qnp}` R package, with the lab's `ggplot2` theme and colors, lives in [its own repository](https://github.com/Morrison-Lab/qnp), not here.
The decision and its reasons are in the lab's [repository-template guidance](https://github.com/Morrison-Lab/ai-config/blob/main/shared/workflow/repo-templates.md#keep-an-r-package-and-a-notes-site-in-separate-repositories).

## License

See [LICENSE](LICENSE).
