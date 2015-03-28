---
title: "From zero to hero in md!"
output: html_document
published: false
status: process
---

It's my philosophy that a person should look to do things in the most efficient way possible. Many tutorials on using R for data analysis and machine learning exist, however many of these rely on base R functionality. While it is arguably a good thing to have a deep understanding of the fundamentals of the language you are using to do your data analysis, I am of the opinion that it isn't essential, and is definitely not the most efficient way. There exist many tools built on top of R that make data analysis a much simpler process, and are advantageous for a hopeful practicioner.

# A quick motivating example

Let's take a quick look at a simple dataset, the famous `mtcars`


{% highlight r %}
# load the dataset:
data(mtcars)
# The columns of mtcars are named:
colnames(mtcars)
{% endhighlight %}



{% highlight text %}
##  [1] "mpg"  "cyl"  "disp" "hp"   "drat" "wt"   "qsec" "vs"   "am"   "gear"
## [11] "carb"
{% endhighlight %}

Consider a very common operation in data analysis: selecting a set of columns from a dataset, and creating a new colum derived from the selected columns.

Let's say we want to extract the `cyl`, `hp`, `drat` and `wt` columns. Then, we want to create a new column which is a ratio of the `hp` to the `wt` values.

###Base R solution

In base R there are several ways to do so:


{% highlight r %}
#Method 1: by column number
method_1 <- mtcars[, c(2, 4:6)]
method_1[,5] <- method_1[,2] / method_1[,4]

#Method 2: by column name
method_2 <- mtcars[, c("cyl", "hp", "drat", "wt")]
method_2[,"hp_wt_ratio"] <- method_2[,"hp"] / method_2[,"wt"]

#Method 3: the $ operator
method_3 <- data.frame("cyl" = mtcars$cyl, "hp" = mtcars$hp, "drat" = mtcars$drat, "wt" = mtcars$wt)
method_3$hp_wt_ratio <- method_3$hp / method_3$wt 
{% endhighlight %}

All of these methods return a data frame containing the desired columns. This data set, however, has the names of the rows encoded as `row.names`, which is not a column in the same way as the other variables. This is bad practice, and leads to `method_3` not being equal to the other two methods.

Do these methods look particularly intuitive? Not to me. There is the comma sitting inside the brackets, the columns names are sometimes in quotation marks and sometimes not. Is there a better solution?

###Modern solution

There is a much simpler solution offered to us by the `dplyr` package written by [Hadley Wickham](http://had.co.nz/). 

This is what a more modern solution might look like:


{% highlight r %}
#Load the package so that we can use the functions it provides
library(dplyr)

#Use the select() function to get the columns
method_4 <- select(mtcars, cyl, hp, drat, wt)
#Use the mutate() function to name and calculate the new column
method_4 <- mutate(method_4, hp_wt_ratio = hp / wt)
{% endhighlight %}

Actually, this can be further simplified into a single call using "pipes" (more on these later) as follows:


{% highlight r %}
method5 <- mtcars %>% 
  select(cyl, hp, drat, wt) %>%
  mutate(hp_wt_ratio = hp / wt)
{% endhighlight %}

###Is the modern solution better?

Is this simpler? In my opinion: yes. `method_5` can be essentially read out in plain english, "Take the mtcars data, select these columns, mutate these variables into a new column". Furthemore, the modern solution does away with confusing quotation marks and commas.

#Where to go from here?

I hope that example was enough to sway you into thinking that the base R solution is not always the best method.
In this set of tutorials we will go from installing R to applying machine learning algorithms to a real world dataset. The next lesson will be concerned with helping a beginner user to set up an R ecosystem with the packages we will use throughout the rest of the lessons; if you are already set up in that regard then feel free to skip it. 

the following lessons we will explore the verbs of data cleaning, data analysis, and finally machine learning. Along the way we will explore best practices for keeping your analysis organized and reproducible, and also methods for presenting your data. Let's get started!

