# Session 3.1 {#session-3-1}

## Set-up

Open your Workshop project and do the following:

* Create and save a new R Markdown document named Session 3.1. get rid of the default template text from line 11 onwards.
* Download this example data file into your data folder: 
<a href="https://psyteachr.github.io/ads-v1/data/budget.csv" download>budget.csv</a>.
* Download the [Data transformation cheat sheet](https://raw.githubusercontent.com/rstudio/cheatsheets/main/data-transformation.pdf).


## Wrangling functions

Data wrangling refers to the process of cleaning, transforming, and restructuring your data to get it into the format you need for analysis and it's something you will spend an awful lot of time doing. A lot of <a class='glossary' target='_blank' title='The process of preparing data for visualisation and statistical analysis.' href='https://psyteachr.github.io/glossary/d#data-wrangling'>data wrangling</a> is covered by six functions from the <code class='package'>dplyr</code> package that is loaded as part of the <code class='package'>tidyverse</code>: `select`, `filter`, `arrange`, `mutate`, `summarise`, and `group_by`. We've covered some of these briefly already but repeating these functions with a different dataset will help.

It's worth highlighting that in this session we're going to cover these common functions and common uses of said functions. However, <code class='package'>dplyr</code> (and packages beyond it) has a huge number of additional wrangling functions and each function has many different arguments. Essentially, if you think you should be able to wrangle your data in a particular way that we haven't explicitly shown you, you almost certainly can, it might just take a bit of Googling to find out how. 

We'll use a small example table with the sales, expenses, and satisfaction for two years from four regions over two products. After you load the data, use `glimpse(budget)` or `View(budget)` to get familiar with the data.


```r
library(tidyverse)
budget <- read_csv("data/budget.csv")
```

<div class="kable-table">

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> region </th>
   <th style="text-align:left;"> product </th>
   <th style="text-align:right;"> sales_2019 </th>
   <th style="text-align:right;"> sales_2020 </th>
   <th style="text-align:right;"> expenses_2019 </th>
   <th style="text-align:right;"> expenses_2020 </th>
   <th style="text-align:left;"> satisfaction_2019 </th>
   <th style="text-align:left;"> satisfaction_2020 </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> North </td>
   <td style="text-align:left;"> widgets </td>
   <td style="text-align:right;"> 2129 </td>
   <td style="text-align:right;"> -517 </td>
   <td style="text-align:right;"> 822 </td>
   <td style="text-align:right;"> -897 </td>
   <td style="text-align:left;"> high </td>
   <td style="text-align:left;"> very high </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North </td>
   <td style="text-align:left;"> gadgets </td>
   <td style="text-align:right;"> 723 </td>
   <td style="text-align:right;"> 77 </td>
   <td style="text-align:right;"> 1037 </td>
   <td style="text-align:right;"> 1115 </td>
   <td style="text-align:left;"> very high </td>
   <td style="text-align:left;"> very high </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South </td>
   <td style="text-align:left;"> widgets </td>
   <td style="text-align:right;"> 1123 </td>
   <td style="text-align:right;"> -1450 </td>
   <td style="text-align:right;"> 1004 </td>
   <td style="text-align:right;"> 672 </td>
   <td style="text-align:left;"> high </td>
   <td style="text-align:left;"> neutral </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South </td>
   <td style="text-align:left;"> gadgets </td>
   <td style="text-align:right;"> 2022 </td>
   <td style="text-align:right;"> -945 </td>
   <td style="text-align:right;"> -610 </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:left;"> low </td>
   <td style="text-align:left;"> low </td>
  </tr>
  <tr>
   <td style="text-align:left;"> East </td>
   <td style="text-align:left;"> widgets </td>
   <td style="text-align:right;"> -728 </td>
   <td style="text-align:right;"> -51 </td>
   <td style="text-align:right;"> -801 </td>
   <td style="text-align:right;"> -342 </td>
   <td style="text-align:left;"> very low </td>
   <td style="text-align:left;"> very low </td>
  </tr>
  <tr>
   <td style="text-align:left;"> East </td>
   <td style="text-align:left;"> gadgets </td>
   <td style="text-align:right;"> -423 </td>
   <td style="text-align:right;"> -354 </td>
   <td style="text-align:right;"> 94 </td>
   <td style="text-align:right;"> 2036 </td>
   <td style="text-align:left;"> neutral </td>
   <td style="text-align:left;"> high </td>
  </tr>
  <tr>
   <td style="text-align:left;"> West </td>
   <td style="text-align:left;"> widgets </td>
   <td style="text-align:right;"> 633 </td>
   <td style="text-align:right;"> 790 </td>
   <td style="text-align:right;"> 783 </td>
   <td style="text-align:right;"> -315 </td>
   <td style="text-align:left;"> neutral </td>
   <td style="text-align:left;"> neutral </td>
  </tr>
  <tr>
   <td style="text-align:left;"> West </td>
   <td style="text-align:left;"> gadgets </td>
   <td style="text-align:right;"> 1204 </td>
   <td style="text-align:right;"> 426 </td>
   <td style="text-align:right;"> 433 </td>
   <td style="text-align:right;"> -136 </td>
   <td style="text-align:left;"> low </td>
   <td style="text-align:left;"> low </td>
  </tr>
</tbody>
</table>

</div>

### Select

You can select a subset of the columns (variables) in a table to make it easier to view or to prepare a table for display. You can also select columns in a new order.

#### By name or index

You can select columns by name or number (which is sometimes referred to as the column index). Selecting by number can be useful when the column names are long or complicated.


```r
# select single column by name
product_dat <- budget %>% select(product) 

# select single column by number
product_dat <- budget %>% select(2) 
```

You can select each column individually, separated by commas (e.g., `region, sales_2019`) but you can also select all columns from one to another by separating them with a colon (e.g., `sales_2019:expenses_2020`).

The colon notation can be much faster because you don't need to type out each individual variable name, but make sure that you know what order your columns are in and always check the output to make sure you have selected what you intended.


```r
# select columns individually
sales2019 <- budget %>% select(region, product, sales_2019)

# select columns with colon
sales2019 <- budget %>% select(region:sales_2019)
```

You can rename columns at the same time as selecting them by setting `new_name = old_col`. 


```r
regions <- budget %>% select(`Sales Region` = 1, 3:6)

head(regions, 2)
```

<div class="kable-table">

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> Sales Region </th>
   <th style="text-align:right;"> sales_2019 </th>
   <th style="text-align:right;"> sales_2020 </th>
   <th style="text-align:right;"> expenses_2019 </th>
   <th style="text-align:right;"> expenses_2020 </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> North </td>
   <td style="text-align:right;"> 2129 </td>
   <td style="text-align:right;"> -517 </td>
   <td style="text-align:right;"> 822 </td>
   <td style="text-align:right;"> -897 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North </td>
   <td style="text-align:right;"> 723 </td>
   <td style="text-align:right;"> 77 </td>
   <td style="text-align:right;"> 1037 </td>
   <td style="text-align:right;"> 1115 </td>
  </tr>
</tbody>
</table>

</div>

#### Un-selecting columns

You can select columns either by telling R which ones you want to keep as in the previous examples, or by specifying which ones you want to exclude by using a minus symbol to un-select columns. You can also use the colon notation to de-select columns, but to do so you need to put parentheses around the span first, e.g., `-(sales_2019:expenses_2020)`, not `-sales_2019:expenses_2020`.


```r
# de-select individual columns
sales <- budget %>% select(-expenses_2019, -expenses_2020)

# de-select a range of columns
sales <- budget %>% select(-(expenses_2019:expenses_2020))
```

#### Select helpers

Finally, you can select columns based on criteria about the column names.

| function | definition |
|----------|------------|
| `starts_with()` | select columns that start with a character string|
| `ends_with()` | select columns that end with a character string |
| `contains()` | select columns that contain a character string |
| `num_range()` | select columns with a name that matches the pattern `prefix` |


### Filter

Whilst `select()` chooses the columns you want to retain, `filter()` chooses the rows to retain by matching row or column criteria.

You can filter by a single criterion. This criterion can be rows where a certain column's value matches a character value (e.g., "North") or a number (e.g., 9003). It can also be the result of a logical equation (e.g., keep all rows with a specific column value larger than a certain value). The criterion is checked for each row, and if the result is FALSE, the row is removed. You can reverse equations by specifying `!=` where `!` means "not".


```r
# select all rows where region equals North
budget %>% filter(region == "North")

# select all rows where expenses_2020 were exactly equal to 200
budget %>% filter(expenses_2020 == 200)

# select all rows where sales_2019 was more than 100
budget %>% filter(sales_2019 > 100)

# everything but the North
budget %>% filter(region != "North")
```

::: {.warning data-latex=""}
Remember to use `==` and not `=` to check if two things are equivalent. A single `=` assigns the right-hand value to the left-hand variable (much like the `<-` operator).
:::

You can also select on multiple criteria by separating them by commas (rows will be kept if they match *all* criteria). Additionally, you can use `&` ("and") and `|` ("or") to create complex criteria.


```r
# regions and products with profit in both 2019 and 2020
profit_both <- budget %>% 
  filter(
    sales_2019 > expenses_2019,
    sales_2020 > expenses_2020
  )

# the same as above, using & instead of a comma
profit_both <- budget %>% 
  filter(
    sales_2019 > expenses_2019 &
    sales_2020 > expenses_2020
  )

# regions and products with profit in 2019 or 2020
profit_either <- budget %>% 
  filter(
    sales_2019 > expenses_2019 |
    sales_2020 > expenses_2020
  )

# 2020 profit greater than 1000
profit_1000 <- budget %>%
  filter(sales_2020 - expenses_2020 > 1000)
```

If you want the filter to retain multiple specific values in the same variable, the <a class='glossary' target='_blank' title='A binary operator (%in%) that returns a logical vector indicating if there is a match or not for its left operand.' href='https://psyteachr.github.io/glossary/m#match-operator'>match operator</a> (`%in%`) should be used rather than `|` (or). The `!` can also be used in combination here, but it is placed before the variable name.


```r
# retain any rows where region is north or south, and where product equals widget
budget %>%
  filter(region %in% c("North", "South"),
         product == "widgets")

# retain any rows where the region is not east or west, and where the product does not equal gadgets
budget %>%
  filter(!region %in% c("East", "West"),
         product != "gadgets")
```

<a class='glossary' target='_blank' title='A symbol that performs some mathematical or comparative process. ' href='https://psyteachr.github.io/glossary/o#operator'>Operator</a>	|Name   |is TRUE if and only if
-----------|----------------------|---------------------------------
`A < B`    |less than 	          |A is less than B
`A <= B`   |less than or equal    |A is less than or equal to B
`A > B`    |greater than 	        |A is greater than B
`A >= B`   |greater than or equal |A is greater than or equal to B
`A == B`   |equivalence 	        |A exactly equals B
`A != B`   |not equal 	          |A does not exactly equal B
`A %in% B` |in 	                  |A is an element of vector B

Finally, you can also pass many other functions to filter. For example, the package <code class='package'>stringr</code> that is loaded as part of the <code class='package'>tidyverse</code> contains many different functions for working with <a class='glossary' target='_blank' title='A piece of text inside of quotes.' href='https://psyteachr.github.io/glossary/s#string'>strings</a> (character data). For example, you you use `str_detect()` to only retain rows where the customer satisfaction rating includes the word "high"


```r
budget %>%
  filter(str_detect(satisfaction_2019, "high"))
```

<div class="kable-table">

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> region </th>
   <th style="text-align:left;"> product </th>
   <th style="text-align:right;"> sales_2019 </th>
   <th style="text-align:right;"> sales_2020 </th>
   <th style="text-align:right;"> expenses_2019 </th>
   <th style="text-align:right;"> expenses_2020 </th>
   <th style="text-align:left;"> satisfaction_2019 </th>
   <th style="text-align:left;"> satisfaction_2020 </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> North </td>
   <td style="text-align:left;"> widgets </td>
   <td style="text-align:right;"> 2129 </td>
   <td style="text-align:right;"> -517 </td>
   <td style="text-align:right;"> 822 </td>
   <td style="text-align:right;"> -897 </td>
   <td style="text-align:left;"> high </td>
   <td style="text-align:left;"> very high </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North </td>
   <td style="text-align:left;"> gadgets </td>
   <td style="text-align:right;"> 723 </td>
   <td style="text-align:right;"> 77 </td>
   <td style="text-align:right;"> 1037 </td>
   <td style="text-align:right;"> 1115 </td>
   <td style="text-align:left;"> very high </td>
   <td style="text-align:left;"> very high </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South </td>
   <td style="text-align:left;"> widgets </td>
   <td style="text-align:right;"> 1123 </td>
   <td style="text-align:right;"> -1450 </td>
   <td style="text-align:right;"> 1004 </td>
   <td style="text-align:right;"> 672 </td>
   <td style="text-align:left;"> high </td>
   <td style="text-align:left;"> neutral </td>
  </tr>
</tbody>
</table>

</div>

Note that `str_detect()` is case sensitive so it would not return values of "High" or "HIGH". You can use the function `tolower()` or `toupper()` to convert a string to lowercase or uppercase before you search for substring if you need case-insensitive matching.

::: {.warning data-latex=""}
`filter()` is incredibly powerful and can allow you to select very specific subsets of data. But, it is also quite dangerous because when you start combining multiple criteria and operators, it's very easy to accidentally specify something slightly different than what you intended. **Always check your output**. If you have a small dataset, then you can eyeball it to see if it looks right. With a larger dataset, you may wish to compute summary statistics or count the number of groups/observations in each variable to verify your filter is correct. There is no level of expertise in coding that can substitute knowing and checking your data. 
:::

### Arrange

You can sort your dataset using `arrange()`. You will find yourself needing to sort data in R much less than you do in Excel, since you don't need to have rows next to each other in order to, for example, calculate group means. But `arrange()` can be useful when preparing data for display in tables. `arrange()` works on character data where it will sort alphabetically, as well as numeric data where the default is ascending order (smallest to largest). Reverse the order using `desc()`.


```r
# arranging the table 
# first by product in alphabetical order
# then by "region" in reverse alphabetical order
budget %>%
  arrange(product, desc(region))
```

<div class="kable-table">

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> region </th>
   <th style="text-align:left;"> product </th>
   <th style="text-align:right;"> sales_2019 </th>
   <th style="text-align:right;"> sales_2020 </th>
   <th style="text-align:right;"> expenses_2019 </th>
   <th style="text-align:right;"> expenses_2020 </th>
   <th style="text-align:left;"> satisfaction_2019 </th>
   <th style="text-align:left;"> satisfaction_2020 </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> West </td>
   <td style="text-align:left;"> gadgets </td>
   <td style="text-align:right;"> 1204 </td>
   <td style="text-align:right;"> 426 </td>
   <td style="text-align:right;"> 433 </td>
   <td style="text-align:right;"> -136 </td>
   <td style="text-align:left;"> low </td>
   <td style="text-align:left;"> low </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South </td>
   <td style="text-align:left;"> gadgets </td>
   <td style="text-align:right;"> 2022 </td>
   <td style="text-align:right;"> -945 </td>
   <td style="text-align:right;"> -610 </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:left;"> low </td>
   <td style="text-align:left;"> low </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North </td>
   <td style="text-align:left;"> gadgets </td>
   <td style="text-align:right;"> 723 </td>
   <td style="text-align:right;"> 77 </td>
   <td style="text-align:right;"> 1037 </td>
   <td style="text-align:right;"> 1115 </td>
   <td style="text-align:left;"> very high </td>
   <td style="text-align:left;"> very high </td>
  </tr>
  <tr>
   <td style="text-align:left;"> East </td>
   <td style="text-align:left;"> gadgets </td>
   <td style="text-align:right;"> -423 </td>
   <td style="text-align:right;"> -354 </td>
   <td style="text-align:right;"> 94 </td>
   <td style="text-align:right;"> 2036 </td>
   <td style="text-align:left;"> neutral </td>
   <td style="text-align:left;"> high </td>
  </tr>
  <tr>
   <td style="text-align:left;"> West </td>
   <td style="text-align:left;"> widgets </td>
   <td style="text-align:right;"> 633 </td>
   <td style="text-align:right;"> 790 </td>
   <td style="text-align:right;"> 783 </td>
   <td style="text-align:right;"> -315 </td>
   <td style="text-align:left;"> neutral </td>
   <td style="text-align:left;"> neutral </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South </td>
   <td style="text-align:left;"> widgets </td>
   <td style="text-align:right;"> 1123 </td>
   <td style="text-align:right;"> -1450 </td>
   <td style="text-align:right;"> 1004 </td>
   <td style="text-align:right;"> 672 </td>
   <td style="text-align:left;"> high </td>
   <td style="text-align:left;"> neutral </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North </td>
   <td style="text-align:left;"> widgets </td>
   <td style="text-align:right;"> 2129 </td>
   <td style="text-align:right;"> -517 </td>
   <td style="text-align:right;"> 822 </td>
   <td style="text-align:right;"> -897 </td>
   <td style="text-align:left;"> high </td>
   <td style="text-align:left;"> very high </td>
  </tr>
  <tr>
   <td style="text-align:left;"> East </td>
   <td style="text-align:left;"> widgets </td>
   <td style="text-align:right;"> -728 </td>
   <td style="text-align:right;"> -51 </td>
   <td style="text-align:right;"> -801 </td>
   <td style="text-align:right;"> -342 </td>
   <td style="text-align:left;"> very low </td>
   <td style="text-align:left;"> very low </td>
  </tr>
</tbody>
</table>

</div>

### Mutate

The function `mutate()` allows you to add new columns or change existing ones by overwriting them by using the syntax `new_column = operation`.  You can add more than one column in the same mutate function by separating the columns with a comma. Once you make a new column, you can use it in further column definitions. For example, the creation of `profit` below uses the column `expenses`, which is created above it.


```r
budget2 <- budget %>%
  mutate(
    sales = sales_2019 + sales_2020,
    expenses = expenses_2019 + expenses_2020,
    profit = sales - expenses,
    region = paste(region, "Office")
  )
```

`mutate()` can also be used in conjunction with other functions and Boolean operators. For example, we can add another column to `budget2` that states whether a profit was returned that year or overwrite our `product` variable as a factor. Just like when we used <a class='glossary' target='_blank' title='An expression that evaluates to TRUE or FALSE.' href='https://psyteachr.github.io/glossary/b#boolean-expression'>Boolean expressions</a> with filter, it will evaluate the equation and return TRUE or FALSE depending on whether the observation meets the criteria.


```r
budget2 <- budget2 %>%
  mutate(profit_category = profit > 0,
         product = as.factor(product))
```

::: {.warning data-latex=""}
You can overwrite a column by giving a new column the same name as the old column (see `region` or `product`) above. Make sure that you mean to do this and that you aren't trying to use the old column value after you redefine it.
:::

You can also use `case_when()` to specify what values to return, rather than defaulting to TRUE or FALSE:


```r
budget3 <- budget2 %>%
  mutate(profit_category = case_when(profit > 0 ~ "PROFIT",
                                     profit < 0 ~ "NO PROFIT"))
```

Use it to recode values:


```r
# create a column where people get a bonus if customer satisfaction was overall high or very high

bonus <- budget3 %>%
  mutate(bonus_2019 = case_when(satisfaction_2019 %in% c("very low", "low", "neutral") ~ "no bonus",
                                satisfaction_2019 %in% c("high", "very high") ~ "bonus"))
```

And combine different criteria:


```r
# new management takes over - people only get a bonus if customer satisfaction was overall high or very high AND if a profit was returned

bonus2 <- budget3 %>%
  mutate(bonus_2020 = case_when(satisfaction_2020 == "high" & 
                                  profit_category == "PROFIT" ~ "bonus",
                                satisfaction_2020 == "very high" & 
                                  profit_category == "PROFIT" ~ "bonus",
                                TRUE ~ "No bonus")) # set all other values to "no bonus"
```

Just like `filter()`, `mutate()` is incredibly powerful and the scope of what you can create is far beyond what we can cover in this book. 

### Summarise {#dplyr-summarise}

We've already used `summarise()` but let's do a quick recap.

Let's say we want to determine the min and max sales for each year, do we can do by using `summarise()`. This will return a table where the names of the variables (columns) are what we provide on the LHS (e.g., `mean_2019`) and the values of each cell will be the result of the calculation specified on the RHS. (The better way to do this would be to use `pivot_longer()` and reshape the data but we'll ignore that for now).


