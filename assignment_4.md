Assignment 4: Data transformation with dplyr and visualization with
ggplot
================

**Instructions: Please read through this before you begin**

  - This assignment is due by **10pm on Monday 10/19/20**.

  - For this assignment, please **reproduce this markdown file** using R
    markdown. This includes the followings:
    
      - **Reproduce this markdown template.** Pay attention to all the
        formatin gin this file, including bullet points, bolded
        characters, inserted code chunks, headings, text colors, blank
        lines, etc.
    
      - **Transform the data as instructed.** Try to use `tidyverse`
        functions even if you are more comfortable with base-R
        solutions.Show **the first 6** lines of the transformed data in
        a table through RMarkdown **using the kable() function**, as
        shown in this markdown file.
    
      - **Reproduce the plots exactly as shown in this html file.** In
        two cases where the plot is not shown (Excercises 3.7 and 3.9),
        generate plots that you think can best answer the question.
    
      - Have all your code embedded within the R markdown file, and show
        **BOTH yourcode and plots** in the knitted markdown file
    
      - When a verbal response is needed, answer by editing the part in
        the R markdwon template where it says “Write your response
        here”.
    
      - Use R Markdwon functionalities to **hide messages and warnings
        when needed**. (Suggestion: messages and warnings can often be
        informative and important, so please examine them carefully and
        only turn them off when you finish the exercise).

  - Please name your R markdown file `assignment_4.Rmd` and the knitted
    markdown file `assignmrent_4.md`. Plese push both files to your
    class GitHub repository.

First, load all the required packages with the following code. Install
them if they are not installed yet.

``` r
library(tidyverse)
library(knitr)
library(gapminder) #
```

**Exercise 1. Theopylline experiment**

This excercise uses the `Theoph` data frame (comes with your R
installation), which has 132 rows and 5 columns of data from an
experiment on the pharmacokinetics of the anti-asthmatic drug
theophylline. Tweleve subjects were given oral doses of theophylline
then serum concentrations were measured at 11 time points over the next
25 hours. You can learn more about this dataset by running `?Theoph`

Have a look at the data structure

``` r
kable(head(Theoph))
```

| Subject |   Wt | Dose | Time |  conc |
| :------ | ---: | ---: | ---: | ----: |
| 1       | 79.6 | 4.02 | 0.00 |  0.74 |
| 1       | 79.6 | 4.02 | 0.25 |  2.84 |
| 1       | 79.6 | 4.02 | 0.57 |  6.57 |
| 1       | 79.6 | 4.02 | 1.12 | 10.50 |
| 1       | 79.6 | 4.02 | 2.02 |  9.66 |
| 1       | 79.6 | 4.02 | 3.82 |  8.58 |

**1.1 Select columns that contain a lower case“t” in the `Theoph`
dataset. Do not mannually list all the columns to include.**

``` r
kable(head(Theoph %>% 
  select(Subject, Wt)))
```

| Subject |   Wt |
| :------ | ---: |
| 1       | 79.6 |
| 1       | 79.6 |
| 1       | 79.6 |
| 1       | 79.6 |
| 1       | 79.6 |
| 1       | 79.6 |

``` r
kable(head(select(Theoph, contains(('t'))))) # this selects the upper case T why?
```

| Subject |   Wt | Time |
| :------ | ---: | ---: |
| 1       | 79.6 | 0.00 |
| 1       | 79.6 | 0.25 |
| 1       | 79.6 | 0.57 |
| 1       | 79.6 | 1.12 |
| 1       | 79.6 | 2.02 |
| 1       | 79.6 | 3.82 |

**Rename the `Wt` column to `Weight` and `conc` column to
`Concentration` in the `Theoph` dataset.**

``` r
kable(head(Theoph %>% 
select(everything(), Weight = Wt, Concentration =conc)))
```

| Subject | Weight | Dose | Time | Concentration |
| :------ | -----: | ---: | ---: | ------------: |
| 1       |   79.6 | 4.02 | 0.00 |          0.74 |
| 1       |   79.6 | 4.02 | 0.25 |          2.84 |
| 1       |   79.6 | 4.02 | 0.57 |          6.57 |
| 1       |   79.6 | 4.02 | 1.12 |         10.50 |
| 1       |   79.6 | 4.02 | 2.02 |          9.66 |
| 1       |   79.6 | 4.02 | 3.82 |          8.58 |

**Extract the `Dose` greater than 4.5 and `Time` greater than the mean
`Time`.**

``` r
kable(head(Theoph %>% 
  filter(Dose > 4.5, Time > mean(Time))))
```

| Subject |   Wt | Dose |  Time | conc |
| :------ | ---: | ---: | ----: | ---: |
| 3       | 70.5 | 4.53 |  7.07 | 5.30 |
| 3       | 70.5 | 4.53 |  9.00 | 4.90 |
| 3       | 70.5 | 4.53 | 12.15 | 3.70 |
| 3       | 70.5 | 4.53 | 24.17 | 1.05 |
| 5       | 54.6 | 5.86 |  7.02 | 7.09 |
| 5       | 54.6 | 5.86 |  9.10 | 5.90 |

**1.4 Sort the `Theoph` dataset by `Wt` from smallest to largest and
secondarily by Time from largest to smallest**

``` r
kable(head(Theoph %>% 
  arrange(Wt, desc(Time))))
```

| Subject |   Wt | Dose |  Time | conc |
| :------ | ---: | ---: | ----: | ---: |
| 5       | 54.6 | 5.86 | 24.35 | 1.57 |
| 5       | 54.6 | 5.86 | 12.00 | 4.37 |
| 5       | 54.6 | 5.86 |  9.10 | 5.90 |
| 5       | 54.6 | 5.86 |  7.02 | 7.09 |
| 5       | 54.6 | 5.86 |  5.02 | 7.56 |
| 5       | 54.6 | 5.86 |  3.50 | 8.74 |

**1.5 Create a new column called `Quantity`that equals to`Wt` x `Dose`
in the `Theoph` dataset. This will tell you the absolute quantity of
drug administered to the subject (in mg). Replace the `Dose` Variable**

``` r
kable(head(Theoph %>% 
  mutate(Quantity=Wt*Dose) %>% 
  select(Subject,Quantity,Time, conc)))
```

| Subject | Quantity | Time |  conc |
| :------ | -------: | ---: | ----: |
| 1       |  319.992 | 0.00 |  0.74 |
| 1       |  319.992 | 0.25 |  2.84 |
| 1       |  319.992 | 0.57 |  6.57 |
| 1       |  319.992 | 1.12 | 10.50 |
| 1       |  319.992 | 2.02 |  9.66 |
| 1       |  319.992 | 3.82 |  8.58 |
