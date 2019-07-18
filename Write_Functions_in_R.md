
Functions allow you to reuse code, without re-writing the lines of code
to do similar tasks. You may simply call the function anywhere else in
the main program. There are a few steps to write a function. First, you
will need to determine the reason for writing the function, and
thereafter define data and features required to incorporate into the
program. Finally do the steps or calculation required, and return the
output.

In this case, we will create a simple function for re-charting bar
graph.

``` r
#Load ggplot2 library to build bar graph visualization
library(ggplot2)
```

    ## Registered S3 methods overwritten by 'ggplot2':
    ##   method         from 
    ##   [.quosures     rlang
    ##   c.quosures     rlang
    ##   print.quosures rlang

``` r
#Load built-in dataset 
data(mtcars)

#View top 6 rows of the dataset
head(mtcars)
```

    ##                    mpg cyl disp  hp drat    wt  qsec vs am gear carb
    ## Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
    ## Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
    ## Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
    ## Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
    ## Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
    ## Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1

``` r
#View structure of the dataset
str(mtcars)
```

    ## 'data.frame':    32 obs. of  11 variables:
    ##  $ mpg : num  21 21 22.8 21.4 18.7 18.1 14.3 24.4 22.8 19.2 ...
    ##  $ cyl : num  6 6 4 6 8 6 8 4 4 6 ...
    ##  $ disp: num  160 160 108 258 360 ...
    ##  $ hp  : num  110 110 93 110 175 105 245 62 95 123 ...
    ##  $ drat: num  3.9 3.9 3.85 3.08 3.15 2.76 3.21 3.69 3.92 3.92 ...
    ##  $ wt  : num  2.62 2.88 2.32 3.21 3.44 ...
    ##  $ qsec: num  16.5 17 18.6 19.4 17 ...
    ##  $ vs  : num  0 0 1 1 0 1 0 1 1 1 ...
    ##  $ am  : num  1 1 1 0 0 0 0 0 0 0 ...
    ##  $ gear: num  4 4 4 3 3 3 3 4 4 4 ...
    ##  $ carb: num  4 4 1 1 2 1 4 2 2 4 ...

This function will output the bar graph required, by calling the list of
argument names within the
brackets.

``` r
bar_graph <- function(data, column, colfill, xaxis, fill_title, main_title){
    ggplot(data, aes(.data[[column]], group=.data[[colfill]],fill=.data[[colfill]]), inherit.aes=F)+
        geom_bar(mapping=aes(x=.data[[column]], y=..prop.., group=.data[[colfill]], fill=.data[[colfill]]),
                 position=position_dodge2(preserve = "single"), stat="count")+
        geom_text(aes(y=..prop.., label=scales::percent(..prop..)), stat="count",
                  position=position_dodge(0.8), vjust=-0.5, size=3)+
        scale_y_continuous(labels = scales::percent_format(), limit=c(0,1))+
        labs(x = xaxis, y="Percentage", fill=fill_title, title=main_title)+
    theme_dark()
}
```

This is simply to convert numeric data type to factor, so it will appear
as consistent color for each data value instead of different color
grading/scaling.

``` r
mtcars$cyl <- as.factor(mtcars$cyl)
mtcars$am <- as.factor(mtcars$am)
```

Call out the function, by inputing the parameters.

``` r
bar_graph(mtcars, 'cyl', 'am', 
          "Number of Cylinders", 
          "Transmission",
          "% Distribution of Number of Cylinders by Transmission")
```