```r
sales <- budget %>%
  summarise(min_2019 = min(sales_2019),
            min_2020 = min(sales_2020),
            max_2019 = max(sales_2019),
            max_2020 = max(sales_2020))

sales
```

<div class="kable-table">

<table>
 <thead>
  <tr>
   <th style="text-align:right;"> min_2019 </th>
   <th style="text-align:right;"> min_2020 </th>
   <th style="text-align:right;"> max_2019 </th>
   <th style="text-align:right;"> max_2020 </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> -728 </td>
   <td style="text-align:right;"> -1450 </td>
   <td style="text-align:right;"> 2129 </td>
   <td style="text-align:right;"> 790 </td>
  </tr>
</tbody>
</table>

</div>


### Group By {#dplyr-groupby}

We've also already looked at `group_by()` but let's do another example and calculate the min and max sales figures by product - note that the summarise code doesn't change, we just add in a call to `group_by()` before it:


```r
sales_prod <- budget %>%
  group_by(product) %>%
  summarise(min_2019 = min(sales_2019),
            min_2020 = min(sales_2020),
            max_2019 = max(sales_2019),
            max_2020 = max(sales_2020)) %>%
  ungroup()

sales_prod
```

<div class="kable-table">

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> product </th>
   <th style="text-align:right;"> min_2019 </th>
   <th style="text-align:right;"> min_2020 </th>
   <th style="text-align:right;"> max_2019 </th>
   <th style="text-align:right;"> max_2020 </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> gadgets </td>
   <td style="text-align:right;"> -423 </td>
   <td style="text-align:right;"> -945 </td>
   <td style="text-align:right;"> 2022 </td>
   <td style="text-align:right;"> 426 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> widgets </td>
   <td style="text-align:right;"> -728 </td>
   <td style="text-align:right;"> -1450 </td>
   <td style="text-align:right;"> 2129 </td>
   <td style="text-align:right;"> 790 </td>
  </tr>
