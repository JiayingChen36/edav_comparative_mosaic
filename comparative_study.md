`vcd::mosaic` and `geom_mosaic` are two mostly used functions to create
mosaic plot - a way of visualizing multivariate categorical data whcih
is an area-proportional visualization of frequencies, composed of tiles
(or block) created by recursive vertical and horizontal splits of a
rectangle. There are several functions available to produce mosaic plot
including mosaicplot - a generic function from base R function, mosaic
from vcd (Visualizing Categorical Data) package, geom\_mosaic from
ggmosaic package etc.

Currently, there is limited online help or guidance to use those
functions. The existing documentation of those functions is not
user-friendly for the beginners. It has a limited number of examples and
test cases.

In the following sections, we are going to discuss a comparative study
of vcd::mosaic and geom\_mosaic. Our goal is to describe different
aspects of these functions with some user-friendly examples and detail
explanation the parameters, and finally to compare ease of use of each
of the functions.

In this study, we try to explain how to use these two functions to
produce different mosaic plots. We have elborate some of the necessary
parameters/arguments in detail that are used in the functions. As a
matter of fact, a new user will able to understand very easily what he
needs to provide to the function. Finally, we showed which function is
better in which scenario such as vcd::mosaic is better in some cases
whereas geom\_mosaic is better other cases based one the avaialbe data
and the objective.

To demonstrate different aspects of these functions, we will use the
Titanic dataset to plot different mosaic plots.

    str(Titanic)

    ##  'table' num [1:4, 1:2, 1:2, 1:2] 0 0 35 0 0 0 17 0 118 154 ...
    ##  - attr(*, "dimnames")=List of 4
    ##   ..$ Class   : chr [1:4] "1st" "2nd" "3rd" "Crew"
    ##   ..$ Sex     : chr [1:2] "Male" "Female"
    ##   ..$ Age     : chr [1:2] "Child" "Adult"
    ##   ..$ Survived: chr [1:2] "No" "Yes"

    tail(as.data.frame(Titanic))

    ##    Class    Sex   Age Survived Freq
    ## 27   3rd   Male Adult      Yes   75
    ## 28  Crew   Male Adult      Yes  192
    ## 29   1st Female Adult      Yes  140
    ## 30   2nd Female Adult      Yes   80
    ## 31   3rd Female Adult      Yes   76
    ## 32  Crew Female Adult      Yes   20

vcd::mosaic:
============

    library(vcd)

    ## Loading required package: grid

`mosaic` is one of the functions of strucplot framework which we can get
from vcd R package. The main idea of strucplot is to visualize the
tables’ cells arranged in rectangular form structable objects. The
“flattened” contingency table can be obtained using the structable()
function.

The basic Usage of mosaic functions is:

    mosaic(x, condvars = NULL,
      split_vertical = NULL, direction = NULL, spacing = NULL,
      spacing_args = list(), gp = NULL, expected = NULL, shade = NULL,
      highlighting = NULL, highlighting_fill = grey.colors, highlighting_direction = NULL,
      zero_size = 0.5, zero_split = FALSE, zero_shade = NULL,
      zero_gp = gpar(col = 0), panel = NULL, main = NULL, sub = NULL, ...)

    OR

    mosaic(formula, data, highlighting = NULL,
      ..., main = NULL, sub = NULL, subset = NULL, na.action = NULL)

Some of the basic parameters/arguments are described in the following
section:

**x:**  
a contingency table in array form, with optional category labels
specified in the dimnames(x) attribute, or an object of class
“structable”.

x is a table or formula. A structable can also be deduced from data
frame or contengency table using formula. For example,

    HEC <- structable(Survived ~ Sex + Age, data = Titanic)
    HEC

    ##              Survived   No  Yes
    ## Sex    Age                     
    ## Male   Child            35   29
    ##        Adult          1329  338
    ## Female Child            17   28
    ##        Adult           109  316

**formula: **

a formula specifying the variables used to create a contingency table
from data. For convenience, conditioning formulas can be specified; the
conditioning variables will then be used first for splitting. If any, a
specified response variable will be highlighted in the cells.

For example, If we want to split the plot by 1 varialbe, we can specify
as ~V1. If V2 is dependent on V1 and V1 needs to be highlighted, we can
do V2 ~ V1.

In the following example, a mosaic plot is crated from Titanic data:

    mosaic(Survived ~ Sex + Age, data = Titanic,
           main = "Survival on the Titanic")

