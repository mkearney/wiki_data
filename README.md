wiki\_data
================

wiki\_data
----------

The purpose of this repository is to collect code for downloading, parsing, and analyzing wikipedia pageviews data.

### working script

``` r
## play with this interactive web tool
browseURL("https://tools.wmflabs.org/pageviews/?project=en.wikipedia.org&platform=all-access&agent=user&range=latest-20&pages=Cat|Dog")

## get the current working directory
getwd()

## set working directory (for example, mine is set to this)
## For example, I created a "wikidata" folder in my "R" folder (which I usually
## work from for R projects)
#setwd("/Users/mwk/R/wikidata")

## make sure your data/wiki files are saved to whatever the working directory 
## file location is!

## read in functions from the other script file
source("read_wiki_data.R")

##----------------------------------------------------------------------------##
##                      READ IN ENTIRE FILE OF WIKI DATA                      ##
##----------------------------------------------------------------------------##


## go here to download the file
browseURL("https://dumps.wikimedia.org/other/pageviews/2017/2017-09/")

## name/location of where you saved the data
file_name <- "~/Downloads/pageviews-20170901-090000"

## read 1 million lines starting at 1700000
x <- wiki_data(file_name, skip = 1700000, n_max = 1000000)

## subset only english lanugage rows
x <- x[x$lang == "en", ]

## save the data as a CSV file
readr::write_csv(
  x, "wiki-english-data.csv"
)

##----------------------------------------------------------------------------##
##                        PLAY WITH FORMATTED WIKI DATA                       ##
##----------------------------------------------------------------------------##

## read csv data
x <- readr::read_csv(
  "wiki-english-data.csv"
)

## key word to search for
search_keyword <- "interpersonal"

## see what topics include the key word
grep(search_keyword, x$topic, ignore.case = TRUE, value = TRUE)

## see rows for all matching keywords (organized by number of pageviews)
ip_topics <- x[grep(search_keyword, x$topic, ignore.case = TRUE), ]
ip_topics[order(ip_topics$pageviews, decreasing = TRUE), ]

## organize all rows by pageviews
x[order(x$pageviews, decreasing = TRUE), ]

## subset data for only one keyword
keyword <- "Interpersonal_attraction"

## view row of selected keyword
x[grep(keyword, x$topic), ]
```