</tbody>
</table>

</div>


Note that you can use the other wrangling functions on the summary table, for example: 


```r
# arrange by maximum profit
sales_prod %>%
  arrange(desc(min_2019))

# filter out gadgets
sales_prod %>%
  filter(product != "gadgets")
```

## Missing values

If you have control over your data, it is always best to keep missing values as empty cells rather than denoting missingness with a word or implausible number. If you used "missing" rather than leaving the cell empty, the entire variable would be read as character data, which means you wouldn't be able to perform mathematical operations like calculating the mean. If you use an implausible number (0 or 999 are common), then you risk these values being included in any calculations as real numbers.

However, we often don't have control over how the data come to us, so let's run through how to fix this. For this example, we're going to revert to use the lexical decision dataset from session 2.1


```r
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

This dataset doesn't have any missing values so let's introduce some and pretend that the imaginary computer we used to record reaction times didn't record any data for RTs below 350ms and the student intern we hired didn't quite follow the instructions properly and instead of leaving the cells blank, they have entered this missing data as 0.

Somewhat unintuitively, there are different types of NAs under the surface so you have to tell `case_when()` which one to use (`NA_real_`, `NA_complex`, `NA_character_`, `NA_integer_`), in our case, rt is a real number.


```r
dat_missing <- dat_long %>%
  mutate(rt = case_when(rt < 350 ~ NA_real_, # when rt is less than 350 replace with NA
                        TRUE ~ rt), # otherwise, use the value in the column rt (i.e., no change)
         acc = case_when(id %in% c("S001", "S002", "S003", "S004", "S005") ~ 0, # when id is one of these listed, replace with 0
                         TRUE ~ acc)) # otherwise use the value in the column acc