![](comparative_study_files/figure-markdown_strict/unnamed-chunk-6-1.png)
In the above plot, the data is partinioned by sex first and then age;
after that survival is used to show the dependency.

    mosaic(Survived ~ ., data = Titanic)

![](comparative_study_files/figure-markdown_strict/unnamed-chunk-7-1.png)

This type of formuala is used when one variable is needed to be
highlighted based on rest of the varialbes. Survived is highlihgted
based on all otther dependend variables.

**data:**  
either a data frame, or an object of class “table” or “ftable”. If we
provide, structable contingency table, we do not need to provide data
(df or table). If data is in a data frame, the count column must be
called Freq.

**direction:**  
character vector of length k, where k is the number of margins of x
(values are recycled as needed). For each component, a value of “h”
indicates that the tile(s) of the corresponding dimension should be
split horizontally, whereas “v” indicates vertical split(s).

By default, vcd::mosaic() splits horizontally from left for the first
variable (e.g. ~ V1, ~ V1 + V2, ~ V2 | V1 or V2 ~ V1 – here V1 is
considered as first variable) and vertically from top for the second
variable and so on. If we want to alter this direction, we can provide
this “direction” parameter. For example,

     mosaic(Survived ~ Sex + Age, data = Titanic, 
            main = "Survival on the Titanic",direction=c("v","v","h"))

![](comparative_study_files/figure-markdown_strict/unnamed-chunk-8-1.png)

    # Sex = Vertical, Age = Vertical, Survived = Horizonal

In the above plot, the sex variable is used to split the plot
vertically, then age variable is used to sub-devide verticaly and then
survived sub-division is highlighted horizontally from the other side.

**gp:** object of class “gpar”, shading function or a corresponding
generating function (see details and shadings). Components of “gpar”
objects are recycled as needed along the last splitting dimension.
Ignored if shade = FALSE.

if gp is not provided, the default gray color is applied with shade =
TRUE

    mosaic(Survived~ Sex + Age, data = Titanic,
           main = "Survival on the Titanic", shade = TRUE,
           direction=c("v","v","h"))

![](comparative_study_files/figure-markdown_strict/unnamed-chunk-9-1.png)

if gp is provided with shade = TRUE (or no shade parameter but not shade
= FALSE), the provided color along with other graphical paramters (if
provided any) will apply.

    mosaic(Survived~ Sex + Age, data = Titanic,
           main = "Survival on the Titanic", shade = TRUE,
           direction=c("v","v","h"), gp = gpar(fill=c('lightblue', 'pink'))
                     )

![](comparative_study_files/figure-markdown_strict/unnamed-chunk-10-1.png)

**shade:**  
logical specifying whether gp should be used or not (see gp). If TRUE
and expected is unspecified, a default model is fitted: if condvars (see
strucplot) is specified, a corresponding conditional independence model,
and else the total independence model.

If we provide shade = FALSE, the gp will not apply at all.

    mosaic(Survived~ Sex + Age, data = Titanic,
           main = "Survival on the Titanic", shade = FALSE,
           direction=c("v","v","h"), gp = gpar(fill=c('lightblue', 'pink'))
                     )

    ## Warning in strucplot(x, condvars = if (is.null(condvars)) NULL else
    ## length(condvars), : gp parameter ignored since shade = FALSE

![](comparative_study_files/figure-markdown_strict/unnamed-chunk-11-1.png)

geom\_mosaic:
=============

`geom_mosaic` is similar to `vcd::mosaic`, which is also designed to
display the relationship between categorical variables. `geom_mosaic`
can be imported from ggmosaic package which is integrated in ggplot2 as
a geom and allows for facetting and layering.

    library(ggmosaic)
    library(gridExtra)

We are also using the same Titanic dataset. But, here will convert the
dataset from contengency table to data frame to easy to use.

    Titanic_df<-as.data.frame(Titanic)

The basic usage or parameters of geom\_mosaic is:

    geom_mosaic(mapping = NULL, data = NULL, stat = "mosaic", 
                position = "identity", conds=NULL, na.rm = FALSE, 
                divider = mosaic(), offset = 0.01, show.legend = NA, 
                inherit.aes = FALSE, ...)

The following is the description of parameters:

**mapping:** In geom\_mosaic, this is used to specify categorical
varaibles and numeric variable to be displayed. The basic parameters of
mapping is

    aes(weight=NULL, x=NULL, fill=NULL, conds=NULL)

Aesthetics contains the arugments of weight, x, fill and conds.

-   **weight:** select a weighting variable, it can be frequency,
    relative frequency or other similar weighting variables, but it must
    be numeric. In mosaic graph, this parameter is used to show the area
    of each splitting rectangle.

