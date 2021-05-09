Microarray Gene Expression Analysis with R
================

## Loading

First, we are going to load the dataset from the `.tsv` file into `R` as
a variable called `data` using the
[`read.table`](http://www.inside-r.org/r-doc/utils/read.table) function.
<br> `data` is just an arbitrary **variable** name to hold the result of
`read.table` and it can be called/named *almost* anything.

``` r
# Load the data from a file into a variable
data = read.table("https://raw.githubusercontent.com/ahmedmoustafa/Notebooks/master/Irf6/Irf6.tsv", header = TRUE, row.names = 1)

# Convert the data.frame (table) in a matrix (numeric)
data = as.matrix(data)
```

**Note:** the hash sign (`#`) indicates that what comes after is a
*comment*. Comments are for documentation and readability of the `R`
code and they are not evaluated (or executed).

## Checking

``` r
dim(data) # Dimension of the dataset
```

    ## [1] 45101     6

``` r
head(data) # First few rows
```

|                |     KO1 |     KO2 |     KO3 |     WT1 |     WT2 |     WT3 |
|:---------------|--------:|--------:|--------:|--------:|--------:|--------:|
| 1415670\_at    |  6531.0 |  5562.8 |  6822.4 |  7732.1 |  7191.2 |  7551.9 |
| 1415671\_at    | 11486.3 | 10542.7 | 10641.4 | 10408.2 |  9484.5 |  7650.2 |
| 1415672\_at    | 14339.2 | 13526.1 | 14444.7 | 12936.6 | 13841.7 | 13285.7 |
| 1415673\_at    |  3156.8 |  2219.5 |  3264.4 |  2374.2 |  2201.8 |  2525.3 |
| 1415674\_a\_at |  4002.0 |  3306.9 |  3777.0 |  3760.6 |  3137.0 |  2911.5 |
| 1415675\_at    |  3468.4 |  3347.4 |  3332.9 |  3073.5 |  3046.0 |  2914.4 |

## Number of Genes and IDs

``` r
number_of_genes = nrow(data) # number of genes = number of rows
number_of_genes
```

    ## [1] 45101

``` r
ids = row.names(data) # The ids of the genes are the names of the rows
head(ids)
```

    ## [1] "1415670_at"   "1415671_at"   "1415672_at"   "1415673_at"   "1415674_a_at"
    ## [6] "1415675_at"

## Exploring

Check the behavior of the data (e.g., normal?, skewed?)

``` r
hist(data, col = "gray", main="Histogram")
```

![](Irf6_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

## Transforming

*l**o**g*<sub>2</sub> transformation (why?)

``` r
data2 = log2(data)
hist(data2, col = "gray")
```

![](Irf6_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

## Boxplot

``` r
colors = c(rep("navy", 3), rep("orange", 3))
boxplot(data2, col = colors)
```

![](Irf6_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

## Clustering 1/2

Hierarchical clustering of the **samples** (i.e., columns) based on the
[correlation
coefficients](http://en.wikipedia.org/wiki/Pearson_product-moment_correlation_coefficient)
of the expression values

``` r
hc = hclust(as.dist(1 - cor(data2)))
plot(hc)
```

![](Irf6_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

To learn more about a function (e.g., `hclust`), you may type
`?function` (e.g., `?hclust`) in the `console` to launch `R`
documentation on that function:

## Splitting Data Matrix into two tables

``` r
ko = data2[, 1:3] # KO matrix
head(ko)
```

|                |      KO1 |      KO2 |      KO3 |
|:---------------|---------:|---------:|---------:|
| 1415670\_at    | 12.67309 | 12.44160 | 12.73606 |
| 1415671\_at    | 13.48763 | 13.36396 | 13.37740 |
| 1415672\_at    | 13.80768 | 13.72346 | 13.81825 |
| 1415673\_at    | 11.62425 | 11.11602 | 11.67260 |
| 1415674\_a\_at | 11.96651 | 11.69126 | 11.88303 |
| 1415675\_at    | 11.76005 | 11.70883 | 11.70256 |

``` r
wt = data2[, 4:6] # WT matrix
head(wt)
```

|                |      WT1 |      WT2 |      WT3 |
|:---------------|---------:|---------:|---------:|
| 1415670\_at    | 12.91664 | 12.81202 | 12.88262 |
| 1415671\_at    | 13.34543 | 13.21136 | 12.90128 |
| 1415672\_at    | 13.65917 | 13.75673 | 13.69759 |
| 1415673\_at    | 11.21323 | 11.10447 | 11.30224 |
| 1415674\_a\_at | 11.87675 | 11.61517 | 11.50755 |
| 1415675\_at    | 11.58567 | 11.57270 | 11.50898 |

## Gene (Row) Mean Expression

``` r
# Compute the means of the KO samples
ko.means = rowMeans(ko)
head(ko.means)
```

    ##   1415670_at   1415671_at   1415672_at   1415673_at 1415674_a_at   1415675_at 
    ##     12.61692     13.40966     13.78313     11.47096     11.84693     11.72381

``` r
# Compute the means of the WT samples
wt.means = rowMeans(wt)
head(wt.means)
```

    ##   1415670_at   1415671_at   1415672_at   1415673_at 1415674_a_at   1415675_at 
    ##     12.87043     13.15269     13.70450     11.20664     11.66649     11.55578

## Scatter

### Between the means

``` r
plot(ko.means ~ wt.means) # The actual scatter plot
abline(0, 1, col = "red") # Only a diagonal line
```

![](Irf6_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

### All by all

``` r
pairs(data2) # All pairwise comparisons
```

![](Irf6_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

## Differentially Expressed Genes (DEGs)

To identify DEGs, we will identify:

-   **Biologically** significantly differentially expressed
-   **Statistically** significantly differentially expressed

Then, we will take the **overlap** (**intersection**) of the two sets

![](images/intersection.png)

## Biological Significance (fold-change)

``` r
fold = ko.means - wt.means # Difference between means
head(fold)
```

    ##   1415670_at   1415671_at   1415672_at   1415673_at 1415674_a_at   1415675_at 
    ##  -0.25351267   0.25697097   0.07863227   0.26431191   0.18044345   0.16803065

-   What do the positive and negative values of the fold-change
    indicate? Considering the `WT` condition is the **reference** (or
    **control**)

-   **+ve** fold-change → **Up**-regulation ↑

-   **-ve** fold-change → **Down**-regulation ↓

``` r
hist(fold, col = "gray") # Histogram of the fold
```

![](Irf6_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

## Statistical Significance (*p*-value)

-   To assess the statistical significance of the difference in the
    expression values for each gene between the two conditions (e.g.,
    `WT` and `KO`), we are going to use
    [*t*-test](http://en.wikipedia.org/wiki/Student%27s_t-test).

![](Irf6_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

## *t*-test

Let’s say there are two samples *x* and *y* from the two populations,
*X* and *Y*, respectively, to determine whether the means of two
populations are significantly different, we can use `t.test`.

``` r
?t.test
```

    ## Student's t-Test
    ## 
    ## Description:
    ## 
    ##      Performs one and two sample t-tests on vectors of data.
    ## 
    ## Usage:
    ## 
    ##      t.test(x, ...)
    ##      
    ##      ## Default S3 method:
    ##      t.test(x, y = NULL,
    ##             alternative = c("two.sided", "less", "greater"),
    ##             mu = 0, paired = FALSE, var.equal = FALSE,
    ##             conf.level = 0.95, ...)
    ##      
    ##      ## S3 method for class 'formula'
    ##      t.test(formula, data, subset, na.action, ...)
    ##      
    ## Arguments:
    ## 
    ##        x: a (non-empty) numeric vector of data values.
    ## 
    ##        y: an optional (non-empty) numeric vector of data values.
    ## 
    ## alternative: a character string specifying the alternative hypothesis,
    ##           must be one of '"two.sided"' (default), '"greater"' or
    ##           '"less"'.  You can specify just the initial letter.
    ## 
    ##       mu: a number indicating the true value of the mean (or difference
    ##           in means if you are performing a two sample test).
    ## 
    ##   paired: a logical indicating whether you want a paired t-test.
    ## 
    ## var.equal: a logical variable indicating whether to treat the two
    ##           variances as being equal. If 'TRUE' then the pooled variance
    ##           is used to estimate the variance otherwise the Welch (or
    ##           Satterthwaite) approximation to the degrees of freedom is
    ##           used.
    ## 
    ## conf.level: confidence level of the interval.
    ## 
    ##  formula: a formula of the form 'lhs ~ rhs' where 'lhs' is a numeric
    ##           variable giving the data values and 'rhs' either '1' for a
    ##           one-sample or paired test or a factor with two levels giving
    ##           the corresponding groups. If 'lhs' is of class '"Pair"' and
    ##           'rhs' is '1', a paired test is done
    ## 
    ##     data: an optional matrix or data frame (or similar: see
    ##           'model.frame') containing the variables in the formula
    ##           'formula'.  By default the variables are taken from
    ##           'environment(formula)'.
    ## 
    ##   subset: an optional vector specifying a subset of observations to be
    ##           used.
    ## 
    ## na.action: a function which indicates what should happen when the data
    ##           contain 'NA's.  Defaults to 'getOption("na.action")'.
    ## 
    ##      ...: further arguments to be passed to or from methods.
    ## 
    ## Details:
    ## 
    ##      'alternative = "greater"' is the alternative that 'x' has a larger
    ##      mean than 'y'. For the one-sample case: that the mean is positive.
    ## 
    ##      If 'paired' is 'TRUE' then both 'x' and 'y' must be specified and
    ##      they must be the same length.  Missing values are silently removed
    ##      (in pairs if 'paired' is 'TRUE').  If 'var.equal' is 'TRUE' then
    ##      the pooled estimate of the variance is used.  By default, if
    ##      'var.equal' is 'FALSE' then the variance is estimated separately
    ##      for both groups and the Welch modification to the degrees of
    ##      freedom is used.
    ## 
    ##      If the input data are effectively constant (compared to the larger
    ##      of the two means) an error is generated.
    ## 
    ## Value:
    ## 
    ##      A list with class '"htest"' containing the following components:
    ## 
    ## statistic: the value of the t-statistic.
    ## 
    ## parameter: the degrees of freedom for the t-statistic.
    ## 
    ##  p.value: the p-value for the test.
    ## 
    ## conf.int: a confidence interval for the mean appropriate to the
    ##           specified alternative hypothesis.
    ## 
    ## estimate: the estimated mean or difference in means depending on
    ##           whether it was a one-sample test or a two-sample test.
    ## 
    ## null.value: the specified hypothesized value of the mean or mean
    ##           difference depending on whether it was a one-sample test or a
    ##           two-sample test.
    ## 
    ##   stderr: the standard error of the mean (difference), used as
    ##           denominator in the t-statistic formula.
    ## 
    ## alternative: a character string describing the alternative hypothesis.
    ## 
    ##   method: a character string indicating what type of t-test was
    ##           performed.
    ## 
    ## data.name: a character string giving the name(s) of the data.
    ## 
    ## See Also:
    ## 
    ##      'prop.test'
    ## 
    ## Examples:
    ## 
    ##      require(graphics)
    ##      
    ##      t.test(1:10, y = c(7:20))      # P = .00001855
    ##      t.test(1:10, y = c(7:20, 200)) # P = .1245    -- NOT significant anymore
    ##      
    ##      ## Classical example: Student's sleep data
    ##      plot(extra ~ group, data = sleep)
    ##      ## Traditional interface
    ##      with(sleep, t.test(extra[group == 1], extra[group == 2]))
    ##      
    ##      ## Formula interface
    ##      t.test(extra ~ group, data = sleep)
    ##      
    ##      ## Formula interface to one-sample test
    ##      t.test(extra ~ 1, data = sleep)
    ##      
    ##      ## Formula interface to paired test
    ##      ## The sleep data are actually paired, so could have been in wide format:
    ##      sleep2 <- reshape(sleep, direction = "wide", 
    ##                        idvar = "ID", timevar = "group")
    ##      t.test(Pair(extra.1, extra.2) ~ 1, data = sleep2)

## *t*-test : Example 1

``` r
x = c(4, 3, 10, 7, 9) ; y = c(7, 4, 3, 8, 10)
t.test(x, y)
```

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  x and y
    ## t = 0.1066, df = 7.9743, p-value = 0.9177
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  -4.12888  4.52888
    ## sample estimates:
    ## mean of x mean of y 
    ##       6.6       6.4

``` r
t.test(x, y)$p.value
```

    ## [1] 0.917739

## *t*-test : Example 2

``` r
x = c(6, 8, 10, 7, 9) ; y = c(3, 2, 1, 4, 5)
t.test(x, y)
```

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  x and y
    ## t = 5, df = 8, p-value = 0.001053
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  2.693996 7.306004
    ## sample estimates:
    ## mean of x mean of y 
    ##         8         3

``` r
t.test(x, y)$p.value
```

    ## [1] 0.001052826

Let’s compute the *p*-value for all genes using a `for`-loop of
`t.test`, one gene at a time:

``` r
pvalue = NULL # Empty list for the p-values

for(i in 1 : number_of_genes) { # for each gene from to the number of genes
  x = wt[i, ] # wt values of gene number i
  y = ko[i, ] # ko values of gene number i
  t = t.test(x, y) # t-test between the two conditions
  pvalue[i] = t$p.value # Store p-value number i into the list of p-values
}
head(pvalue)
```

    ## [1] 0.092706280 0.182663337 0.129779075 0.272899180 0.262377176 0.005947807

``` r
hist(-log10(pvalue), col = "gray") # Histogram of p-values (-log10)
```

![](Irf6_files/figure-gfm/unnamed-chunk-22-1.png)<!-- -->

## Volcano

``` r
plot(-log10(pvalue) ~ fold)
```

![](Irf6_files/figure-gfm/unnamed-chunk-23-1.png)<!-- -->

``` r
fold_cutoff = 2
pvalue_cutoff = 0.01

plot(-log10(pvalue) ~ fold)

abline(v = fold_cutoff, col = "blue", lwd = 3)
abline(v = -fold_cutoff, col = "red", lwd = 3)
abline(h = -log10(pvalue_cutoff), col = "green", lwd = 3)
```

![](Irf6_files/figure-gfm/unnamed-chunk-25-1.png)<!-- -->

## Filtering for DEGs

``` r
filter_by_fold = abs(fold) >= fold_cutoff # Biological
sum(filter_by_fold) # Number of genes staisfy the condition
```

    ## [1] 1051

``` r
filter_by_pvalue = pvalue <= pvalue_cutoff # Statistical
sum(filter_by_pvalue)
```

    ## [1] 1564

``` r
filter_combined = filter_by_fold & filter_by_pvalue # Combined
sum(filter_combined)
```

    ## [1] 276

``` r
filtered = data2[filter_combined, ]
dim(filtered)
```

    ## [1] 276   6

``` r
head(filtered)
```

|                |       KO1 |       KO2 |       KO3 |      WT1 |       WT2 |       WT3 |
|:---------------|----------:|----------:|----------:|---------:|----------:|----------:|
| 1416200\_at    | 13.312004 | 12.973357 | 12.868456 |  7.40429 |  8.558803 |  8.683696 |
| 1416236\_a\_at | 14.148397 | 14.039236 | 14.130007 | 12.23604 | 12.022403 | 11.495055 |
| 1417808\_at    |  5.321928 |  5.442944 |  4.053111 | 15.16978 | 15.070087 | 14.753274 |
| 1417932\_at    | 10.602884 | 10.257152 | 10.496055 | 13.98445 | 14.203295 | 13.720960 |
| 1418050\_at    | 10.622052 | 10.975490 | 10.795066 | 12.86513 | 13.012048 | 12.658122 |
| 1418100\_at    |  9.117903 |  8.634811 |  9.057721 | 12.90358 | 12.842449 | 12.233769 |

``` r
plot(-log10(pvalue) ~ fold)
points(-log10(pvalue[filter_combined]) ~ fold[filter_combined],
       col = "green")
```

![](Irf6_files/figure-gfm/unnamed-chunk-28-1.png)<!-- -->

## Exercise

On the volcano plot, highlight the up-regulated genes in red and the
download-regulated genes in blue

## Solution

-   Up-regulated genes

``` r
# Screen for the up-regulated genes (+ve fold)
filter_up = filter_combined & fold > 0

head(filter_up)
```

    ##   1415670_at   1415671_at   1415672_at   1415673_at 1415674_a_at   1415675_at 
    ##        FALSE        FALSE        FALSE        FALSE        FALSE        FALSE

``` r
# Number of filtered genes
sum(filter_up)
```

    ## [1] 95

-   Down-regulated genes

``` r
# Screen for the down-regulated genes (-ve fold)
filter_down = filter_combined & fold < 0

head(filter_down)
```

    ##   1415670_at   1415671_at   1415672_at   1415673_at 1415674_a_at   1415675_at 
    ##        FALSE        FALSE        FALSE        FALSE        FALSE        FALSE

``` r
# Number of filtered genes
sum(filter_down)
```

    ## [1] 181

``` r
plot(-log10(pvalue) ~ fold)
points(-log10(pvalue[filter_up]) ~ fold[filter_up], col = "red")
points(-log10(pvalue[filter_down]) ~ fold[filter_down], col = "blue")
```

![](Irf6_files/figure-gfm/unnamed-chunk-33-1.png)<!-- -->

## Heatmap

``` r
heatmap(filtered)
```

![](Irf6_files/figure-gfm/unnamed-chunk-34-1.png)<!-- -->

-   By default, `heatmap` clusters genes (rows) and samples (columns)
    based on [the Euclidean
    distance](http://en.wikipedia.org/wiki/Euclidean_distance).

-   In the context of gene expression, we need to cluster genes and
    samples based on the correlation to explore patterns of
    **[co-regulation](http://dx.doi.org/10.1186/1471-2105-5-18)**
    (**co-expression**) - *Guilt by Association*.

-   To let `heatmap` cluster the genes and/or samples, the genes and
    samples will be clustered (grouped) by correlation coefficients
    (using `cor`) among the genes and samples.

``` r
# Clustering of the columns (samples)
col_dendrogram = as.dendrogram(hclust(as.dist(1-cor(filtered))))

# Clustering of the rows (genes)
row_dendrogram = as.dendrogram(hclust(as.dist(1-cor(t(filtered)))))
```

![](images/guilt_by_association.jpg)

``` r
# Heatmap with the rows and columns clustered by correlation coefficients
heatmap(filtered, Rowv=row_dendrogram, Colv=col_dendrogram)
```

![](Irf6_files/figure-gfm/unnamed-chunk-36-1.png)<!-- -->

``` r
library(gplots) # Load the gplots library
heatmap(filtered, Rowv=row_dendrogram, Colv=col_dendrogram, col = rev(redgreen(1024)))
```

![](Irf6_files/figure-gfm/unnamed-chunk-38-1.png)<!-- -->

## Annnotation

To obtain the functional annotation of the differentially expressed
genes, we are going first to extract their probe ids:

``` r
filterd_ids = row.names(filtered) # ids of the filtered DE genes
length(filterd_ids)
```

    ## [1] 276

``` r
head(filterd_ids)
```

    ## [1] "1416200_at"   "1416236_a_at" "1417808_at"   "1417932_at"   "1418050_at"  
    ## [6] "1418100_at"

## Sanity Check (Irf6)

![Down Regulation of
Irf6](https://raw.githubusercontent.com/ahmedmoustafa/Notebooks/master/Irf6/images/irf6_down.png)

<img src="Irf6_files/figure-gfm/unnamed-chunk-40-1.png" style="display: block; margin: auto;" />

## Multiple Testing Correction

We conducted 10^{6} statistical tests. The computed *p*-values need to
be corrected for *multiple testing*. The correction can be performed
using `p.adjust`, which simply takes the orignial *p*-values a vector
and returns the adjusted (corrected) *p*-values:

``` r
adjusted.pvalues = p.adjust(pvalue, method = "fdr")
```

Number **original** *p*-values ≤ 0.05 = 5099 while the number
**adjusted** (**corrected**) *p*-values &lt; 0.05 ≥ 9

Here is an example of the original *p*-values and corresponding adjusted
*p*-values:

|    pvalue | adjusted.pvalue |
|----------:|----------------:|
| 0.0927063 |       0.5278755 |
| 0.1826633 |       0.6346918 |
| 0.1297791 |       0.5805456 |
| 0.2728992 |       0.7025472 |
| 0.2623772 |       0.6967834 |
| 0.0059478 |       0.2518079 |

![](Irf6_files/figure-gfm/unnamed-chunk-43-1.png)<!-- -->

## Homework

-   Identify the top 10 *biologically* significant genes (i.e., by
    fold-change)
-   Identify the top 10 *statistically* significant genes (i.e., by
    *p*-value)

![Funny
Genes](https://raw.githubusercontent.com/ahmedmoustafa/Notebooks/master/Irf6/images/joke.png)