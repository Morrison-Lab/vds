# Choosing Colors

Code

- [Show All Code](javascript:void(0))

- [Hide All Code](javascript:void(0))

- 

  ------------------------------------------------------------------------

- [View Source](javascript:void(0))

Published

Last modified: 2026-10-22 15:55:46 (PDT)

## 0.1 Why test a palette

A palette that looks distinct to you can merge for readers with a color vision deficiency. Instead of judging by eye, simulate the common kinds of color vision deficiency and measure how far apart the closest two colors get.

The `colorblindcheck` package ([Nowosad 2025](#ref-colorblindcheck)) does this. Its `palette_check()` function returns the smallest distance between two colors of a palette, under normal vision and under each simulated deficiency. As a rule of thumb, a smallest distance of about 10 or more means every pair is easy to tell apart. This chapter uses that rule of thumb as its cutoff; the package does not define one. Below the cutoff, some pair is hard to tell apart for some readers.

> **NOTE:**
>
> **Example 1 (Comparing qualitative palettes)** This example computes, for each palette and number of groups, the smallest distance between two colors under the worst of the three simulated deficiencies (deuteranopia, protanopia and tritanopia). A dash marks a palette that has fewer colors than groups.
>
> Show R code
>
> ``` r
> options(knitr.kable.NA = "-")
>
> # order puts blue and vermillion first and leaves out black and grey
> okabe_ito_order <- c(5, 6, 3, 7, 2, 1, 4)
> brewer <- function(name) {
>   function(n) RColorBrewer::brewer.pal(max(n, 3), name)[seq_len(n)]
> }
> palettes <- list(
>   "Okabe-Ito (ggokabeito)" = function(n) {
>     ggokabeito::palette_okabe_ito(order = okabe_ito_order)[seq_len(n)]
>   },
>   "RColorBrewer Dark2" = brewer("Dark2"),
>   "RColorBrewer Set2" = brewer("Set2"),
>   "RColorBrewer Paired" = brewer("Paired"),
>   "khroma muted" = function(n) unname(khroma::color("muted")(n)),
>   "viridis" = function(n) viridisLite::viridis(n),
>   "rcartocolor Safe" = function(n) rcartocolor::carto_pal(n, "Safe")
> )
>
> worst_case_distance <- function(make_palette, n) {
>   colors <- tryCatch(
>     make_palette(n),
>     error = function(e) NULL,
>     warning = function(w) NULL
>   )
>   if (is.null(colors) || length(colors) < n || anyNA(colors)) {
>     return(NA_real_)
>   }
>   # viridis adds an alpha channel; palette_check() wants plain "#rrggbb"
>   colors <- substr(as.character(colors), 1, 7)
>   # the first row is normal vision; the rest are the simulated deficiencies
>   min(colorblindcheck::palette_check(colors)$min_dist[-1])
> }
>
> expand.grid(palette = names(palettes), groups = c(3, 5, 7, 10)) |>
>   tibble::as_tibble() |>
>   dplyr::rowwise() |>
>   dplyr::mutate(
>     distance = worst_case_distance(palettes[[as.character(palette)]], groups)
>   ) |>
>   dplyr::ungroup() |>
>   tidyr::pivot_wider(
>     names_from = groups,
>     values_from = distance,
>     names_prefix = "n = "
>   ) |>
>   knitr::kable(digits = 1)
> ```
>
> | palette                | n = 3 | n = 5 | n = 7 | n = 10 |
> |:-----------------------|------:|------:|------:|-------:|
> | Okabe-Ito (ggokabeito) |  12.1 |  12.1 |  11.1 |     \- |
> | RColorBrewer Dark2     |  18.8 |   3.4 |   2.3 |     \- |
> | RColorBrewer Set2      |  12.9 |   1.6 |   1.6 |     \- |
> | RColorBrewer Paired    |   8.2 |   8.2 |   4.7 |    1.3 |
> | khroma muted           |  22.6 |  17.7 |  11.8 |     \- |
> | viridis                |  31.6 |  13.7 |   6.8 |    3.9 |
> | rcartocolor Safe       |   5.8 |   5.8 |   5.8 |    5.3 |
>
> Table 1: Smallest distance between two colors under simulated color vision deficiency, by palette and number of groups. Higher is better; about 10 or more is easy to tell apart.

[Example 1](#exm-palette-comparison) shows the results. The Okabe-Ito palette ([Okabe and Ito 2008](#ref-okabe2008)) stays above 10 for all 7 of its colors, and with 10 groups no palette tested reaches 10. The RColorBrewer qualitative palettes Dark2 and Set2 are fine for 3 groups and then lose distinctness quickly. The viridis palette scores well for 3 and 5 groups, but it is a sequential palette, so its neighboring colors look alike when the groups have no order. The `khroma` package’s Okabe-Ito scheme starts with black, so it is not comparable here. Its `muted` scheme has at most 9 colors.

## 0.2 What to do with many groups

With 10 groups, no tested palette keeps every pair easy to tell apart, and the table does not test 8 or 9 groups. Pair color with shape or line type, so that color is never the only way to tell groups apart. Better still, show fewer groups: combine small groups, or split the figure into panels.

The `rcartocolor` package’s `Safe` palette gives up to 12 colors, so it is the fallback when a figure needs more than 7 groups. Its smallest distance is about 5 to 6 at every size in the table, which is below the cutoff, so it is a fallback and not a recommendation. The [`fxtas` package](https://github.com/ucdavis/fxtas) uses it for its biomarker groups.

# References

Nowosad, Jakub. 2025. *Colorblindcheck: Check Color Palettes for Problems with Color Vision Deficiency*. <https://CRAN.R-project.org/package=colorblindcheck>.

Okabe, Masataka, and Kei Ito. 2008. *Color Universal Design (CUD): How to Make Figures and Presentations That Are Friendly to Colorblind People*. <https://jfly.uni-koeln.de/color/>.

Back to top
