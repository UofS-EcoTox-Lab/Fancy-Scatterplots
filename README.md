# Fancy-Scatterplots
Here I show how to use base R to make a scatterplot with all of the data, highlighting a subset, and adding group means.

#### Load the packages we'll need for this

``
library(scales)     # Adding transparency to plots
library(tidyverse)  # Enter the tidyverse
``

#### I like to start with a clean workspace

`rm(list = ls())     # To start with a clean workspace`

#### Generate some random data to work with

```````````````
# x data
set.seed(42)                          # Set seed to get reproducible result
x.dat <- sample(x = seq(0,1,0.0001), 
                size = 100, 
                replace = T)          # Sample 100 numbers from 0 to 1 with replacement
# y data
set.seed(86)                          # Set seed to get reproducible result
y.dat <- sample(x = seq(0,1,0.0001), 
                size = 100, 
                replace = T)          # Sample 100 numbers from 0 to 1 with replacement
# Combine x and y into a dataframe, add groups column
data.df <- cbind.data.frame(x = x.dat,
                            y = y.dat,
                            groups = rep(c("A","B","C","D"), each = 25))
```````````````

#### Calculate the mean for each group

```
data.means <- data.df %>%
  group_by(groups) %>%
  summarise_all(list(mean = mean))
```

#### Make a scatter plot of the data, highlighting the A group, and include the group means as letters

First, plot the whole dataset

`````````````
par(mar = c(3.5,3.5,1,1))         # Set the plot margins

# Plot all the data
plot(y ~ x,                                               # Plot y versus x
     data.df,                                             # Specify which dataframe to pull from
     axes = F,                                            # Exclude axes as we'll add those manually
     xlab = "",                                           # Exclude x-axis headings, we'll add those manually
     ylab = "",                                           # Exclude x=y-axis headings, we'll add those manually 
     pch = 16,                                            # pch = 16 is filled circles
     col = alpha("gray50", 0.3))                          # Gray points, 70% transparent
box()                                                     # Add a box around the plot
mtext("Pirates of the Caribbean", side = 1, line = 2.2)   # Label the x-axis
mtext("Pilates in the Caribbean", side = 2, line = 2.2)   # Label the y-axis
`````````````

Now highlight the points for group A

````
points(data.df$x[data.df$groups == "A"], 
       data.df$y[data.df$groups == "A"], 
       pch = 16, 
       col = "blue")
````

Plot the group means as their respective labels

```
text(data.means$x_mean, data.means$y_mean, data.means$groups, font = 2)
axis(side = 1); axis(side = 2)
```
