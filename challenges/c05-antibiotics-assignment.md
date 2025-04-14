Antibiotics
================
Carter Harris
2025-04-14

*Purpose*: Creating effective data visualizations is an *iterative*
process; very rarely will the first graph you make be the most
effective. The most effective thing you can do to be successful in this
iterative process is to *try multiple graphs* of the same data.

Furthermore, judging the effectiveness of a visual is completely
dependent on *the question you are trying to answer*. A visual that is
totally ineffective for one question may be perfect for answering a
different question.

In this challenge, you will practice *iterating* on data visualization,
and will anchor the *assessment* of your visuals using two different
questions.

*Note*: Please complete your initial visual design **alone**. Work on
both of your graphs alone, and save a version to your repo *before*
coming together with your team. This way you can all bring a diversity
of ideas to the table!

<!-- include-rubric -->

# Grading Rubric

<!-- -------------------------------------------------- -->

Unlike exercises, **challenges will be graded**. The following rubrics
define how you will be graded, both on an individual and team basis.

## Individual

<!-- ------------------------- -->

| Category | Needs Improvement | Satisfactory |
|----|----|----|
| Effort | Some task **q**’s left unattempted | All task **q**’s attempted |
| Observed | Did not document observations, or observations incorrect | Documented correct observations based on analysis |
| Supported | Some observations not clearly supported by analysis | All observations clearly supported by analysis (table, graph, etc.) |
| Assessed | Observations include claims not supported by the data, or reflect a level of certainty not warranted by the data | Observations are appropriately qualified by the quality & relevance of the data and (in)conclusiveness of the support |
| Specified | Uses the phrase “more data are necessary” without clarification | Any statement that “more data are necessary” specifies which *specific* data are needed to answer what *specific* question |
| Code Styled | Violations of the [style guide](https://style.tidyverse.org/) hinder readability | Code sufficiently close to the [style guide](https://style.tidyverse.org/) |

## Submission

<!-- ------------------------- -->

Make sure to commit both the challenge report (`report.md` file) and
supporting files (`report_files/` folder) when you are done! Then submit
a link to Canvas. **Your Challenge submission is not complete without
all files uploaded to GitHub.**

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(ggrepel)
```

*Background*: The data\[1\] we study in this challenge report the
[*minimum inhibitory
concentration*](https://en.wikipedia.org/wiki/Minimum_inhibitory_concentration)
(MIC) of three drugs for different bacteria. The smaller the MIC for a
given drug and bacteria pair, the more practical the drug is for
treating that particular bacteria. An MIC value of *at most* 0.1 is
considered necessary for treating human patients.

These data report MIC values for three antibiotics—penicillin,
streptomycin, and neomycin—on 16 bacteria. Bacteria are categorized into
a genus based on a number of features, including their resistance to
antibiotics.

``` r
## NOTE: If you extracted all challenges to the same location,
## you shouldn't have to change this filename
filename <- "./data/antibiotics.csv"

## Load the data
df_antibiotics <- read_csv(filename)
```

    ## Rows: 16 Columns: 5
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): bacteria, gram
    ## dbl (3): penicillin, streptomycin, neomycin
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
df_antibiotics %>% knitr::kable()
```

| bacteria                        | penicillin | streptomycin | neomycin | gram     |
|:--------------------------------|-----------:|-------------:|---------:|:---------|
| Aerobacter aerogenes            |    870.000 |         1.00 |    1.600 | negative |
| Brucella abortus                |      1.000 |         2.00 |    0.020 | negative |
| Bacillus anthracis              |      0.001 |         0.01 |    0.007 | positive |
| Diplococcus pneumonia           |      0.005 |        11.00 |   10.000 | positive |
| Escherichia coli                |    100.000 |         0.40 |    0.100 | negative |
| Klebsiella pneumoniae           |    850.000 |         1.20 |    1.000 | negative |
| Mycobacterium tuberculosis      |    800.000 |         5.00 |    2.000 | negative |
| Proteus vulgaris                |      3.000 |         0.10 |    0.100 | negative |
| Pseudomonas aeruginosa          |    850.000 |         2.00 |    0.400 | negative |
| Salmonella (Eberthella) typhosa |      1.000 |         0.40 |    0.008 | negative |
| Salmonella schottmuelleri       |     10.000 |         0.80 |    0.090 | negative |
| Staphylococcus albus            |      0.007 |         0.10 |    0.001 | positive |
| Staphylococcus aureus           |      0.030 |         0.03 |    0.001 | positive |
| Streptococcus fecalis           |      1.000 |         1.00 |    0.100 | positive |
| Streptococcus hemolyticus       |      0.001 |        14.00 |   10.000 | positive |
| Streptococcus viridans          |      0.005 |        10.00 |   40.000 | positive |

# Visualization

<!-- -------------------------------------------------- -->

### **q1** Prototype 5 visuals

To start, construct **5 qualitatively different visualizations of the
data** `df_antibiotics`. These **cannot** be simple variations on the
same graph; for instance, if two of your visuals could be made identical
by calling `coord_flip()`, then these are *not* qualitatively different.

For all five of the visuals, you must show information on *all 16
bacteria*. For the first two visuals, you must *show all variables*.

*Hint 1*: Try working quickly on this part; come up with a bunch of
ideas, and don’t fixate on any one idea for too long. You will have a
chance to refine later in this challenge.

*Hint 2*: The data `df_antibiotics` are in a *wide* format; it may be
helpful to `pivot_longer()` the data to make certain visuals easier to
construct.

#### Visual 1 (All variables)

In this visual you must show *all three* effectiveness values for *all
16 bacteria*. This means **it must be possible to identify each of the
16 bacteria by name.** You must also show whether or not each bacterium
is Gram positive or negative.

``` r
# WRITE YOUR CODE HERE
df_antibiotics_long <-
  df_antibiotics %>% 
  pivot_longer(
    cols = c(penicillin, streptomycin, neomycin),
    names_to = "drug",
    values_to = "mic"
  ) %>% 
  mutate(bacteria = fct_reorder(bacteria, mic))

df_antibiotics_long
```

    ## # A tibble: 48 × 4
    ##    bacteria              gram     drug             mic
    ##    <fct>                 <chr>    <chr>          <dbl>
    ##  1 Aerobacter aerogenes  negative penicillin   870    
    ##  2 Aerobacter aerogenes  negative streptomycin   1    
    ##  3 Aerobacter aerogenes  negative neomycin       1.6  
    ##  4 Brucella abortus      negative penicillin     1    
    ##  5 Brucella abortus      negative streptomycin   2    
    ##  6 Brucella abortus      negative neomycin       0.02 
    ##  7 Bacillus anthracis    positive penicillin     0.001
    ##  8 Bacillus anthracis    positive streptomycin   0.01 
    ##  9 Bacillus anthracis    positive neomycin       0.007
    ## 10 Diplococcus pneumonia positive penicillin     0.005
    ## # ℹ 38 more rows

``` r
df_antibiotics_long %>% 
  ggplot(aes(bacteria, log10(mic), fill = drug, color = gram)) +
  theme(axis.text.x = element_text(angle = 45, hjust = 1)) +
  geom_col(position = "dodge") +
  scale_color_manual(values = c("negative" = "black", "positive" = "white"))
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.1-1.png)<!-- -->

``` r
  #facet_wrap(~gram, scales = "free_x")
```

#### Visual 2 (All variables)

In this visual you must show *all three* effectiveness values for *all
16 bacteria*. This means **it must be possible to identify each of the
16 bacteria by name.** You must also show whether or not each bacterium
is Gram positive or negative.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
# WRITE YOUR CODE HERE
df_antibiotics_long %>% 
  ggplot(aes(drug, mic, color = gram)) +
  theme(
    axis.text.x = element_text(angle = 45, hjust = 1), aspect.ratio = 1, strip.text = 
    #element_text(size = 8),  # Increase facet label size
    #plot.margin = margin(10, 10, 10, 10)) +
  ) +
  geom_point(alpha = 0.6) +
  facet_wrap(vars(bacteria), scales = "free_y")
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.2-1.png)<!-- -->

``` r
#ggsave("test_plot.png", width = 12, height = 10) 
```

#### Visual 3 (Some variables)

In this visual you may show a *subset* of the variables (`penicillin`,
`streptomycin`, `neomycin`, `gram`), but you must still show *all 16
bacteria*.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
# WRITE YOUR CODE HERE
df_antibiotics_long %>% 
  ggplot(aes(bacteria, drug, fill = log10(mic))) +
  theme(axis.text.x = element_text(angle = 45, hjust = 1)) +
  geom_tile(size = 0.5) +
  facet_wrap(~gram, scales = "free_x")
```

    ## Warning: Using `size` aesthetic for lines was deprecated in ggplot2 3.4.0.
    ## ℹ Please use `linewidth` instead.
    ## This warning is displayed once every 8 hours.
    ## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
    ## generated.

![](c05-antibiotics-assignment_files/figure-gfm/q1.3-1.png)<!-- -->

#### Visual 4 (Some variables)

In this visual you may show a *subset* of the variables (`penicillin`,
`streptomycin`, `neomycin`, `gram`), but you must still show *all 16
bacteria*.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
# WRITE YOUR CODE HERE
df_antibiotics_long %>% 
  # group_by(bacteria) %>% 
  # mutate(mean_mic = log10(mean(mic))) %>% 
  ggplot(aes(bacteria, log10(mic), fill = gram)) +
  theme(axis.text.x = element_text(angle = 45, hjust = 1)) +
  geom_boxplot()
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.4-1.png)<!-- -->

