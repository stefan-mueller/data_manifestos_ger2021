2021 German federal election manifestos
================

Repository containing manifestos of the main parties competing in the
2021 German federal elections (26 September 2021). The repository
currently contains the manifesto versions listed below. I will update
the files when all parties have published their final manifestos.

| Party      | Description                                                                                                                                |
|------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| AfD        | [Final version (20 May 2021)](https://cdn.afd.tools/wp-content/uploads/sites/111/2021/05/2021-05-20-_-AfD-Bundestagswahlprogramm-2021.pdf) |
| CDU/CSU    | [Final version (21 June 2021)](https://www.csu.de/common/download/Regierungsprogramm.pdf)                                                  |
| FDP        | [Final version (16 May 2021)](https://www.fdp.de/sites/default/files/2021-06/FDP_Programm_Bundestagswahl2021_1.pdf)                        |
| Greens     | [Draft (13 June 2021)](https://cms.gruene.de/uploads/documents/2021_Wahlprogrammentwurf.pdf)                                               |
| Left Party | [Draft (20 June 2021)](https://www.die-linke.de/fileadmin/download/wahlen2021/BTWP21_Entwurf_Vorsitzende.pdf)                              |
| SPD        | [Final version (9 May 2021)](https://www.spd.de/fileadmin/Dokumente/Beschluesse/Programm/SPD-Zukunftsprogramm.pdf)                         |

## Files

The repository contains the manifestos in the following formats:

-   **Original PDF files**: the folder `manifestos_originals` contains
    the manifestos published on the parties’ websites
-   **Edited PDF files**: the folder `manifestos_clean_pdf` contains
    cleaned PDF files. I removed title pages, page numbers, headers,
    footers, table of contents, and the index
-   **Text files**: the folder `manifestos_clean_txt` contains `.txt`
    files of the cleaned PDFs
-   **Corpus**: the file `data_corpus_manifestos_ger2021.rds` contains
    all party manifestos as a [**quanteda**](https://quanteda.io) text
    corpus

## Example

This example shows how to load the corpus object and extract the most
frequent terms in each manifesto (with minimal pre-processing and no
compounding of multiword expressions).

``` r
## Load packages
library(quanteda)
library(quanteda.textstats)
library(ggplot2)

## Load text corpus
data_corpus_manifestos_ger2021 <- readRDS("data_corpus_manifestos_ger2021.rds")

## Tokenize corpus and transform to document feature matrix
dfmat_man <- data_corpus_manifestos_ger2021 %>% 
  tokens(remove_punct = TRUE, remove_numbers = TRUE) %>% 
  tokens_compound(phrase("* innen")) %>%  # compound *innen
  tokens_remove(pattern = c(stopwords("de"), "dass")) %>% 
  dfm() 


## Get most frequent words by party
tstat_freq <- textstat_frequency(dfmat_man, 
                                 groups = party, 
                                 n = 10)

## Plot most frequent words
ggplot(data = tstat_freq, aes(x = factor(nrow(tstat_freq):1), y = frequency)) +
  geom_point() +
  facet_wrap(~ group, scales = "free_y") +
  coord_flip() +
  scale_x_discrete(breaks = nrow(tstat_freq):1,
                   labels = tstat_freq$feature) +
  labs(x = NULL, y = "Most frequent words") +
  theme_minimal()
```

![](README_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

## Citation

Feel free to use the manifestos or edited files for your own work.
Please cite the data as follows:

Stefan Müller 2021. *2021 German federal election manifestos*. Version
0.1: <https://github.com/stefan-mueller/data_manifestos_ger2021>.

If you have any questions or suggestions, please file a [GitHub
issue](https://github.com/stefan-mueller/data_manifestos_ger2021/issues)
or [get in touch with me](https://muellerestefan.net).