```

#### Ignore missing values

Let's deal with the rt data first. If we try and compute mean rt it  returns `NA`. This is because `NA` basically means "I don't know", and the sum of 100 and "I don't know" is "I don't know", not 100. However, when you're calculating means, you often want to just ignore missing values. Set `na.rm = TRUE` in the summary function to remove missing values before calculating.


```r
# code that will return NA
dat_missing %>%
  summarise(mean_rt = mean(rt))

# ignore missing values to return mean
dat_missing %>%
  summarise(mean_rt = mean(rt, na.rm = TRUE))
```

<div class="kable-table">

<table>
 <thead>
  <tr>
   <th style="text-align:right;"> mean_rt </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> NA </td>
  </tr>
</tbody>
</table>

</div><div class="kable-table">

<table>
 <thead>
  <tr>
   <th style="text-align:right;"> mean_rt </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 471.1353 </td>
  </tr>
</tbody>
</table>

</div>

#### Convert values to NA

For accuracy, if you compute the mean you can see we have a different problem - it does return a number but it's actually wrong because it's including those 0s in the calculation of the mean. This is an important lesson that you should always check your data - jsut because the code runs doesn't mean it's right. 


```r
dat_missing %>%
  summarise(mean_acc = mean(acc))
```

<div class="kable-table">

<table>
 <thead>
  <tr>
   <th style="text-align:right;"> mean_acc </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 85.505 </td>
  </tr>
</tbody>
</table>

</div>

To fix this we need to convert the 0s to NAs (which is what we did to create the missing data for rt) using `case_when()`:


```r
dat_fixed <- dat_missing %>%
  mutate(acc = case_when(acc == 0 ~ NA_real_,
                         TRUE ~ acc))