#### Visual 5 (Some variables)

In this visual you may show a *subset* of the variables (`penicillin`,
`streptomycin`, `neomycin`, `gram`), but you must still show *all 16
bacteria*.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
# WRITE YOUR CODE HERE
df_antibiotics_long %>%
  #group_by(gram) %>% 
  #mutate(mean_mic = mean(mic)) %>% 
  ggplot(aes(gram, log10(mic))) +
  geom_boxplot()
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.5-1.png)<!-- -->

### **q2** Assess your visuals

There are **two questions** below; use your five visuals to help answer
both Guiding Questions. Note that you must also identify which of your
five visuals were most helpful in answering the questions.

*Hint 1*: It’s possible that *none* of your visuals is effective in
answering the questions below. You may need to revise one or more of
your visuals to answer the questions below!

*Hint 2*: It’s **highly unlikely** that the same visual is the most
effective at helping answer both guiding questions. **Use this as an
opportunity to think about why this is.**

#### Guiding Question 1

> How do the three antibiotics vary in their effectiveness against
> bacteria of different genera and Gram stain?

*Observations*

- What is your response to the question above?

  - First, looking at the Gram stain:

    - As an aggregate, Gram stain negative bacteria tend to be more
      resistant to all three bacteria types than Gram stain positive,
      with penicillin having a particularly huge outlier MIC value and
      being very ineffective at treating Gram stain positive bacteria.
      However, neomycin is the most effective for Gram stain negative.

    - For all Gram stain positive bacteria, either neomycin or pencillin
      are the best choices with the lowest MIC value. Streptomycin tends
      to be much less effective against this bacteria.

  - Looking at different genera:

    - Penicillin is often the best choice for streptococcus bacteria,
      although this is not always the case, as streptococcus fecalis is
      a notable outlier.

    - Staphylcoccus and samonella genera are usually best treated by
      neomycin.

