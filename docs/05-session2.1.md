
# Session 2.1 {#session-2-1}

## Set-up

Open your Workshop project and do the following:

* Create and save a new R Markdown document named Session 2.1. get rid of the default template text from line 11 onwards.
* Add the below code to the set-up chunk and then run the code to load the packages and data.


```r
library(tidyverse)
library(patchwork)
library(psych)
dat <- read_csv(file = "data/ldt_data.csv") %>%
  mutate(language = factor(
  x = language, # column to translate
  levels = c(1, 2), # values of the original data in preferred order
  labels = c("monolingual", "bilingual") # labels for display
))

dat_long <- pivot_longer(data = dat, 
                         cols = rt_word:acc_nonword, 
                         names_sep = "_", 
                         names_to = c("dv_type", "condition"),
                         values_to = "dv") %>%
  pivot_wider(names_from = "dv_type", 
              values_from = "dv")
```

## Counting groups

You can calculate and plot some basic descriptive information about the demographics of our sample using the oroginal wide-form dataset without any additional wrangling (i.e., data processing). The code below uses the `%>%` operator and can be read as:

-   Start with the dataset `dat` *and then;*

-   Group it by the variable `language` *and then;*

-   Count the number of observations in each group *and then;*

-   Remove the grouping


```r
dat %>%
  group_by(language) %>%
  count() %>%
  ungroup()
```

<table>
 <thead>
  <tr>
   <th style="text-align:center;"> language </th>
   <th style="text-align:center;"> n </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:center;"> monolingual </td>
   <td style="text-align:center;"> 55 </td>
  </tr>
  <tr>
   <td style="text-align:center;"> bilingual </td>
   <td style="text-align:center;"> 45 </td>
  </tr>
</tbody>
</table>


`group_by()` does not result in surface level changes to the dataset, rather, it changes the underlying structure so that if groups are specified, whatever functions called next are performed separately on each level of the grouping variable. This grouping remains in the object that is created so it is important to remove it with `ungroup()` to avoid future operations on the object unknowingly being performed by groups. 

The above code therefore counts the number of observations in each group of the variable `language`. If you just need the total number of observations, you could remove the `group_by()` and `ungroup()` lines, which would perform the operation on the whole dataset, rather than by groups:


```r
dat %>%
  count()
```

<table>
 <thead>
  <tr>
   <th style="text-align:center;"> n </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:center;"> 100 </td>
  </tr>
</tbody>
</table>

## Summarise

Similarly, we may wish to calculate the mean age (and SD) of the sample and we can do so using the function `summarise()` from the `dplyr` tidyverse package.


```r
dat %>%
  summarise(mean_age = mean(age),
            sd_age = sd(age),
            n_values = n())
```

<table>
 <thead>
  <tr>
   <th style="text-align:center;"> mean_age </th>
   <th style="text-align:center;"> sd_age </th>
   <th style="text-align:center;"> n_values </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:center;"> 29.75 </td>
   <td style="text-align:center;"> 8.28 </td>
   <td style="text-align:center;"> 100 </td>
  </tr>
</tbody>
</table>

This code produces summary data in the form of a column named `mean_age` that contains the result of calculating the mean of the variable `age`. It then creates `sd_age` which does the same but for standard deviation. Finally, it uses the function `n()` to add the number of values used to calculate the statistic in a column named `n_values` - this is a useful sanity check whenever you make summary statistics.

Note that the above code will not save the result of this operation, it will simply output the result in the console. If you wish to save it for future use, you can store it in an object by using the `<-` notation and print it later by typing the object name.


```r
age_stats <- dat %>%
  summarise(mean_age = mean(age),
            sd_age = sd(age),
            n_values = n())
```

Finally, the `group_by()` function will work in the same way when calculating summary statistics -- the output of the function that is called after `group_by()` will be produced for each level of the grouping variable.


```r
dat %>%
  group_by(language) %>%
  summarise(mean_age = mean(age),
            sd_age = sd(age),
            n_values = n()) %>%
  ungroup()
```

<table>
 <thead>
  <tr>
   <th style="text-align:center;"> language </th>
   <th style="text-align:center;"> mean_age </th>
   <th style="text-align:center;"> sd_age </th>
   <th style="text-align:center;"> n_values </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:center;"> monolingual </td>
   <td style="text-align:center;"> 27.96 </td>
   <td style="text-align:center;"> 6.78 </td>
   <td style="text-align:center;"> 55 </td>
  </tr>
  <tr>
   <td style="text-align:center;"> bilingual </td>
   <td style="text-align:center;"> 31.93 </td>
   <td style="text-align:center;"> 9.44 </td>
   <td style="text-align:center;"> 45 </td>
  </tr>
