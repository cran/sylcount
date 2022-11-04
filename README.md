# sylcount

* **Version:** 0.2-5
* **License:** [BSD 2-Clause](https://opensource.org/licenses/BSD-2-Clause)
* **Project home**: https://github.com/wrathematics/sylcount
* **Bug reports**: https://github.com/wrathematics/sylcount/issues


A simple English language syllable counter, plus readability score measure-er.  The package has been optimized to be very efficient, both in terms of run time performance and memory consumption.  The main methods `sylcount()` and `readability()` are both vectorized by document.  Scores for multiple documents are computed in parallel.



## Installation

The stable version is available on CRAN:

```r
install.packages("sylcount")
```

The development version is maintained on GitHub:

```r
remotes::install_github("wrathematics/sylcount", subdir="R")
```



## Example Usage

Let's start with some very simple data:

```r
library(sylcount)
a = "I am the very model of a modern major general."
b = "I have information vegetable, animal, and mineral."
```

We can easily get the readability of one "document":

```r
readability(a)
#   chars words nonwords sents sylls polys     re   gl      ari     smog
# 1    36    10        0     1    16     1 61.325 7.19 25.70125 8.841846
```

Both at once, scored one at a time:

```r
readability(c(a, b))
#   chars words nonwords sents sylls polys        re       gl      ari      smog
# 1    36    10        0     1    16     1 61.325000  7.19000 25.70125  8.841846
# 2    41     7        0     1    17     4 -5.727143 15.79714 11.56941 14.554593
```

Both at once, scored as a single document:

```r
readability(paste0(a, b, collapse=" "))
#   chars words nonwords sents sylls polys       re       gl     ari     smog
# 1    77    17        0     2    33     5 33.98397 10.63088 18.6353 12.16174
```

We can also just get the syllable counts if we want. The interface/behavior is similar to the above readability scoring function:

```r
sylcount(a)
# [[1]]
#  [1] 1 1 1 2 2 1 1 2 2 3

sylcount(c(a, b))
# [[1]]
#  [1] 1 1 1 2 2 1 1 2 2 3
# 
# [[2]]
#  [1] 1 1 4 4 3 1 3

sylcount(paste(a, b, collapse=" "))
# [[1]]
#  [1] 1 1 1 2 2 1 1 2 2 3 1 1 4 4 3 1 3

sylcount(a, counts.only=FALSE)
# [[1]]
#       word syllables
# 1        I         1
# 2       am         1
# 3      the         1
# 4     very         2
# 5    model         2
# 6       of         1
# 7        a         1
# 8   modern         2
# 9    major         2
# 10 general         3
```



## How It Works

The syllable counter first looks up each word in a hash table to see if its syllable count is known.  If not, it uses a vowel counting rule to try to approximate it.  The hash table uses a perfect hash function generated by gperf.  For more information, see `?sylcount`.

For a source for the readability measures, see `?readability`.

For performance considerations, the maximum length allowed for a word is 64 characters.  Anything longer will be treated as a "non-word".  See the package documentation for `readability()` and `sylcount()` for more details.
