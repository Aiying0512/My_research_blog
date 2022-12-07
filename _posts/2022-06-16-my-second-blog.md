---
title: "How to add p-values on a Boxplot using R"
date: 2022-06-16
---

Sometimes, just a boxplot cannot illustrate the significant difference between groups. If we could add a p-value on the plot, then things will become more clear.

I learned from these two posts:

http://www.sthda.com/english/articles/24-ggpubr-publication-ready-plots/76-add-p-values-and-significance-levels-to-ggplots/

https://cran.r-project.org/web/packages/ggprism/vignettes/pvalues.html


#### Requried packages
```
library(ggplot2)
library(ggpubr)
```
Demo
```
data("ToothGrowth")
compare_means(len ~ supp, data = ToothGrowth)
# Boxplot
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

### Add calculated p-values
In some cases, the testing method one needs is not provided in the R package. One can first calculate the p-value and then added it to the plot. 

#### Required packages
```
library(ggplot2)
library(ggprism)
library(patchwork)
library(magrittr)
```

Need to do: Form the data frame with p-values in required format 

```
str(sleep)

p <- ggplot(sleep, aes(x = group, y = extra)) +
  geom_jitter(aes(shape = group), width = 0.1) + 
  stat_summary(geom = "crossbar", fun = mean, colour = "red", width = 0.2) + 
  theme_prism() + 
  theme(legend.position = "none")
p

# perform a t-test and obtain the p-value
result <- t.test(extra ~ group, data = sleep)$p.value
result <- signif(result, digits = 3)
result

df_p_val <- data.frame(
  group1 = "1",
  group2 = "2",
  label = result,
  y.position = 6
)

# add p-value brackets
p1 <- p + add_pvalue(df_p_val,
                     xmin = "group1",
                     xmax = "group2",
                     label = "label",
                     y.position = "y.position") 
p1
```



Use the ToothGrowth data as a Demo

```
# compare means again reference
result1 <- t.test(len ~ dose, 
                  data = subset(ToothGrowth, dose %in% c(0.5, 1.0)))$p.value
result2 <- t.test(len ~ dose, 
                  data = subset(ToothGrowth, dose %in% c(0.5, 2.0)))$p.value

# Benjamini-Hochberg correction for multiple testing
result <- p.adjust(c(result1, result2), method = "BH")
# don't need group2 column (i.e. xmax)
# instead just specify x position in the same way as y.position
df_p_val <- data.frame(
  group1 = c(0.5, 0.5),
  group2 = c(1, 2),
  x = c(2, 3),
  label = signif(result, digits = 3),
  y.position = c(35, 35)
)

p1 <- p + add_pvalue(df_p_val, 
                     xmin = "group1", 
                     x = "x", 
                     label = "label",
                     y.position = "y.position")

p2 <- p + add_pvalue(df_p_val, 
                     xmin = "group1", 
                     x = "x", 
                     label = "p = {label}",
                     y.position = "y.position",
                     label.size = 3.2,
                     fontface = "bold")

p1 + p2

```

The required matching columns from the generated data frame and the add_pvalue() function are **xmin**, **xmax** (to generate brackets position on x-axis) or **x** (indicate the position of p-values on x-axis), **y.position** (indicate the position of p-values on y-axis), **label** (i.e., p-values in this case).