</tbody>
</table>

## Pipes {#pipes-first}

Before we go further, let's take a quick detour to formally introduce the <a class='glossary' target='_blank' title='A way to order your code in a more readable format using the symbol %>%' href='https://psyteachr.github.io/glossary/p#pipe'>pipe</a>. Pipes allow you to send the output from one function straight into another function. Specifically, they send the result of the function before `%>%` to be the first argument of the function after `%>%`. It can be useful to translate the pipe as "**and then**". It's easier to show than tell, so let's look at an example.


```r
# without pipe
rt_pipes <- summarise(dat,
                      mean_rt = mean(rt_word), 
                      median_rt = median(rt_word))

# with pipe
rt_pipes <- dat %>% 
  summarise(mean_rt = mean(rt_word), 
            median_rt = median(rt_word))
```

Notice that `summarise()` no longer needs the first argument to be the data table, it is pulled in from the pipe. The power of the pipe may not be obvious now, but it will soon prove its worth. 

## Alternatives

The method we've used above is pure <code class='package'>tidyverse</code>. The good thing about this approach is that the output it creates is easy to work with and can be used with a range of different functions (for example, you can calculate a table of descriptives and then use this as the data set for a ggplot). However, there are lots of alternatives and they're useful to know because a) you'll see them when you try to Google for help and b) some of them are easier or have additional functionality.

### Base R

Rather than embedding the functions within `summarise()` you can call the summary functions straight from Base R. Let's also clear up what that `$` notation is doing. The dollar sign allows you to select items from an object, such as columns from a table. The left-hand side is the object, and the right-hand side is the item. So in the below code, we're calculating the mean of the `rt_nonword` column that is present in the data set `dat_long`.


```r
mean(dat_long$rt)
median(dat_long$rt)
```

```
## [1] 434.703
## [1] 412.5891
```

Base R also contains the function `summary()`:


```r
summary(dat)
```

```
##       id                 age               language     rt_word     
##  Length:100         Min.   :18.00   monolingual:55   Min.   :256.3  
##  Class :character   1st Qu.:24.00   bilingual  :45   1st Qu.:322.6  
##  Mode  :character   Median :28.50                    Median :353.8  
##                     Mean   :29.75                    Mean   :353.6  
##                     3rd Qu.:33.25                    3rd Qu.:379.5  
##                     Max.   :58.00                    Max.   :479.6  
##    rt_nonword       acc_word       acc_nonword   
##  Min.   :327.3   Min.   : 89.00   Min.   :76.00  
##  1st Qu.:438.8   1st Qu.: 94.00   1st Qu.:82.75  
##  Median :510.6   Median : 95.00   Median :85.00  
##  Mean   :515.8   Mean   : 95.01   Mean   :84.90  
##  3rd Qu.:582.9   3rd Qu.: 96.25   3rd Qu.:88.00  
##  Max.   :706.2   Max.   :100.00   Max.   :93.00
```

### psych::describe()

The function `describe()` from the <code class='package'>psych</code> is incredibly useful because it quickly produces a range of descriptive statistics, including standard error which is otherwise a faff to compute in R because there is no base function for standard error like there is for e.g., `sd()`.


```r
describe(dat_long)
```