```

We can now calculate accuracy accurately by adding in `na.rm = TRUE`:


```r
dat_fixed %>%
  summarise(mean_acc = mean(acc, na.rm = TRUE))
```

<div class="kable-table">

<table>
 <thead>
  <tr>
   <th style="text-align:right;"> mean_acc </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 90.00526 </td>
  </tr>
</tbody>
</table>

</div>


#### Count missing values

If you want to find out how many missing or non-missing values there are in a column, use the `is.na()` function to get a <a class='glossary' target='_blank' title='A data type representing TRUE or FALSE values.' href='https://psyteachr.github.io/glossary/l#logical'>logical</a> vector of whether or not each value is missing, and use `sum()` to count how many values are TRUE or `mean()` to calculate the proportion of TRUE values.


```r
dat_missing %>%
  group_by(condition) %>%
  summarise(
    n_valid = sum(!is.na(rt)),
    n_missing = sum(is.na(rt)),
    prop_missing = mean(is.na(rt))
  )
```

<div class="kable-table">

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> condition </th>
   <th style="text-align:right;"> n_valid </th>
   <th style="text-align:right;"> n_missing </th>
   <th style="text-align:right;"> prop_missing </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> nonword </td>
   <td style="text-align:right;"> 98 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 0.02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> word </td>
   <td style="text-align:right;"> 55 </td>
   <td style="text-align:right;"> 45 </td>
   <td style="text-align:right;"> 0.45 </td>
  </tr>
</tbody>
</table>

</div>

#### Omit missing values

You may also want to remove rows that have missing values and only work from complete datasets. `drop_na()` will remove any row that has a missing observation. You can use `drop_na()` on the entire dataset which will remove any row that has *any* missing value, or you can specify to only remove rows that are missing a specific value.


```r
# remove any rows with any missing values
complete_data <- dat_missing %>%
  drop_na()