- Which of your visuals above (1 through 5) is **most effective** at
  helping to answer this question?

  - I found a combination of my second and third visual the most
    effective at answering this question. Visual 5 was also helpful for
    gram strain.

- Why?

  - Visual 3 was particularly helpful for providing a side-by side
    comparison of each bacteria to find common pattern on a common
    scale. However, the color hue used limited the precision of values I
    was able to view. To fill this gap, I referred back to visual 2
    after finding a pattern to confirm results. Having each plot on an
    independent axis was much easier compared to the log plot, which
    with positive and negative values and the huge variation in MIC
    value ranges, was slightly difficult to read. The common x-axis
    between graphs was also particularly helpful for identifying
    patterns.
  - Visual 5 was helpful by showcasing aggregate trends across the gram
    data.

#### Guiding Question 2

In 1974 *Diplococcus pneumoniae* was renamed *Streptococcus pneumoniae*,
and in 1984 *Streptococcus fecalis* was renamed *Enterococcus fecalis*
\[2\].

> Why was *Diplococcus pneumoniae* renamed *Streptococcus pneumoniae*?

*Observations*

- What is your response to the question above?

  - The renamed Streptococcus pneumonia responds to antibiotics in much
    the same way as other members of the streptococcus genera with
    penicillin being very effective and both other antibiotics tested,
    neomycin and strepomycin, being fairly ineffective.

  - Streptococcus fecalis was likely renamed as it is an outlier in the
    streptococcus genera in terms of its antibiotics response. It can
    not be treated with penicillin as the other genera members can and
    is instead best treated by neomycin.

- Which of your visuals above (1 through 5) is **most effective** at
  helping to answer this question?

  - Visual 2 was once again the most helpful, although visual 3 was less
    helpful for this more specific comparison.

- Why?

  - Visual 2 provided a clear side by side of each bacteria that quickly
    provided pattern recognition, like that of streptococcus’ simialr
    reactions to pencillin and the other antibiotics.

# References

<!-- -------------------------------------------------- -->

\[1\] Neomycin in skin infections: A new topical antibiotic with wide
antibacterial range and rarely sensitizing. Scope. 1951;3(5):4-7.

\[2\] Wainer and Lysen, “That’s Funny…” *American Scientist* (2009)
[link](https://www.americanscientist.org/article/thats-funny)