<table>
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:center;"> vars </th>
   <th style="text-align:center;"> n </th>
   <th style="text-align:center;"> mean </th>
   <th style="text-align:center;"> sd </th>
   <th style="text-align:center;"> median </th>
   <th style="text-align:center;"> trimmed </th>
   <th style="text-align:center;"> mad </th>
   <th style="text-align:center;"> min </th>
   <th style="text-align:center;"> max </th>
   <th style="text-align:center;"> range </th>
   <th style="text-align:center;"> skew </th>
   <th style="text-align:center;"> kurtosis </th>
   <th style="text-align:center;"> se </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> id* </td>
   <td style="text-align:center;"> 1 </td>
   <td style="text-align:center;"> 200 </td>
   <td style="text-align:center;"> 50.500 </td>
   <td style="text-align:center;"> 28.9385070 </td>
   <td style="text-align:center;"> 50.5000 </td>
   <td style="text-align:center;"> 50.5000 </td>
   <td style="text-align:center;"> 37.0650 </td>
   <td style="text-align:center;"> 1.0000 </td>
   <td style="text-align:center;"> 100.0000 </td>
   <td style="text-align:center;"> 99.0000 </td>
   <td style="text-align:center;"> 0.0000000 </td>
   <td style="text-align:center;"> -1.2181926 </td>
   <td style="text-align:center;"> 2.0462615 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age </td>
   <td style="text-align:center;"> 2 </td>
   <td style="text-align:center;"> 200 </td>
   <td style="text-align:center;"> 29.750 </td>
   <td style="text-align:center;"> 8.2637125 </td>
   <td style="text-align:center;"> 28.5000 </td>
   <td style="text-align:center;"> 28.8250 </td>
   <td style="text-align:center;"> 6.6717 </td>
   <td style="text-align:center;"> 18.0000 </td>
   <td style="text-align:center;"> 58.0000 </td>
   <td style="text-align:center;"> 40.0000 </td>
   <td style="text-align:center;"> 1.0552985 </td>
   <td style="text-align:center;"> 0.8539197 </td>
   <td style="text-align:center;"> 0.5843327 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> language* </td>
   <td style="text-align:center;"> 3 </td>
   <td style="text-align:center;"> 200 </td>
   <td style="text-align:center;"> 1.450 </td>
   <td style="text-align:center;"> 0.4987421 </td>
   <td style="text-align:center;"> 1.0000 </td>
   <td style="text-align:center;"> 1.4375 </td>
   <td style="text-align:center;"> 0.0000 </td>
   <td style="text-align:center;"> 1.0000 </td>
   <td style="text-align:center;"> 2.0000 </td>
   <td style="text-align:center;"> 1.0000 </td>
   <td style="text-align:center;"> 0.1995019 </td>
   <td style="text-align:center;"> -1.9699740 </td>
   <td style="text-align:center;"> 0.0352664 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> condition* </td>
   <td style="text-align:center;"> 4 </td>
   <td style="text-align:center;"> 200 </td>
   <td style="text-align:center;"> 1.500 </td>
   <td style="text-align:center;"> 0.5012547 </td>
   <td style="text-align:center;"> 1.5000 </td>
   <td style="text-align:center;"> 1.5000 </td>
   <td style="text-align:center;"> 0.7413 </td>
   <td style="text-align:center;"> 1.0000 </td>
   <td style="text-align:center;"> 2.0000 </td>
   <td style="text-align:center;"> 1.0000 </td>
   <td style="text-align:center;"> 0.0000000 </td>
   <td style="text-align:center;"> -2.0099750 </td>
   <td style="text-align:center;"> 0.0354441 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> rt </td>
   <td style="text-align:center;"> 5 </td>
   <td style="text-align:center;"> 200 </td>
   <td style="text-align:center;"> 434.703 </td>
   <td style="text-align:center;"> 107.9616026 </td>
   <td style="text-align:center;"> 412.5891 </td>
   <td style="text-align:center;"> 425.6075 </td>
   <td style="text-align:center;"> 110.4421 </td>
   <td style="text-align:center;"> 256.2833 </td>
   <td style="text-align:center;"> 706.2317 </td>
   <td style="text-align:center;"> 449.9484 </td>
   <td style="text-align:center;"> 0.6437571 </td>
   <td style="text-align:center;"> -0.6201470 </td>
   <td style="text-align:center;"> 7.6340381 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> acc </td>
   <td style="text-align:center;"> 6 </td>
   <td style="text-align:center;"> 200 </td>
   <td style="text-align:center;"> 89.955 </td>
   <td style="text-align:center;"> 5.8672907 </td>
   <td style="text-align:center;"> 91.0000 </td>
   <td style="text-align:center;"> 90.2125 </td>
   <td style="text-align:center;"> 7.4130 </td>
   <td style="text-align:center;"> 76.0000 </td>
   <td style="text-align:center;"> 100.0000 </td>
   <td style="text-align:center;"> 24.0000 </td>
   <td style="text-align:center;"> -0.3225076 </td>
   <td style="text-align:center;"> -1.1034030 </td>
   <td style="text-align:center;"> 0.4148801 </td>
  </tr>
</tbody>
</table>


As you can see, `describe()` computes stats for all variables in the data set which probably isn't what you need. Instead, we can use `select()` from the <code class='package'>tidyverse</code> to select the columns we want to describe and then pass these to `describe()`. We may also want to drop some of the columns in the default `describe()` table which we can again do with `select()` - hopefully the power of the `%>%` begins to be obvious.


```r
describe_table <- dat_long %>%
  select(rt, acc)%>%
  describe() %>%
  select(-vars)
```