# remove any rows that are missing a value for sales
complete_rt <- dat_missing %>%
  drop_na(rt)
```

Missing data can be quite difficult to deal with depending on how it is represented. As always, no amount of coding expertise can make up for not understanding the structure and idiosyncrasies of your data. 

## Glossary {#glossary-3-1}

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> term </th>
   <th style="text-align:left;"> definition </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> [boolean expression](https://psyteachr.github.io/glossary/b.html#boolean-expression){class="glossary" target="_blank"} </td>
   <td style="text-align:left;"> An expression that evaluates to TRUE or FALSE. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> [data wrangling](https://psyteachr.github.io/glossary/d.html#data-wrangling){class="glossary" target="_blank"} </td>
   <td style="text-align:left;"> The process of preparing data for visualisation and statistical analysis. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> [logical](https://psyteachr.github.io/glossary/l.html#logical){class="glossary" target="_blank"} </td>
   <td style="text-align:left;"> A data type representing TRUE or FALSE values. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> [match operator](https://psyteachr.github.io/glossary/m.html#match-operator){class="glossary" target="_blank"} </td>
   <td style="text-align:left;"> A binary operator (%in%) that returns a logical vector indicating if there is a match or not for its left operand. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> [operator](https://psyteachr.github.io/glossary/o.html#operator){class="glossary" target="_blank"} </td>
   <td style="text-align:left;"> A symbol that performs some mathematical or comparative process. </td>
  </tr>
  <tr>
   <td style="text-align:left;"> [string](https://psyteachr.github.io/glossary/s.html#string){class="glossary" target="_blank"} </td>
   <td style="text-align:left;"> A piece of text inside of quotes. </td>
  </tr>
</tbody>
</table>



## Further resources {#resources-3-1}

* [Data transformation cheat sheet](https://raw.githubusercontent.com/rstudio/cheatsheets/main/data-transformation.pdf)
* [Chapter 5: Data Transformation ](http://r4ds.had.co.nz/transform.html) in *R for Data Science*
* [Chapter 19: Functions](https://r4ds.had.co.nz/functions.html) in *R for Data Science*
* [Introduction to stringr](https://stringr.tidyverse.org/articles/stringr.html)