-   **x:** select variables to add to formula, which is used to assign
    categorical varaibles. The basic parameters of x is:

<!-- -->

    x=product(col_1, col_2, col_3,...)

For example,

    ggplot(data = Titanic_df) +
      geom_mosaic(aes(weight = Freq, x = product(Survived, Age, Sex)))

![](comparative_study_files/figure-markdown_strict/unnamed-chunk-17-1.png)

The data is partinioned by Sex first, then Age, and finally Survived.

-   **fill:** select a variable to be filled, it is used to color each
    rectangle in different colors, which is same to splitting each
    rectangle into two smaller rectangle and then coloring each smaller
    rectangle in the same parent rectangle in different colors. For
    example,

<!-- -->

    ggplot(data = Titanic_df) +
      geom_mosaic(aes(weight = Freq, x = product( Age, Sex), fill=Survived))

![](comparative_study_files/figure-markdown_strict/unnamed-chunk-18-1.png)

-   **conds:** select variables to condition on, which will be used
    first for splitting. The basic parameters of conds is

<!-- -->

    conds=product(col_1, col_2, ...)

For example,

    ggplot(data = Titanic_df) +
      geom_mosaic(aes(weight = Freq, x = product(Age, Sex), 
                      conds=product(Class, Survived),fill=Survived))

![](comparative_study_files/figure-markdown_strict/unnamed-chunk-20-1.png)

In this example, the data is partitioned by variables in conds first,
(Survived, then Class), and then by variables in x.

When variables in “product”, then the order is always from the right
most varible to the left most variable, not only in x but also in conds.

And geom\_mosaic is a little different to other geom\_\* functions, we
don’t need to and also can’t specific y. In geom\_mosaic, there is no y
value.

**order of split**

    ggplot(data = Titanic_df) +
      geom_mosaic(aes(weight = Freq, x = product(Survived, Age, Sex), fill=Class))

![](comparative_study_files/figure-markdown_strict/unnamed-chunk-21-1.png)

In this example, the order of partition is Sex, Age, Survived, and then
each rectangle splitted by Sex, Age and Survived will also be
partitioned by Class into 4 smaller rectanglers and then color these
smaller ones into four different colors. The most important thing for
this parameter is that the direction of splitting depends on the number
of patameters in x. In the above example, the number of patameters in x
is 3, which is odd, then the data is partitioned by Class horizontally.
While when the number of patameters in x is even, for example, if it is
2, we can see the direction is now vertical.

    ggplot(data = Titanic_df) +
      geom_mosaic(aes(weight = Freq, x = product(Age, Sex), fill=Class))

![](comparative_study_files/figure-markdown_strict/unnamed-chunk-22-1.png)

**data:** Only data frame can be used, no “table” or “ftable”. If we
want to use an object of “table”, we must transfer it into dataframe.

    #Titanic <- as.data.frame(Titanic)

And we can also put data in the function of ggplot.

**na.rm:** The default is “FALSE”, which means it will not remove “NA”;
while when it equals to “TURE”, it will remove missing values.

**divider:** We have two methods to set divider.

The first one is mosaic().The default of directions is first vertical
and then horizontal, repeatedly. And we can also use divider to reset
the directions of splitting. For example,

    ggplot(data = Titanic_df) +
      geom_mosaic(aes(weight = Freq, x = product(Survived, Age, Sex),
                      fill=Survived), divider = mosaic("v"))

![](comparative_study_files/figure-markdown_strict/unnamed-chunk-24-1.png)
The default parameter is “horizontal”(or “h”), the splitting directions
is v-h-v-h-…; while when using “vertical”(or “v”), the directions will
become h-v-h-v-…. In other words, it is transversed, varaibles in y-axis
will be in x-axis, and varaibles in x-axis will be in y-axis.

The second one is setting divider directly, hspine, vspine, vbar or
hbar. But in this method, the data can only be splitted by one
categorical variable. Using hspine or vspine, it is same to the original
mosaic graph, which using vbar or hbar, it will be like bar chart. For
example,

    p1<-ggplot(data = Titanic_df) +
      geom_mosaic(aes(weight = Freq, x = product(Sex)), divider="hspine")+ggtitle("Hspine")
    p2<-ggplot(data = Titanic_df) +
      geom_mosaic(aes(weight = Freq, x = product(Sex)), divider="hbar")+ggtitle("Hbar")
    grid.arrange(p1, p2, nrow = 1)