<table>
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:center;"> n </th>
   <th style="text-align:center;"> mean </th>
   <th style="text-align:center;"> sd </th>
   <th style="text-align:center;"> median </th>
   <th style="text-align:center;"> trimmed </th>
   <th style="text-align:center;"> mad </th>
   <th style="text-align:center;"> min </th>
   <th style="text-align:center;"> max </th>
   <th style="text-align:center;"> range </th>
   <th style="text-align:center;"> skew </th>
   <th style="text-align:center;"> kurtosis </th>
   <th style="text-align:center;"> se </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> rt </td>
   <td style="text-align:center;"> 200 </td>
   <td style="text-align:center;"> 434.703 </td>
   <td style="text-align:center;"> 107.961603 </td>
   <td style="text-align:center;"> 412.5891 </td>
   <td style="text-align:center;"> 425.6075 </td>
   <td style="text-align:center;"> 110.4421 </td>
   <td style="text-align:center;"> 256.2833 </td>
   <td style="text-align:center;"> 706.2317 </td>
   <td style="text-align:center;"> 449.9484 </td>
   <td style="text-align:center;"> 0.6437571 </td>
   <td style="text-align:center;"> -0.620147 </td>
   <td style="text-align:center;"> 7.6340381 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> acc </td>
   <td style="text-align:center;"> 200 </td>
   <td style="text-align:center;"> 89.955 </td>
   <td style="text-align:center;"> 5.867291 </td>
   <td style="text-align:center;"> 91.0000 </td>
   <td style="text-align:center;"> 90.2125 </td>
   <td style="text-align:center;"> 7.4130 </td>
   <td style="text-align:center;"> 76.0000 </td>
   <td style="text-align:center;"> 100.0000 </td>
   <td style="text-align:center;"> 24.0000 </td>
   <td style="text-align:center;"> -0.3225076 </td>
   <td style="text-align:center;"> -1.103403 </td>
   <td style="text-align:center;"> 0.4148801 </td>
  </tr>
</tbody>
</table>


Finally, because `describe()` isn't tidyverse, we can't use `group_by()` with it however, there is also a grouped version, `describeBy()`. 


```r
describebytable <- dat_long %>%
  select(language, rt, acc) %>%
  describeBy(group = "language") 
```

The object this creates is different to what we've created so far. Rather than a single data frame or tibble, this creates a <a class='glossary' target='_blank' title='A container data type that allows items with different data types to be grouped together.' href='https://psyteachr.github.io/glossary/l#list'>list</a>. Lists can contain multiple objects, datasets, and data types but it does mean they're a little bit harder to work with and we need to use the `$` notation to pull out specific objects in the list before we can manipulate them.


```r
describebytable$monolingual %>%
  select(-vars) 
```

<div class="kable-table">

<table>
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:right;"> n </th>
   <th style="text-align:right;"> mean </th>
   <th style="text-align:right;"> sd </th>
   <th style="text-align:right;"> median </th>
   <th style="text-align:right;"> trimmed </th>
   <th style="text-align:right;"> mad </th>
   <th style="text-align:right;"> min </th>
   <th style="text-align:right;"> max </th>
   <th style="text-align:right;"> range </th>
   <th style="text-align:right;"> skew </th>
   <th style="text-align:right;"> kurtosis </th>
   <th style="text-align:right;"> se </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> language* </td>
   <td style="text-align:right;"> 110 </td>
   <td style="text-align:right;"> 1.00000 </td>
   <td style="text-align:right;"> 0.000000 </td>
   <td style="text-align:right;"> 1.0000 </td>
   <td style="text-align:right;"> 1.00000 </td>
   <td style="text-align:right;"> 0.00000 </td>
   <td style="text-align:right;"> 1.0000 </td>
   <td style="text-align:right;"> 1.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> NaN </td>
   <td style="text-align:right;"> NaN </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> rt </td>
   <td style="text-align:right;"> 110 </td>
   <td style="text-align:right;"> 402.92091 </td>
   <td style="text-align:right;"> 63.432778 </td>
   <td style="text-align:right;"> 401.1573 </td>
   <td style="text-align:right;"> 400.58185 </td>
   <td style="text-align:right;"> 69.31472 </td>
   <td style="text-align:right;"> 265.9044 </td>
   <td style="text-align:right;"> 569.0931 </td>
   <td style="text-align:right;"> 303.1887 </td>
   <td style="text-align:right;"> 0.2657938 </td>
   <td style="text-align:right;"> -0.7161263 </td>
   <td style="text-align:right;"> 6.0480781 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> acc </td>
   <td style="text-align:right;"> 110 </td>
   <td style="text-align:right;"> 89.87273 </td>
   <td style="text-align:right;"> 5.824827 </td>
   <td style="text-align:right;"> 91.0000 </td>
   <td style="text-align:right;"> 90.13636 </td>
   <td style="text-align:right;"> 7.41300 </td>
   <td style="text-align:right;"> 76.0000 </td>
   <td style="text-align:right;"> 99.0000 </td>
   <td style="text-align:right;"> 23.0000 </td>
   <td style="text-align:right;"> -0.3198717 </td>
   <td style="text-align:right;"> -1.0899387 </td>
   <td style="text-align:right;"> 0.5553754 </td>
  </tr>
