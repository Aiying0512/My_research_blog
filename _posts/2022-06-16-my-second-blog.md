---
title: "Second blog"
date: 2022-06-16
---



## How to add p-values on a Boxplot using R

I learned from these two posts:

http://www.sthda.com/english/articles/24-ggpubr-publication-ready-plots/76-add-p-values-and-significance-levels-to-ggplots/

https://cran.r-project.org/web/packages/ggprism/vignettes/pvalues.html


### Requried packages
```
library(ggplot2)
library(ggpubr)
```
Demo
```
data("ToothGrowth")
compare_means(len ~ supp, data = ToothGrowth)
p <- ggboxplot(ToothGrowth, x = "supp", y = "len",
               color = "supp", palette = "jco",
               add = "jitter")
#  Add p-value
p + stat_compare_means()
# Change method
p + stat_compare_means(method = "t.test")
# Multiple groups: Global test and pairewise comparison
my_comparisons <- list( c("0.5", "1"), c("1", "2"), c("0.5", "2") )
ggboxplot(ToothGrowth, x = "dose", y = "len",
          color = "dose", palette = "jco")+ 
  stat_compare_means(comparisons = my_comparisons, label.y = c(29, 35, 40))+ #label.y specifies the precise y location of bars
  stat_compare_means(label.y = 45)
```