![](comparative_study_files/figure-markdown_strict/unnamed-chunk-25-1.png)

**offset:** Set the space between the first spine, but at the same time,
the space between other spines also will change in equal proportion. For
example,

    g1<-ggplot(data = Titanic_df) +
      geom_mosaic(aes(weight = Freq, x = product(Survived, Age, Sex),fill=Survived),offset = 0.01)+ggtitle("Offset=0.01")
    g2<-ggplot(data = Titanic_df) +
      geom_mosaic(aes(weight = Freq, x = product(Survived, Age, Sex),fill=Survived),offset = 0.05)+ggtitle("Offset=0.1")
    grid.arrange(g1, g2, nrow = 1)

![](comparative_study_files/figure-markdown_strict/unnamed-chunk-26-1.png)

Besides, geom\_mosaic also has other similar features of ggplot, like it
can use facet\_\* to facet. For example,

    ggplot(data = Titanic_df) +
      geom_mosaic(aes(weight = Freq, x = product(Survived, Age, Sex), fill=Survived))+facet_grid(Class~.)

![](comparative_study_files/figure-markdown_strict/unnamed-chunk-27-1.png)

vcd::mosaic vs geom\_mosaic – which one is better?
--------------------------------------------------

-   **Type of data** `geom_mosaic` can only use data frame as its `data`
    argument, however the type of data in `vcd::mosaic` can be data
    frame or table.

-   **Handling variables used in partition**

`geom_mosaic`, in fact ggplot2, is not capable of handling a different
number of variables in aesthetics. `product`, a wrapper function for a
list, is used as a work around for this.

However, this work around leads to issues with the labeling; those can
be fixed manually though. On the other hand,`formualla` of `vcd::mosaic`
is capable to handling a number of spliting variables and labeling is
also very easy in this case.

-   **Use of different layer including facatting**

Since `geom_mosaic` is a faction of ggplot2, it inherits the layered
mechanisom. So, we can plot mosaic with other plots. We can also use
other grouping tehcnique like facetting.

-   **Order of splits** `vcd::mosaic` is much easier to control the
    order of split than `geom_mosaic`. `vcd::mosaic` uses formula
    (e.g. V3 ~ V1 + V2) to set the split order which has a clear
    explaination of the position of the variables. On the other hand,
    `geom_mosaic` uses odd or even number of variables to decide the
    split. If the number of patameters is odd, the data is partitioned
    by the last dependent variable horizontally.

-   **Direction of splits**

`vcd::mosaic` has a `direction` argument which directs split directions
whether the varialbe should be split horizontally or vertically.
`geom_mosaic` has default direction to split the product variables. The
default of directions is first vertical and then horizontal, repeatedly.
`divider` is used to reset the directions of splitting; but different
combination of splitting direction with varialbe is very challenging.

-   **Color** In the use of color in different places (e.g. fill,
    border), both functions have straightforward structure.
    `geom_mosaic` also give a auto legand facility in the use of color.

-   **offset** Setting offset and spacing, `geom_mosaic` is very use to
    work with.

In conclusion, it’s easier to use `vcd::mosaic`, especially when setting
the arguments of categorial variables and their splitting directions.
What’s more, `vcd::mosaic` can also display the relationship between
categorical variables more clearly. But `geom_mosaic` is integrated in
ggplot2 as a geom which allows for facetting, layering, setting offset
and so on.

**Reference:**

1.  <a href="https://pdfs.semanticscholar.org/2416/2c1a46669f94356854176ece8548bf7fb989.pdf" class="uri">https://pdfs.semanticscholar.org/2416/2c1a46669f94356854176ece8548bf7fb989.pdf</a>

2.  <a href="https://ncss-wpengine.netdna-ssl.com/wp-content/themes/ncss/pdf/Procedures/NCSS/Mosaic_Plots.pdf" class="uri">https://ncss-wpengine.netdna-ssl.com/wp-content/themes/ncss/pdf/Procedures/NCSS/Mosaic_Plots.pdf</a>

3.  <a href="https://en.wikipedia.org/wiki/Mosaic_plot" class="uri">https://en.wikipedia.org/wiki/Mosaic_plot</a>

4.  <a href="http://www.pmean.com/definitions/mosaic.htm" class="uri">http://www.pmean.com/definitions/mosaic.htm</a>

5.  <a href="https://cran.r-project.org/web/packages/ggmosaic/vignettes/ggmosaic.html" class="uri">https://cran.r-project.org/web/packages/ggmosaic/vignettes/ggmosaic.html</a>