</tbody>
</table>

</div>


If you want to get rid of the row that contains language (because the descriptives aren't informative) then it requires a little bit of extra wrangling. The first column is actually stored in the `rownames` rather than in a column so first we have to convert these. Once we have the row names in a column, we can then use `filter()` to get remove it.


```r
rownames_to_column(describebytable$monolingual, var = "names")
```

<div class="kable-table">

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> names </th>
   <th style="text-align:right;"> vars </th>
   <th style="text-align:right;"> n </th>
   <th style="text-align:right;"> mean </th>
   <th style="text-align:right;"> sd </th>
   <th style="text-align:right;"> median </th>
   <th style="text-align:right;"> trimmed </th>
   <th style="text-align:right;"> mad </th>
   <th style="text-align:right;"> min </th>
   <th style="text-align:right;"> max </th>
   <th style="text-align:right;"> range </th>
   <th style="text-align:right;"> skew </th>
   <th style="text-align:right;"> kurtosis </th>
   <th style="text-align:right;"> se </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> language* </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 110 </td>
   <td style="text-align:right;"> 1.00000 </td>
   <td style="text-align:right;"> 0.000000 </td>
   <td style="text-align:right;"> 1.0000 </td>
   <td style="text-align:right;"> 1.00000 </td>
   <td style="text-align:right;"> 0.00000 </td>
   <td style="text-align:right;"> 1.0000 </td>
   <td style="text-align:right;"> 1.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> NaN </td>
   <td style="text-align:right;"> NaN </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> rt </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 110 </td>
   <td style="text-align:right;"> 402.92091 </td>
   <td style="text-align:right;"> 63.432778 </td>
   <td style="text-align:right;"> 401.1573 </td>
   <td style="text-align:right;"> 400.58185 </td>
   <td style="text-align:right;"> 69.31472 </td>
   <td style="text-align:right;"> 265.9044 </td>
   <td style="text-align:right;"> 569.0931 </td>
   <td style="text-align:right;"> 303.1887 </td>
   <td style="text-align:right;"> 0.2657938 </td>
   <td style="text-align:right;"> -0.7161263 </td>
   <td style="text-align:right;"> 6.0480781 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> acc </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 110 </td>
   <td style="text-align:right;"> 89.87273 </td>
   <td style="text-align:right;"> 5.824827 </td>
   <td style="text-align:right;"> 91.0000 </td>
   <td style="text-align:right;"> 90.13636 </td>
   <td style="text-align:right;"> 7.41300 </td>
   <td style="text-align:right;"> 76.0000 </td>
   <td style="text-align:right;"> 99.0000 </td>
   <td style="text-align:right;"> 23.0000 </td>
   <td style="text-align:right;"> -0.3198717 </td>
   <td style="text-align:right;"> -1.0899387 </td>
   <td style="text-align:right;"> 0.5553754 </td>
  </tr>
</tbody>
</table>

</div>


## Glossary {#glossary-intro}

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> term </th>
   <th style="text-align:left;"> definition </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> [list](https://psyteachr.github.io/glossary/l.html#list){class="glossary" target="_blank"} </td>
   <td style="text-align:left;"> A container data type that allows items with different data types to be grouped together. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> [pipe](https://psyteachr.github.io/glossary/p.html#pipe){class="glossary" target="_blank"} </td>
   <td style="text-align:left;"> A way to order your code in a more readable format using the symbol %&gt;% </td>
  </tr>
</tbody>
</table>



## Further resources {#resources-summary}

* [Data transformation cheat sheet](https://raw.githubusercontent.com/rstudio/cheatsheets/main/data-transformation.pdf)
* [Chapter 5: Data Transformation ](http://r4ds.had.co.nz/transform.html) in *R for Data Science*
* [Tidy Text](https://www.tidytextmining.com/index.html)





