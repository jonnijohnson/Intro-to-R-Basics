---
title: 'Learning R: The Basics'
knit: (function(input_file, encoding) {
  out_dir <- 'docs';
  rmarkdown::render(input_file,
 encoding=encoding,
 output_file=file.path(dirname(input_file), out_dir, 'index.html'))})
author: "Jonni Johnson"
date: "May 16, 2019"
output:
  html_document:
    keep_md: true
    highlight: pygments
    theme: readable
    toc: yes
    toc_depth: 3
    toc_float: yes
---

# Yes, you can <3 R!

**Note this top is for beginners, those familiar with R may skip down using the TOC on the side**

Many of my colleagues resist using R because, well, it is like learning another languPetal.Length. But R is so spectacular and is a great introduction to using other--and some might say more complex, programming languPetal.Lengths too (e.g., python, SQL, C++, perl). </br> 

What I like most about R is that working with data at any stage can happen under one program's roof (i.e., data entry, cleaning, analyses, graphing). 

I had a steep learning curve and often hear that time to learn R is often what stops them. I created the following tutorial that covers some introductory data exploration, analysis, and graphing-- to hopefully show others that R is not that intimidating. For the below I assume R studio is installed and you're familiar with terms such as script, console, environment, and library.

One difficulty I had was grappling with the language syntax of R-- not just the literal coding-- but the way I should _say_ things to R to get it to perform certain functions. In the tutorial below I sometimes go into explanations about how this syntax of writing in R makes sense to me. In addition to learning commands, learning the R languPetal.Length (or any computer language) requires understanding of the language's syntax and semantic structures. 

In this tutorial I use the **iris data set** as well as add some additional variables for graphing purposes and data analyses.

</br>

### packages and Libraries....

Your library is all commands you can use at the present time-- 

*NERD ALERT* I usually think of Trinity in the movie the Matrix learning how to fly the helicopter. This knowledge is _"loaded"_ into her brain and suddenly she can perform new actions! Other than wearing all black, I have nothing in common with her. But it helps me understand that the library R uses can be modified with new packages so that you can learn new commands to perform new actions 

In this way, I think of packages as special dictionaries that, if loaded into your library of other dictionaries, can reference a specific argument of the packages. It is similar to typing a formula in excel to _sum_ the numbers of a column. You simply write the argument _sum()_ and indicate within parentheses which rows to sum across. The program understands that the command _sum_, otherwise known as **an argument** or **function**, means to add and print a total. 

Loading packages essentially provide definitions of arguments, like _sum_, so that once installed and put in your library, you may use the argument.


This requires us to frequently down/update packages and load them into the library. In this tutorial there are several packages installed and loaded into the library. As such, some packages that use the same argument name (e.g., such as _'select'_) will print a warning message when loaded, notifying you of the two packages' conflict of using the same argument name. This really just means that we have to specify which package we mean when using the argument. 

For fun, think of it as loading two packages into a magical program: American English and British English-- Go with me on this-- Both packages have a _boot()_ argument, but one package, American English, will turn an object into a shoe; the other package, British English, will turn an object into the back part of a car/trunk. ...This is why I specified that the program was magical. 



...We use a lot of them. Make sure that the packages are installed first using the command function: _install.packages("Name of the package")_. packages include:

tidyverse, psych, tableone, tadaatoolbox, ggplot2, corrplot, reshape2, magrittr, Hmisc, epiDisplay 

* Some of these may not need to be installed just loaded into your library. Tadaatoolbox is just used to print nice tables in markdown documents (i.e., what you are reading). You otherwise don't need it, but it sure is _pretty!_


If I wanted to use the commands in the tidyverse package I would write:

install.packages("tidyverse")
library(tidyverse)

Repeat for each of the packages above.





### Loading in Data

Most of the time we will be loading in CSV files. In this tutorial I won't be loading data, but instead use a built in data set from R, df. 


In addition to csv files, you can read in other types of files, SPSS, TXT, using other base argument commands. You can also load data from a browser, but that includes arguments not presented below. The data file, whatever its extension (.csv, .txt, etc.), should be saved in a folder which will become your _working directory_ for the project. 

Useful commands for loading in data files include:

|  This Argument... |  ... Performs This Function                                                |
|-------------------|----------------------------------------------------------------------------|
| _getwd()_         |   Asks R to report back the folder/location it has access to               |
| _setwd()_         |  Can specify or change the working directory, the pathway is specified inside parentheses _setwd("C:\\My Example\\Example Folder")_ |                               |
| _dir()_           |  Ask R to print the names of all the files in the working directory        |
| _ls()_            |  Same as above, another way to list files in the working directory         |
| _read.csv()_      | Used to read in csv file also includes commands within to describe how the data should be handled|
|Within read.csv()  |  _sep = ","_  traditionally the csv is separated by commas, but this could be '\'' for TXT files|
|                   | _header = T_  use if the first row of data are variable names for columns, change to "F" if no header |
|                   | _stringAsFactors = F_ specfiies how R reads string/character vectors, personally I like to make my own factors, this can otherwised be changed to "T" and R will assign factor levels to strings|
|                   | _skip = 2_  Only needed if you want to skip the first X-number of lines-- in this example, 2|


Below I am asking to "get the working directory", otherwise asking, "What is my current directory?". ls() is asking for a list of the files and dir() is an alternative way to ask as well. 

I have a **#** in front of the setwd() command. This is a comment notation in R; R will not run this command and it is handy to make brief comments within your code to explain processes along the way. 

If the # symbol were not there, this command would otherwise set the working directory to the path specified in parentheses.


```r
getwd()
```

```
## [1] "C:/Users/Jonni/Desktop/R Class/Intro-to-R-Basics"
```

```r
ls()
```

```
## [1] "encoding"   "input_file" "out_dir"
```

```r
#setwd("C:\\Users\\Jonni\\Desktop\\R Class")

dir()
```

```
## [1] "docs"                    "Intro-to-R-Basics.Rproj"
## [3] "Intro to R.Rmd"          "Intro_to_R.Rmd"         
## [5] "Matching_sample.csv"     "README.md"
```

Once the data file is read, you'll want to assign it a name so that R saves it to its working environment

If loading a csv file, simply use this:


_df <- read.csv("example.csv", header = T, stringsAsFactors = F)_

header = T or TRUE when the first row entry are the names of your columns, as is genuinely what most people see. This command also view string or character variables as strings, and not as factors or categories (more on this later)

_str(df)_  # Calling for the structure of this object


This file is loaded from my local C: drive just as an example of loading in a csv file after changing the directory with the setwd() command.



The rest of this markdown I will just be working with the iris data set. For a complete list of available data sets in R, good for data reproducibility questions and learning R, type data() into the console/script.

Leaving data() with nothing in parantheses will ask for a complete list of sets R has. Naming one data set within the parantheses is the equivalent of loading it into the environment and begin working with it.



### Basic Viewing/Exploring/Cleaning Commands

|This Argument.....  | ...... Performs This Function                                            |
|--------------------|--------------------------------------------------------------------------|
| _data(iris)_       | This example is loading the data set iris, a dataset that automatically comes installed with R | | _str(iris)_            | Call for the 'str'ucture of an object, if a dataframe it return information about each column variable type, factor levels (if app), and number of variables and observations in the object/dataframe|
| _head(iris)_           | Calls for the first 6 rows or heading of the dataframe                   |
| _tail(iris)_           | Calls for the last 6 rows or tail end of data                            |
| _dim(iris)_            | Calls for the dimensions of an object-- more useful for lists, matrices, and tables|
| _class(iris)_          | Asking for how R is classifying an object (e.g., character, logic, number, integer, string, factor, double, dataframe)|
| _$_                 | This operator command can be used with the name of the dataframe to call a column of interest by name. Such as _iris$ColumnName_ |
| _View(iris)_            | Useful to "see" what data look like in a traditional excel-type format, opens Viewer window | 
| _cbind()_           |   Base R command to add column                                          |
| _rbind()_           |  Base R command to add row                                              |

Often times in this dataset, there may be unnecessary columns and rows-- perhaps used for a temporary purpose or totalling sums or means. Alternatively, data variables may need to be added.


```r
data(iris)
View(iris) #Note this is with a capital "V"
str(iris)
```

```
## 'data.frame':	150 obs. of  5 variables:
##  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
##  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
##  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
##  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
##  $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
```

We'll create a new dataframe, **'df'** that only has the data we are interested in-- it is a good habit to not overwrite the original dataset read in/loaded-- that way you can always return to see if you need any additional information/variables without having to re-read the file in.

This dataset currently has 5 variables and 150 observations. More about the types of variables in a moment~

As stated above, some times loading in data from others has unnecessary rows or columns.

For this example I'm just acting as if the row entries of observations 41 through 50 are not wanted, maybe they are comments or trials that there was an experimenter error, etc. 


```r
df <- iris[c(1:40,51:150),]  # "Create a new dataframe called 'df'; In it, take rows 1 through 40 and 51 through 150, and bring ALL columns"


str(df)
```

```
## 'data.frame':	140 obs. of  5 variables:
##  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
##  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
##  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
##  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
##  $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
```

Now we have 10 fewer observations, still same number of columns


The _[  ]_ tell R to "look here" in this referenced spot. 

Within brackets syntax are **always** [ _Rows, Columns_ ]. R tells rows from columns be detecting the ','   

Above, we use the _c()_ command to let R know that the things in parentheses are apart of the same series; therefore, R knows not to treat the comma within the parentheses as specifying the columns. By leaving an empty space after the comma and not specifying columns, we are saying "All of the columns"

Alternatively if there is only one thing within the brackets _without a comma_, R will assume it is the column position. 

So df[9] would be the equivalent of saying "Print the 9th column in dataframe df" whereas df[9,] says, "Print row 9 all columns"


Now we have a dataframe df. 

Using the _str()_ command you can see that now we have fewer observations. 

I'll add some columns to this data set. For now, I will create a vector of numbers that is the same length of our dataframe-- that is-- it will have the same number of row entries, in our case, 140 in df.


Below I'm saying, take a random sample, possible outcomes are 1,3, or 5, 140 times with replacement (i.e., if a 5 is chosen on one trial it has an equal probabilty of being chosen on the next trial). 


```r
x1 <- sample(x = c(1, 3, 5), size = 140, replace = TRUE)

x1
```

```
##   [1] 5 5 1 1 3 1 3 3 3 5 5 1 5 1 3 5 5 1 5 1 5 1 1 5 1 5 5 3 1 1 3 5 1 1 1
##  [36] 3 5 1 5 5 3 3 1 5 5 3 3 3 5 1 3 1 5 3 1 3 5 5 3 1 1 5 5 5 1 3 5 1 1 3
##  [71] 3 3 1 1 5 5 5 5 3 5 1 1 1 1 3 1 3 5 3 3 5 5 1 5 3 1 3 1 5 1 1 3 1 1 5
## [106] 5 3 5 3 1 3 5 5 1 1 5 1 3 5 3 5 1 5 3 3 3 5 3 3 1 1 1 1 5 3 5 1 3 1 1
```

```r
# Make another and call it x2

x2 <- sample(x = c(1, 3, 5), size = 140, replace = TRUE)

x2
```

```
##   [1] 1 3 5 5 1 5 5 5 1 5 1 3 5 3 1 5 1 1 3 3 5 1 5 3 1 1 5 3 5 5 1 1 1 1 3
##  [36] 3 1 5 3 3 3 1 3 5 3 1 5 1 3 3 1 3 1 3 3 1 3 1 5 3 1 1 5 1 3 3 1 5 1 3
##  [71] 3 5 3 1 1 3 5 3 3 1 3 3 5 3 1 5 1 1 3 5 5 3 3 3 1 5 1 3 5 1 3 5 5 1 1
## [106] 5 5 5 3 3 5 1 1 5 3 5 3 3 1 1 5 1 5 5 5 1 5 3 5 5 5 5 5 3 1 5 5 1 1 1
```

Let's add these variables x1 and x2 to our df using a base R command, _cbind()_. Thinking of this as "binding" information together the 'c' indicates these are columns that will be bound, but we can also bind rows together with _rbind()_


The assignment operator **' <- '** or also **' = '** means we are writing over whatever the current df is. Here it says, "Bind together the dataframe df, vector x1, and vector x2."  We can think of vectors x1 and x2 as additional variables, perhaps categories, experimental conditions, or behavioral codes from two coders. 

We otherwise can easily merge these because each is all the same _length_ or has 140 enteries 


```r
df <- cbind(df,x1,x2)


str(df)
```

```
## 'data.frame':	140 obs. of  7 variables:
##  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
##  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
##  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
##  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
##  $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ x1          : num  5 5 1 1 3 1 3 3 3 5 ...
##  $ x2          : num  1 3 5 5 1 5 5 5 1 5 ...
```

Let's act like columns x1 and x2 are behavioral codes from two coders who specified the number of bees on each flower species. Ideally, the coders would make the same observations, but we all know that isn't always the case, especially in behavioral data coding, which can be quite subjective at times.

To check that columns x1 and x2 are similar/identical we would use the _==_ operator meaning "is equal to" -- R will print a logic output saying either TRUE or FALSE, where columns 2 and 3 either match or differ, respectively. 

Below we are using the _$_ operator to individually select the columns by name.


```r
df$x1 == df$x2
```

```
##   [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE  TRUE FALSE
##  [12] FALSE  TRUE FALSE FALSE  TRUE FALSE  TRUE FALSE FALSE  TRUE  TRUE
##  [23] FALSE FALSE  TRUE FALSE  TRUE  TRUE FALSE FALSE FALSE FALSE  TRUE
##  [34]  TRUE FALSE  TRUE FALSE FALSE FALSE FALSE  TRUE FALSE FALSE  TRUE
##  [45] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE  TRUE FALSE
##  [56] FALSE FALSE FALSE FALSE FALSE  TRUE FALSE  TRUE FALSE FALSE  TRUE
##  [67] FALSE FALSE  TRUE  TRUE  TRUE FALSE FALSE  TRUE FALSE FALSE  TRUE
##  [78] FALSE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [89]  TRUE FALSE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE FALSE  TRUE
## [100]  TRUE FALSE FALSE FALSE  TRUE FALSE  TRUE FALSE  TRUE  TRUE FALSE
## [111] FALSE FALSE FALSE FALSE FALSE  TRUE FALSE  TRUE FALSE FALSE  TRUE
## [122]  TRUE  TRUE FALSE FALSE FALSE  TRUE  TRUE FALSE FALSE FALSE FALSE
## [133] FALSE FALSE FALSE  TRUE FALSE FALSE  TRUE  TRUE
```

Could also say this by index the number of each column. Examining the structure we see that x1 is column 6 and x2 is column 7:


```r
str(df)
```

```
## 'data.frame':	140 obs. of  7 variables:
##  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
##  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
##  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
##  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
##  $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ x1          : num  5 5 1 1 3 1 3 3 3 5 ...
##  $ x2          : num  1 3 5 5 1 5 5 5 1 5 ...
```

```r
table(df[6] == df[7]) # There's many falses, indicating many discrepancies
```

```
## 
## FALSE  TRUE 
##    98    42
```

If there were a larger dataset, you likely wouldn't want to eyeball. To find the row entry that doesn't match, we can use _which()_ and to print the contents of what's there we just index it using brackets. Since we know we want columns 6 and 7, we can concatenate them together and select out the entry. 

The which command lets us know if it in row 21 and uses the _!=_ symbols to indicate "Does not equal" - so the command below reads as "Which entries in columns 6 and 7 of df are not equal"


```r
which(df$x1 != df$x2)
```

```
##  [1]   1   2   3   4   5   6   7   8   9  11  12  14  15  17  19  20  23
## [18]  24  26  29  30  31  32  35  37  38  39  40  42  43  45  46  47  48
## [35]  49  50  51  52  53  55  56  57  58  59  60  62  64  65  67  68  72
## [52]  73  75  76  78  80  81  82  83  84  85  86  87  88  90  92  93  94
## [69]  95  96  97  98 101 102 103 105 107 110 111 112 113 114 115 117 119
## [86] 120 124 125 126 129 130 131 132 133 134 135 137 138
```

```r
dferrors <- df[which(df$x1 != df$x2),]
```

Here, I'm selecting out all of the data that's associated with the instances where my coders did not make the same observation. Can be useful for going over discrepencies in coding, data entry errors, etc. I'm calling this new dataframe _'dferrors'_


```r
str(dferrors) #Yikes a lot discrepencies!
```

```
## 'data.frame':	98 obs. of  7 variables:
##  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 5.4 ...
##  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.7 ...
##  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
##  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.2 ...
##  $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ x1          : num  5 5 1 1 3 1 3 3 3 5 ...
##  $ x2          : num  1 3 5 5 1 5 5 5 1 1 ...
```

Let's say we speak to our coders about their discrepencies and coder 1 states that her 5's should actually be 3's. Let's see how this would change the results

We'll again use base R commands here. Make a new df for potential errors. We need to do the comparison originally from our df, but since we are modifying the original, we may want to retain it as a cleaner copy. 

*Note using tidyverse methods (outlined later) keeps your environment cleaner of objects


We define a new dataframe, dferrors2 as a clone of df. Then say, 'In dataframe dferrors2, select out all instances where the value of variable x1 is equal to 5, in the 6th column (could also say dferrors2$x1), . 

**Remember, [rows, columns]**

You essentially are saying look for value 5 in column x1. But to define the row element, it takes the x1 specification. Since we only want this to rewrite over the values for our first coder, x1, we have to specify that this only applies to column x1 or column 6. If the 6 were omitted, then _all_ corresponding columns that had a '5' entered would be written.

The first example omits the 5 column as it is non numeric, just for demonstration only


```r
dferrors2 <- df[-5] #For demonstration purposes dropping a categorical/factor variable

str(dferrors2) #Now only 6 variables
```

```
## 'data.frame':	140 obs. of  6 variables:
##  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
##  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
##  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
##  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
##  $ x1          : num  5 5 1 1 3 1 3 3 3 5 ...
##  $ x2          : num  1 3 5 5 1 5 5 5 1 5 ...
```

```r
dferrors2[dferrors2$x1 == 5,] <- 3  #Omiting the columns reference
```

We can confirm that some of the other columns, like Sepal.Length were also over written by comparing the entry from our dferrors2 set with the original df.  The places that read the logic output 'FALSE' are instances where the 5 was changed to the 3, but unfortunately, since we left out the column specification in the recode, we overwrote the original values that were in Sepal.Length (and other columns too!)


```r
dferrors2$Sepal.Length == df$Sepal.Length 
```

```
##   [1] FALSE FALSE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE FALSE
##  [12]  TRUE FALSE  TRUE  TRUE FALSE FALSE  TRUE FALSE  TRUE FALSE  TRUE
##  [23]  TRUE FALSE  TRUE FALSE FALSE  TRUE  TRUE  TRUE  TRUE FALSE  TRUE
##  [34]  TRUE  TRUE  TRUE FALSE  TRUE FALSE FALSE  TRUE  TRUE  TRUE FALSE
##  [45] FALSE  TRUE  TRUE  TRUE FALSE  TRUE  TRUE  TRUE FALSE  TRUE  TRUE
##  [56]  TRUE FALSE FALSE  TRUE  TRUE  TRUE FALSE FALSE FALSE  TRUE  TRUE
##  [67] FALSE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE FALSE FALSE
##  [78] FALSE  TRUE FALSE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE
##  [89]  TRUE  TRUE FALSE FALSE  TRUE FALSE  TRUE  TRUE  TRUE  TRUE FALSE
## [100]  TRUE  TRUE  TRUE  TRUE  TRUE FALSE FALSE  TRUE FALSE  TRUE  TRUE
## [111]  TRUE FALSE FALSE  TRUE  TRUE FALSE  TRUE  TRUE FALSE  TRUE FALSE
## [122]  TRUE FALSE  TRUE  TRUE  TRUE FALSE  TRUE  TRUE  TRUE  TRUE  TRUE
## [133]  TRUE FALSE  TRUE FALSE  TRUE  TRUE  TRUE  TRUE
```
Let's recreate dferrors2. This time, I'm leaving all columns. Remember below, dferrors2 is initially a clone of df. We then re-write over the x1 variable because our coder says that her 5's were supposed to be 3. We specify this time that we only want to rewrite over column 6 (i.e., x1), and then ask which enteries of the coders do not match. This is what ultimately defines the dferrors2 dataframe. 



```r
dferrors2 <- df

str(dferrors2) #Now has 7 variables
```

```
## 'data.frame':	140 obs. of  7 variables:
##  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
##  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
##  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
##  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
##  $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ x1          : num  5 5 1 1 3 1 3 3 3 5 ...
##  $ x2          : num  1 3 5 5 1 5 5 5 1 5 ...
```

```r
dferrors2[dferrors2$x1 == 5,6] <- 3

which(dferrors2$x1 != dferrors2$x2)
```

```
##   [1]   1   3   4   5   6   7   8   9  10  11  12  13  14  15  16  17  20
##  [18]  21  23  26  27  29  30  31  32  35  37  38  42  43  44  46  47  48
##  [35]  50  51  52  53  55  56  58  59  60  62  63  64  65  67  68  72  73
##  [52]  75  77  80  81  82  83  84  85  86  87  88  90  91  93  95  96  97
##  [69]  98  99 101 102 103 105 106 107 108 110 111 112 113 114 115 116 117
##  [86] 119 120 121 123 124 125 126 127 129 130 131 132 133 135 136 137 138
```

```r
dferrors2 <- dferrors2[which(dferrors2$x1 != dferrors2$x2),]


str(dferrors2) #Not much of an improvement 
```

```
## 'data.frame':	102 obs. of  7 variables:
##  $ Sepal.Length: num  5.1 4.7 4.6 5 5.4 4.6 5 4.4 4.9 5.4 ...
##  $ Sepal.Width : num  3.5 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 3.7 ...
##  $ Petal.Length: num  1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 1.5 ...
##  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 0.2 ...
##  $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ x1          : num  3 1 1 3 1 3 3 3 3 3 ...
##  $ x2          : num  1 5 5 1 5 5 5 1 5 1 ...
```

Above I dropped a column when defining the dferrors2 dataframe initially using the minus sign  _-_ . This can also be used in conjunction with _c()_ if I wanted to remove multiple columns. 

Let's say, I'm not satisfied with coders' x1 and x2 scores, and want to drop them. 

Here I'm back using our original dataframe, not the ones we were looking at with errors. 


```r
df <- df[-c(6,7)]  #We are overwrite the existing df now with just the first 5 columns and not columns 6 and 7 or variables x1 and x2

str(df)
```

```
## 'data.frame':	140 obs. of  5 variables:
##  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
##  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
##  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
##  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
##  $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
```

_NOTE_ Because there is no comma above outside of the c() argument, R reads 6 and 7 as columns. We could also write as:

df[,-c(6,7)]    

if we wanted to retain all the rows.


We also could have subsetted these data in a single command with:

_df <- df[c(1:40,51:150), -c(6,7)]_

If our original dataset had variables x1 and x2 included.


```r
head(df) # Get a glimpse~
```

```
##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
## 1          5.1         3.5          1.4         0.2  setosa
## 2          4.9         3.0          1.4         0.2  setosa
## 3          4.7         3.2          1.3         0.2  setosa
## 4          4.6         3.1          1.5         0.2  setosa
## 5          5.0         3.6          1.4         0.2  setosa
## 6          5.4         3.9          1.7         0.4  setosa
```



### Data Modification

To begin, I'm going to redefine our df with the original iris data set-- to keep the total sample to 150. Removing the rows above just serves as an example.


```r
df <- iris

dim(df) #reports dimensions of the dataframe, 150 rows by 5 columns
```

```
## [1] 150   5
```

I'm going to add some additional variables, randomly selecting 0 or 1. These will serve as additional categorical variables for us to work with in analyses and graphing. Since these are flowers, let's say these variables represent rainfall conditions, Dry versus Wet, and use of fertilizer, yes or no. The rainfall variable would be a random effect and fertizlier was our experimental manipulation 



```r
rainfall <- sample(x = c(1,2), size = 150, replace = T)

table(rainfall) #Note uneven number, perhaps expected since we can't control the whether
```

```
## rainfall
##  1  2 
## 72 78
```

```r
fert <- rep(c(1, 2),each = 75)

fert
```

```
##   [1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
##  [36] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
##  [71] 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
## [106] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
## [141] 2 2 2 2 2 2 2 2 2 2
```

```r
fert <- sample(fert) #Takes random sample of the order fert, essentially, randomizing it

fert
```

```
##   [1] 1 2 1 1 1 2 1 1 2 2 2 1 2 1 1 1 2 2 1 2 1 2 2 2 2 2 2 1 2 1 2 1 2 1 1
##  [36] 2 1 1 2 1 2 2 2 1 1 1 1 1 2 1 1 2 2 1 2 1 2 2 1 2 1 1 1 2 1 2 1 2 2 2
##  [71] 1 1 1 1 1 1 1 1 2 2 2 1 2 2 1 1 2 2 2 2 1 2 2 2 2 2 2 2 2 1 2 2 2 1 2
## [106] 1 2 1 2 1 1 2 2 1 1 2 2 2 1 2 2 1 2 2 2 1 1 1 2 1 1 1 1 1 2 1 2 2 2 1
## [141] 1 1 1 2 1 1 1 2 1 1
```

```r
df <- cbind(df,rainfall, fert)

str(df)
```

```
## 'data.frame':	150 obs. of  7 variables:
##  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
##  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
##  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
##  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
##  $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ rainfall    : num  1 2 1 1 2 2 2 2 2 1 ...
##  $ fert        : num  1 2 1 1 1 2 1 1 2 2 ...
```

rainfall and fert were originally coded as 1's and 2's, where let's say 1 meant dry and 2 meant wet.  Similarly for fert, 1 = no and 2 = yes. The _Species_ variable is a categorical variable of the type of iris being observed.

We will need to overwrite these rainfall and fert variables, which are presently viewed as numbers, and tell R that these are really _factors_ or a categorical variable. We want them to be similarly to how R views Species. 


Below is one way to reassign these variables. 


_df$rainfall <- factor(df$rainfall, labels = c("Dry", "Wet"))_

_df$fert <- factor(df$fert, labels = c("No", "Yes"))_


Or use _lapply()_  

_lapply_ is essentially saying apply the following command _as.factor_ and returns a list object, in this case, a factor. The reassignment is like saying: "In df, columns 6 and 7, reassign to this value. This value should take all contents of what's in df columns 6 and 7 ( _which presently are just numbers_ ), and make them into factors"


```r
df$rainfall # 1 = Dry and 2 = Wet
```

```
##   [1] 1 2 1 1 2 2 2 2 2 1 1 2 2 2 1 2 2 2 2 2 1 2 2 2 1 1 1 2 2 2 1 1 1 1 1
##  [36] 1 1 2 1 1 1 1 2 2 2 1 2 2 2 1 2 1 1 1 2 2 2 1 1 2 2 1 2 2 2 1 2 2 2 1
##  [71] 1 1 1 1 1 2 2 2 2 1 2 1 1 1 2 1 1 2 1 1 1 2 2 2 1 2 1 2 1 2 2 1 1 2 1
## [106] 2 1 2 1 2 1 1 1 2 1 2 2 2 1 2 1 2 2 1 1 1 2 1 2 2 2 2 2 2 1 1 2 1 2 1
## [141] 1 2 2 1 2 1 2 1 2 1
```

```r
df$fert # 1 = No and 2 = Yes
```

```
##   [1] 1 2 1 1 1 2 1 1 2 2 2 1 2 1 1 1 2 2 1 2 1 2 2 2 2 2 2 1 2 1 2 1 2 1 1
##  [36] 2 1 1 2 1 2 2 2 1 1 1 1 1 2 1 1 2 2 1 2 1 2 2 1 2 1 1 1 2 1 2 1 2 2 2
##  [71] 1 1 1 1 1 1 1 1 2 2 2 1 2 2 1 1 2 2 2 2 1 2 2 2 2 2 2 2 2 1 2 2 2 1 2
## [106] 1 2 1 2 1 1 2 2 1 1 2 2 2 1 2 2 1 2 2 2 1 1 1 2 1 1 1 1 1 2 1 2 2 2 1
## [141] 1 1 1 2 1 1 1 2 1 1
```

```r
str(df)
```

```
## 'data.frame':	150 obs. of  7 variables:
##  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
##  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
##  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
##  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
##  $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ rainfall    : num  1 2 1 1 2 2 2 2 2 1 ...
##  $ fert        : num  1 2 1 1 1 2 1 1 2 2 ...
```

```r
df[c(6,7)] <- lapply(df[c(6,7)], as.factor)

str(df[c(6,7)])
```

```
## 'data.frame':	150 obs. of  2 variables:
##  $ rainfall: Factor w/ 2 levels "1","2": 1 2 1 1 2 2 2 2 2 1 ...
##  $ fert    : Factor w/ 2 levels "1","2": 1 2 1 1 1 2 1 1 2 2 ...
```

You'll notice that although rainfall and fert are now factors like our Species variable, they are still listed as 1's and 2's.

Our Species variable lists the levels of this factor in the structure print out. We can also print them with the command _levels()_


```r
levels(df$Species)
```

```
## [1] "setosa"     "versicolor" "virginica"
```

```r
levels(df$rainfall)
```

```
## [1] "1" "2"
```

We can assign labels to our levels of a factor using the _labels()_ argument

**Order is _very_ important**

The order that these labels are listed is how R determines what the 1 equals, 2 equals, etc.

We need to "assign" labels to these levels. We are **not** changing the levels themselves. These exist as 1 and 2. All we are doing is defining what 1 and 2 are, and since there currently are no labels, we cannot specify that without R printing an error message about labels not existing.

For this, we will need to say _levels()_ of the variable of interest and then assign a concatenated string as our labels. We do not use the _labels_ command here.



```r
levels(df$rainfall) <- c("Dry", "Wet")
levels(df$fert) <- c("No", "Yes")

str(df)
```

```
## 'data.frame':	150 obs. of  7 variables:
##  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
##  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
##  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
##  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
##  $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ rainfall    : Factor w/ 2 levels "Dry","Wet": 1 2 1 1 2 2 2 2 2 1 ...
##  $ fert        : Factor w/ 2 levels "No","Yes": 1 2 1 1 1 2 1 1 2 2 ...
```

```r
head(df)
```

```
##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species rainfall fert
## 1          5.1         3.5          1.4         0.2  setosa      Dry   No
## 2          4.9         3.0          1.4         0.2  setosa      Wet  Yes
## 3          4.7         3.2          1.3         0.2  setosa      Dry   No
## 4          4.6         3.1          1.5         0.2  setosa      Dry   No
## 5          5.0         3.6          1.4         0.2  setosa      Wet   No
## 6          5.4         3.9          1.7         0.4  setosa      Wet  Yes
```

```r
tail(df) # Everything looks good!
```

```
##     Sepal.Length Sepal.Width Petal.Length Petal.Width   Species rainfall
## 145          6.7         3.3          5.7         2.5 virginica      Wet
## 146          6.7         3.0          5.2         2.3 virginica      Dry
## 147          6.3         2.5          5.0         1.9 virginica      Wet
## 148          6.5         3.0          5.2         2.0 virginica      Dry
## 149          6.2         3.4          5.4         2.3 virginica      Wet
## 150          5.9         3.0          5.1         1.8 virginica      Dry
##     fert
## 145   No
## 146   No
## 147   No
## 148  Yes
## 149   No
## 150   No
```

Last few modifications we could consider are the variable names. Sometimes, variables have verbose names, and sometimes they are more ambiguous. Using _colnames()_  or simply _names()_ with the dataframe of interest inside will print. 



```r
colnames(df)
```

```
## [1] "Sepal.Length" "Sepal.Width"  "Petal.Length" "Petal.Width" 
## [5] "Species"      "rainfall"     "fert"
```

```r
names(df)
```

```
## [1] "Sepal.Length" "Sepal.Width"  "Petal.Length" "Petal.Width" 
## [5] "Species"      "rainfall"     "fert"
```

We can also assign these names all at once    **Not recommended for large datasets**

We can create a new object-- a character vector, then assign this character vector to the column names of the data frame


```r
newnames <- c("V1", "V2", "V3", "V4", "V5", "V6", "V7")

colnames(df) <- newnames

colnames(df)
```

```
## [1] "V1" "V2" "V3" "V4" "V5" "V6" "V7"
```
This would be very tedious with large data sets. Let's change back. Just re-assign it!


```r
oldnames <- c("Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "Species", "rainfall", "fert")

colnames(df) <- oldnames

names(df)
```

```
## [1] "Sepal.Length" "Sepal.Width"  "Petal.Length" "Petal.Width" 
## [5] "Species"      "rainfall"     "fert"
```


Individual columns can be renamed as well. We can just rename these columns specifically using _[  ]_ to index.


```r
colnames(df)[c(1,3)] <- c("Example", "NewName")

names(df)
```

```
## [1] "Example"     "Sepal.Width" "NewName"     "Petal.Width" "Species"    
## [6] "rainfall"    "fert"
```

```r
names(df) <- oldnames  #Change it back

names(df) #confirmed!
```

```
## [1] "Sepal.Length" "Sepal.Width"  "Petal.Length" "Petal.Width" 
## [5] "Species"      "rainfall"     "fert"
```


Now we are ready to check some assumptions about the data~

# 5/8/19: df Basics 


## Summaries and Quick Descriptives

_Easy-peasey, lemon squeazy~_


|**This argument....**     | **.....performs this function**              |
|--------------------------|----------------------------------------------|
| _summary()_               |                            |
| _aggregate()_             |                                             |
| _tapply()_ or _lapply()_ or _sapply()_ or _mapply()_    |    The apply essentially say take Y, indexing X, and perform a function, such as mean, median, sd, quantile, sum, etc., depending on the R object in use/type of desired output determines which apply to use:  **lapply** for _lists_, **mapply** for _matrices_, **tapply** for things like factors (though these still are treated as lists) and **sapply** for for returning a vector, matrix, or array   |
| _with()_                |                                               |
| _tidyr:summarize()_ , *summarize_if()*, *summarize_all*, *summarize_at()* |  a part of tidyverse          |
| *group_by*              |                                               |
| _CreateTableOne()_      |                                               |
| _describeBy()_          |   Gives summary statistics for numeric varialbes specified by grouping variable, including skewness and kurtosis  |
| _describe_              | gives summary statistics, treating factors as numbers, cannot be grouped useful for whole sample descriptives of central tendency and variance                      |


To start-- there is the base R _summary_ command. Calling this on our dataframe, df, will count factors, measure character lengths, and quartiles/means/medians of numeric variables. 


```r
summary(df)
```

```
##   Sepal.Length    Sepal.Width     Petal.Length    Petal.Width   
##  Min.   :4.300   Min.   :2.000   Min.   :1.000   Min.   :0.100  
##  1st Qu.:5.100   1st Qu.:2.800   1st Qu.:1.600   1st Qu.:0.300  
##  Median :5.800   Median :3.000   Median :4.350   Median :1.300  
##  Mean   :5.843   Mean   :3.057   Mean   :3.758   Mean   :1.199  
##  3rd Qu.:6.400   3rd Qu.:3.300   3rd Qu.:5.100   3rd Qu.:1.800  
##  Max.   :7.900   Max.   :4.400   Max.   :6.900   Max.   :2.500  
##        Species   rainfall  fert   
##  setosa    :50   Dry:72   No :75  
##  versicolor:50   Wet:78   Yes:75  
##  virginica :50                    
##                                   
##                                   
## 
```

Continuing the base R way, if you want to look at some summaries across groups, say comparing fertilizer use, you can use _aggregate_ and _tapply_ commands.

For each of these examples, R requires that numeric variables be treated as numbers and factors are not accepted as numbers-- but are categories. Consider this if you have 'non-numeric' error warnings and R is trying to averPetal.Length a category/factor.


This formula is using aggregate's built in algorithm: _aggregate(df$frequencyVaraible, by=list(df$CategoryVariable), FUN = sum/mean/sd/etc.)_


```r
aggregate(Sepal.Length ~Species, df, mean)  # is the same as
```

```
##      Species Sepal.Length
## 1     setosa        5.006
## 2 versicolor        5.936
## 3  virginica        6.588
```

```r
aggregate(df$Sepal.Length, by=list(df$Species), FUN = mean )
```

```
##      Group.1 mean.df$Sepal.Length
## 1     setosa                5.006
## 2 versicolor                5.936
## 3  virginica                6.588
```

```r
aggregate(Sepal.Length ~ Species + rainfall, df, mean) # Additional categories can be added to the formula
```

```
##      Species rainfall Sepal.Length
## 1     setosa      Dry     5.034783
## 2 versicolor      Dry     5.920000
## 3  virginica      Dry     6.545833
## 4     setosa      Wet     4.981481
## 5 versicolor      Wet     5.952000
## 6  virginica      Wet     6.626923
```

Or we could use _tapply_ 


```r
tapply(df$Sepal.Length, df$Species, FUN = mean)
```

```
##     setosa versicolor  virginica 
##      5.006      5.936      6.588
```

Multiple summary statistics? There's a function for that. This asks for the averPetal.Length and standard deviation


```r
tapply(df$Sepal.Length, df$Species, FUN = function(x) c( MN= mean(x), SD = sd(x)))
```

```
## $setosa
##        MN        SD 
## 5.0060000 0.3524897 
## 
## $versicolor
##        MN        SD 
## 5.9360000 0.5161711 
## 
## $virginica
##        MN        SD 
## 6.5880000 0.6358796
```


This individual/self-defined function also works with aggregate command as well. Here the _with()_ begins specifying the dataframe, so we don't need to specify this later in the aggregate formula


```r
with(df, aggregate(Sepal.Length ~ Species + rainfall, FUN = function(x) c(MN = mean(x), SD = sd(x))))
```

```
##      Species rainfall Sepal.Length.MN Sepal.Length.SD
## 1     setosa      Dry       5.0347826       0.3472079
## 2 versicolor      Dry       5.9200000       0.5066228
## 3  virginica      Dry       6.5458333       0.6121801
## 4     setosa      Wet       4.9814815       0.3616597
## 5 versicolor      Wet       5.9520000       0.5355060
## 6  virginica      Wet       6.6269231       0.6666679
```


Here we are using some tidyr commands and the **%>%** operator. This lets R know to "keep going" as a flow from the previous line to the next. It saves the additional writing from Base R commands of calling the dataframe each time we reference a column

The tableone package is handy for providing descriptive statistics on each variable. However, because some of our variables are factors, R will count them as categories.

Below is saying:  "Create the object tabave. Take df -- then group by Species -- summarize the variable if it is numeric, and return the mean and standard deviation"

The initial assignment only assigns-- to see what we created-- call it's name~


```r
tabave <- df %>%
            group_by(Species) %>%
            summarise_if(is.numeric, c(mean, sd))
tabave
```

```
## # A tibble: 3 x 9
##   Species Sepal.Length_fn1 Sepal.Width_fn1 Petal.Length_fn1 Petal.Width_fn1
##   <fct>              <dbl>           <dbl>            <dbl>           <dbl>
## 1 setosa              5.01            3.43             1.46           0.246
## 2 versic~             5.94            2.77             4.26           1.33 
## 3 virgin~             6.59            2.97             5.55           2.03 
## # ... with 4 more variables: Sepal.Length_fn2 <dbl>,
## #   Sepal.Width_fn2 <dbl>, Petal.Length_fn2 <dbl>, Petal.Width_fn2 <dbl>
```

This prints out, by Species, our group means-- listed first with a "_fn1" -- followed by the group SD with a "_fn2"

_Note_ This did not print out anything with rainfall or fert. That is because these variables are not numeric-- they are factors. This requries the *summarise_if()* command. 

Alternative summarise (or summarize! both are accepted) commands include: summarize(), summarize_at, summarize_all.   

In older R code you will sometimes see summarize_each, but this command is deprecated in the recent dplyr updates.

There are other summary type commands-- such as using the **tableone** package


```r
vars <- colnames(df)
vars <- vars[c(1:4)]
vars
```

```
## [1] "Sepal.Length" "Sepal.Width"  "Petal.Length" "Petal.Width"
```

```r
CreateTableOne(vars = vars, data = df, strata = "Species", test = T) #Have true test to obtain p values
```

```
##                           Stratified by Species
##                            setosa      versicolor  virginica   p      test
##   n                          50          50          50                   
##   Sepal.Length (mean (SD)) 5.01 (0.35) 5.94 (0.52) 6.59 (0.64) <0.001     
##   Sepal.Width (mean (SD))  3.43 (0.38) 2.77 (0.31) 2.97 (0.32) <0.001     
##   Petal.Length (mean (SD)) 1.46 (0.17) 4.26 (0.47) 5.55 (0.55) <0.001     
##   Petal.Width (mean (SD))  0.25 (0.11) 1.33 (0.20) 2.03 (0.27) <0.001
```


## Chi-Square Tests

A common test of independence and experimental check for adequate group representation examines whether expected frequencies of group membership match observed values or depend on other factors. That is, did a certain species inadvertainly experience a certain level of rainfall? Or did a species vary in its probability of receiving fertilizer?



```r
chisq.test(df$Species, df$rainfall)
```

```
## 
## 	Pearson's Chi-squared test
## 
## data:  df$Species and df$rainfall
## X-squared = 0.16026, df = 2, p-value = 0.923
```

```r
chisq.test(df$Species, df$fert)
```

```
## 
## 	Pearson's Chi-squared test
## 
## data:  df$Species and df$fert
## X-squared = 1.12, df = 2, p-value = 0.5712
```

```r
tadaa_chisq(df, x = Species, y = rainfall, print = "markdown")
```

Table 1: **Pearson's Chi-squared test**


| $\chi^2$ |  p   | df | Cramer\'s V |
|:--------:|:----:|:--:|:-----------:|
|   0.16   | .923 | 2  |    0.03     |


<br>


<br>

Both tests indicate a non-significant X-squared value, meaning we cannot reject the null hypothesis that the groups do not differ. That is, we observed no evidence to contradict the statement that the species of iris received similar variation of rainfall and fertilizer probabilities. 

## Variance Assumptions: Bartlett's and Levene's

We also want to examine homogeneity of variance for each group. To do this, we can run Bartlett's test or Levene's test. For here we just specify _bartlett.test_ wrapped around our variable of interest, in this case Sepal.Length, and our grouping variable Species. We then specify that the data file we are referring to is df

Alternative way of writing this test using _leveneTest()_ similarly with a (y, group) ordering and specify the dataframe directly.

Bartlett's test and Levene's are similar in testing for equal variances across groups for a continuous variable, with Levene's test being less influenced by outliers. 


```r
bartlett.test(Sepal.Length ~ Species, df) 
```

```
## 
## 	Bartlett test of homogeneity of variances
## 
## data:  Sepal.Length by Species
## Bartlett's K-squared = 16.006, df = 2, p-value = 0.0003345
```

```r
leveneTest(df$Sepal.Length, df$Species)
```

```
## Levene's Test for Homogeneity of Variance (center = median)
##        Df F value   Pr(>F)   
## group   2  6.3527 0.002259 **
##       147                    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

These test strongly indicate that the variance in sepal length among the groups is not equal, some groups have a larger variance than others.



```r
bartlett.test(Sepal.Length ~ rainfall, df) 
```

```
## 
## 	Bartlett test of homogeneity of variances
## 
## data:  Sepal.Length by rainfall
## Bartlett's K-squared = 0.59032, df = 1, p-value = 0.4423
```

```r
leveneTest(df$Sepal.Length, df$fert)
```

```
## Levene's Test for Homogeneity of Variance (center = median)
##        Df F value Pr(>F)
## group   1  0.4215 0.5172
##       148
```

For our other variables though, there appear to be similar variances among groups, when collapsing across species. It may be unrealistic to expect the different species variances to be similar, but good to keep in mind when running analyses


### Q-Q plots

Plotting normal qq-plots is also fairly easy. Here are examples with three methods-- you'll notice that once we start using tidyverse the output becomes a dataframe/double value. This will need to be unlisted first then changed to a number. This step is avoided in the other two methods because the Sepal.Length variable is a number, but once it is filtered and selected it is created into its own dataframe.


```r
# Base R way

qqnorm(df$Sepal.Length[df$rainfall == "Dry"], main = "Q-QPlot for Dry Sepal.Length"); qqline(df$Sepal.Length[df$rainfall == "Dry"])
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-35-1.png)<!-- -->

```r
#Tidy way

WetSepal.Length <- df %>%
dplyr::filter(rainfall == "Wet") %>%
  dplyr::select(Sepal.Length) 

qqnorm(as.numeric(unlist(WetSepal.Length)), main = "Q-QPlot for Wet Sepal.Length"); qqline(as.numeric(unlist(WetSepal.Length)))
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-35-2.png)<!-- -->

```r
## A GGPLOT way

df %>%
  ggplot(aes(sample = Sepal.Length)) +
  geom_qq() + geom_qq_line() +
  facet_wrap(~rainfall, scales = "free_y")
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-35-3.png)<!-- -->


## Skewedness and Kurtosis

We might already have an idea if the data are skewed from the Q-Q Plots, but we can also print these values directly for the whole data set

We can use _describeBy()_ and just like with summarize- the variable will need to be numeric. Below we are asking R to describe columns 3-5, all numeric, and by Species Group.



```r
describeBy(df[1:4], group = df$Species)
```

```
## 
##  Descriptive statistics by group 
## group: setosa
##              vars  n mean   sd median trimmed  mad min max range skew
## Sepal.Length    1 50 5.01 0.35    5.0    5.00 0.30 4.3 5.8   1.5 0.11
## Sepal.Width     2 50 3.43 0.38    3.4    3.42 0.37 2.3 4.4   2.1 0.04
## Petal.Length    3 50 1.46 0.17    1.5    1.46 0.15 1.0 1.9   0.9 0.10
## Petal.Width     4 50 0.25 0.11    0.2    0.24 0.00 0.1 0.6   0.5 1.18
##              kurtosis   se
## Sepal.Length    -0.45 0.05
## Sepal.Width      0.60 0.05
## Petal.Length     0.65 0.02
## Petal.Width      1.26 0.01
## -------------------------------------------------------- 
## group: versicolor
##              vars  n mean   sd median trimmed  mad min max range  skew
## Sepal.Length    1 50 5.94 0.52   5.90    5.94 0.52 4.9 7.0   2.1  0.10
## Sepal.Width     2 50 2.77 0.31   2.80    2.78 0.30 2.0 3.4   1.4 -0.34
## Petal.Length    3 50 4.26 0.47   4.35    4.29 0.52 3.0 5.1   2.1 -0.57
## Petal.Width     4 50 1.33 0.20   1.30    1.32 0.22 1.0 1.8   0.8 -0.03
##              kurtosis   se
## Sepal.Length    -0.69 0.07
## Sepal.Width     -0.55 0.04
## Petal.Length    -0.19 0.07
## Petal.Width     -0.59 0.03
## -------------------------------------------------------- 
## group: virginica
##              vars  n mean   sd median trimmed  mad min max range  skew
## Sepal.Length    1 50 6.59 0.64   6.50    6.57 0.59 4.9 7.9   3.0  0.11
## Sepal.Width     2 50 2.97 0.32   3.00    2.96 0.30 2.2 3.8   1.6  0.34
## Petal.Length    3 50 5.55 0.55   5.55    5.51 0.67 4.5 6.9   2.4  0.52
## Petal.Width     4 50 2.03 0.27   2.00    2.03 0.30 1.4 2.5   1.1 -0.12
##              kurtosis   se
## Sepal.Length    -0.20 0.09
## Sepal.Width      0.38 0.05
## Petal.Length    -0.37 0.08
## Petal.Width     -0.75 0.04
```
The Petal Width of the Setosa group appears slight skewed, but no other value indicates an issue


We can also use regular _describe_ which can handle factors


```r
describe(df)
```

```
## df 
## 
##  7  Variables      150  Observations
## ---------------------------------------------------------------------------
## Sepal.Length 
##        n  missing distinct     Info     Mean      Gmd      .05      .10 
##      150        0       35    0.998    5.843   0.9462    4.600    4.800 
##      .25      .50      .75      .90      .95 
##    5.100    5.800    6.400    6.900    7.255 
## 
## lowest : 4.3 4.4 4.5 4.6 4.7, highest: 7.3 7.4 7.6 7.7 7.9
## ---------------------------------------------------------------------------
## Sepal.Width 
##        n  missing distinct     Info     Mean      Gmd      .05      .10 
##      150        0       23    0.992    3.057   0.4872    2.345    2.500 
##      .25      .50      .75      .90      .95 
##    2.800    3.000    3.300    3.610    3.800 
## 
## lowest : 2.0 2.2 2.3 2.4 2.5, highest: 3.9 4.0 4.1 4.2 4.4
## ---------------------------------------------------------------------------
## Petal.Length 
##        n  missing distinct     Info     Mean      Gmd      .05      .10 
##      150        0       43    0.998    3.758    1.979     1.30     1.40 
##      .25      .50      .75      .90      .95 
##     1.60     4.35     5.10     5.80     6.10 
## 
## lowest : 1.0 1.1 1.2 1.3 1.4, highest: 6.3 6.4 6.6 6.7 6.9
## ---------------------------------------------------------------------------
## Petal.Width 
##        n  missing distinct     Info     Mean      Gmd      .05      .10 
##      150        0       22     0.99    1.199   0.8676      0.2      0.2 
##      .25      .50      .75      .90      .95 
##      0.3      1.3      1.8      2.2      2.3 
## 
## lowest : 0.1 0.2 0.3 0.4 0.5, highest: 2.1 2.2 2.3 2.4 2.5
## ---------------------------------------------------------------------------
## Species 
##        n  missing distinct 
##      150        0        3 
##                                            
## Value          setosa versicolor  virginica
## Frequency          50         50         50
## Proportion      0.333      0.333      0.333
## ---------------------------------------------------------------------------
## rainfall 
##        n  missing distinct 
##      150        0        2 
##                     
## Value       Dry  Wet
## Frequency    72   78
## Proportion 0.48 0.52
## ---------------------------------------------------------------------------
## fert 
##        n  missing distinct 
##      150        0        2 
##                   
## Value       No Yes
## Frequency   75  75
## Proportion 0.5 0.5
## ---------------------------------------------------------------------------
```

## Correlations

Useful links for correlation basics:


http://www.sthda.com/english/wiki/correlation-matrix-a-quick-start-guide-to-analyze-format-and-visualize-a-correlation-matrix-using-r-software


https://cran.r-project.org/web/packages/corrplot/vignettes/corrplot-intro.html


There are several ways to print or _summarize_ correlations of a variable set. First the **tidy** way!


Methods that are accepted include: "pearson", "spearman", or "kendall"


```r
# Need to drop non-numeric variables which are in columns 5,6, and 7

cor_matrix <- df %>%
                group_by(Species) %>%
                  do(as.data.frame(cor(.[,-c(5:7)], method="spearman", use="pairwise.complete.obs")))

#
# to add row names
#
cor_matrix1 <- cor_matrix %>%  
              data.frame(row=rep(colnames(.)[-1], n_groups(.))) 
#
# calculate correlations and display in column format
#
num_col=ncol(df[,-c(5,6,7)])
out_indx <-  which(upper.tri(diag(num_col))) 
cor_cols <- df %>% group_by(Species) %>%
            do(melt(cor(.[,-c(5,6,7)], method="spearman", use="pairwise.complete.obs"), value.name="cor")[out_indx,])

ncol(df[,-c(5,6,7)])
```

```
## [1] 4
```

```r
cor_matrix1
```

```
##       Species Sepal.Length Sepal.Width Petal.Length Petal.Width
## 1      setosa    1.0000000   0.7553375    0.2788849   0.2994989
## 2      setosa    0.7553375   1.0000000    0.1799110   0.2865359
## 3      setosa    0.2788849   0.1799110    1.0000000   0.2711414
## 4      setosa    0.2994989   0.2865359    0.2711414   1.0000000
## 5  versicolor    1.0000000   0.5176060    0.7366251   0.5486791
## 6  versicolor    0.5176060   1.0000000    0.5747272   0.6599826
## 7  versicolor    0.7366251   0.5747272    1.0000000   0.7870096
## 8  versicolor    0.5486791   0.6599826    0.7870096   1.0000000
## 9   virginica    1.0000000   0.4265165    0.8243234   0.3157721
## 10  virginica    0.4265165   1.0000000    0.3873587   0.5443098
## 11  virginica    0.8243234   0.3873587    1.0000000   0.3629133
## 12  virginica    0.3157721   0.5443098    0.3629133   1.0000000
##             row
## 1  Sepal.Length
## 2   Sepal.Width
## 3  Petal.Length
## 4   Petal.Width
## 5  Sepal.Length
## 6   Sepal.Width
## 7  Petal.Length
## 8   Petal.Width
## 9  Sepal.Length
## 10  Sepal.Width
## 11 Petal.Length
## 12  Petal.Width
```


Not bad, but it would be nice to know if some of these are significant or not. We can use _cor.test_.  Here I'm defining is as an object **examplecor** so you can print the object by calling its name:


```r
examplecor <- cor.test(df$Sepal.Length,df$Petal.Length)
```

And as for its structure to understand what R has created-- unsurprisingly, it's a list!. But this is good to know as it will let us index individual aspects of the list if we would like more direct comparisons using it in conjunction with dplyr


```r
str(examplecor)
```

```
## List of 9
##  $ statistic  : Named num 21.6
##   ..- attr(*, "names")= chr "t"
##  $ parameter  : Named int 148
##   ..- attr(*, "names")= chr "df"
##  $ p.value    : num 1.04e-47
##  $ estimate   : Named num 0.872
##   ..- attr(*, "names")= chr "cor"
##  $ null.value : Named num 0
##   ..- attr(*, "names")= chr "correlation"
##  $ alternative: chr "two.sided"
##  $ method     : chr "Pearson's product-moment correlation"
##  $ data.name  : chr "df$Sepal.Length and df$Petal.Length"
##  $ conf.int   : num [1:2] 0.827 0.906
##   ..- attr(*, "conf.level")= num 0.95
##  - attr(*, "class")= chr "htest"
```

```r
examplecor$conf.int
```

```
## [1] 0.8270363 0.9055080
## attr(,"conf.level")
## [1] 0.95
```

The structure reports this as a _list_ of information. We can index these as column names using the $ operator or [] indexing their number in the list.


```r
df %>%
  group_by(fert) %>%
  dplyr::summarize(cor = cor.test(Sepal.Length,Petal.Length)$estimate, p.value = cor.test(Sepal.Length, Petal.Length)$p.value, LowerCInt  = cor.test(Sepal.Length, Petal.Length)$conf.int[[1]], UpperCInt  = cor.test(Sepal.Length, Petal.Length)$conf.int[[2]])
```

```
## # A tibble: 2 x 5
##   fert    cor  p.value LowerCInt UpperCInt
##   <fct> <dbl>    <dbl>     <dbl>     <dbl>
## 1 No    0.864 1.91e-23     0.792     0.912
## 2 Yes   0.886 4.62e-26     0.825     0.927
```

```r
#Ungrouped by commenting out the grouping part of the argument

df %>%
  #group_by(fert) %>%
  dplyr::summarize(cor = cor.test(Sepal.Length,Petal.Length)$estimate, p.value = cor.test(Sepal.Length, Petal.Length)$p.value, LowerCInt  = cor.test(Sepal.Length, Petal.Length)$conf.int[[1]], UpperCInt  = cor.test(Sepal.Length, Petal.Length)$conf.int[[2]])
```

```
##         cor      p.value LowerCInt UpperCInt
## 1 0.8717538 1.038667e-47 0.8270363  0.905508
```

OK, but what if we wanted a correlation matrix _and_ p-values?


For this we can use the Hmisc package using the _rcorr()_ command. 


This requires that a matrix be fed in and outputs a list, the first item, 'r',  being the correlation value and the second item, 'p', being the corresponding p-value

Below I'm creating two dataframes that will ultimately be matrices, first grouping by fertilizer condition and then selecting only numeric values


```r
nofertmat1 <- df %>%
  filter(fert == 'No') %>%
  dplyr::select_if(is.numeric)
  

yesfertmat1 <- df %>%
  filter(fert == 'Yes') %>%
  dplyr::select_if(is.numeric)

#Now both are now numeric values in dataframes-- still need to be made into matrices

  
nofertcors <- rcorr(as.matrix(nofertmat1))  #Creates a list containing objects about the matrix's r-value, n of sample, and p-value or correlation


#As index by its structure

str(nofertcors)
```

```
## List of 3
##  $ r: num [1:4, 1:4] 1 -0.177 0.864 0.796 -0.177 ...
##   ..- attr(*, "dimnames")=List of 2
##   .. ..$ : chr [1:4] "Sepal.Length" "Sepal.Width" "Petal.Length" "Petal.Width"
##   .. ..$ : chr [1:4] "Sepal.Length" "Sepal.Width" "Petal.Length" "Petal.Width"
##  $ n: int [1:4, 1:4] 75 75 75 75 75 75 75 75 75 75 ...
##   ..- attr(*, "dimnames")=List of 2
##   .. ..$ : chr [1:4] "Sepal.Length" "Sepal.Width" "Petal.Length" "Petal.Width"
##   .. ..$ : chr [1:4] "Sepal.Length" "Sepal.Width" "Petal.Length" "Petal.Width"
##  $ P: num [1:4, 1:4] NA 0.129 0 0 0.129 ...
##   ..- attr(*, "dimnames")=List of 2
##   .. ..$ : chr [1:4] "Sepal.Length" "Sepal.Width" "Petal.Length" "Petal.Width"
##   .. ..$ : chr [1:4] "Sepal.Length" "Sepal.Width" "Petal.Length" "Petal.Width"
##  - attr(*, "class")= chr "rcorr"
```


We can then bind the vectors from the list of separated groups, Wet and Dry, and compare.



```r
yesfertcors <- rcorr(as.matrix(yesfertmat1))


cbind(yesfertcors$r,yesfertcors$P,nofertcors$r, nofertcors$P)
```

```
##              Sepal.Length Sepal.Width Petal.Length Petal.Width
## Sepal.Length   1.00000000 -0.07433529    0.8859194   0.8465170
## Sepal.Width   -0.07433529  1.00000000   -0.3654328  -0.3015800
## Petal.Length   0.88591938 -0.36543284    1.0000000   0.9698363
## Petal.Width    0.84651701 -0.30157997    0.9698363   1.0000000
##              Sepal.Length Sepal.Width Petal.Length Petal.Width
## Sepal.Length           NA 0.526193234  0.000000000  0.00000000
## Sepal.Width     0.5261932          NA  0.001264733  0.00855315
## Petal.Length    0.0000000 0.001264733           NA  0.00000000
## Petal.Width     0.0000000 0.008553150  0.000000000          NA
##              Sepal.Length Sepal.Width Petal.Length Petal.Width
## Sepal.Length    1.0000000  -0.1766929    0.8639526   0.7964975
## Sepal.Width    -0.1766929   1.0000000   -0.4965847  -0.4351760
## Petal.Length    0.8639526  -0.4965847    1.0000000   0.9570866
## Petal.Width     0.7964975  -0.4351760    0.9570866   1.0000000
##              Sepal.Length  Sepal.Width Petal.Length  Petal.Width
## Sepal.Length           NA 1.294023e-01 0.000000e+00 0.000000e+00
## Sepal.Width     0.1294023           NA 5.862721e-06 9.549074e-05
## Petal.Length    0.0000000 5.862721e-06           NA 0.000000e+00
## Petal.Width     0.0000000 9.549074e-05 0.000000e+00           NA
```

Pretty messy, but there are other methods to that have more to do with visualization. _rcorr_ works well, when you are looking at full sample descriptives or not binding columns from different groups. 


One package, Performance Analytics, quickly gives visualizations and r/p values; however, specification is limited as of now in separting groups on the same graph. * I currently don't have a more recent version of R to run this package-- I'm updating, don't judge. As such, I'm skipping this part for now.




#### Point Biserial Correlations


```r
#I haven't read about this yet!
```


# 5/15/19: Plots! {.tabset .tabset-fade .tabset-pill}

## Quick Base R plots

_Who doesn't love a good plot, am I right?_

There are many different kinds of plots you can use-- below covers just the basics to get you started with quick visualizations.Before making fancy plots with ggplot2 package, we can use Base R's plot commands to quickly visualize the data.

### Histograms

We can exam a variables distribution across the whole sample:


```r
hist(df$Petal.Width)
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-46-1.png)<!-- -->

Or by levels of a categorical variable of interest:


```r
# A tidy way

Dryhist <- df %>%  
dplyr::filter(rainfall == "Dry") %>%
  dplyr::select(Petal.Width) %>%
            lapply(hist, main = "Histogram Dry Example", xlab = "Dry", col.main = "red", cex.main = 2, col.lab = "darkblue")
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-47-1.png)<!-- -->

```r
### Other way with more Base R

hist(df$Petal.Width[df$rainfall == "Wet"], xlab = "Wet", main = "Histogram Wet Example", col = "purple")
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-47-2.png)<!-- -->

Notice that the _col_ placement is coloring various aspects of the plot, be it the bars, the title, the axis label, etc. and the addition of main title and axis titles with _main_ and _xlab_ the _cex.main_ refers to the size of the main title should be scaled to. _cex_ is otherwise used to reference scaling of an object -- quite useful graphing tool for a perfectionist. Other common graphical parameters can be found here:

https://www.statmethods.net/advgraphs/parameters.html


### Boxplots


```r
boxplot(df$Petal.Length, data =  df)
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-48-1.png)<!-- -->

```r
boxplot(df$Petal.Length~df$Species, data =  df, ylab = "Petal Length")
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-48-2.png)<!-- -->


Want it horizontal? Or with notches?


```r
boxplot(df$Petal.Length~df$Species, data =  df, xlab = "Petal Length", horizontal = TRUE, notch = TRUE, frame = FALSE)  #Note since this is being horizontal now we have to change our former "y lab" to an "x lab"
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-49-1.png)<!-- -->


Can't forget the colors


```r
boxplot(Sepal.Length~Species, data =  df, ylab = "Sepal Length", col = "grey", border = c("blue", "red", "green"), frame = F)
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-50-1.png)<!-- -->





### Bar Plots

Bar plots can depict counts, frequencies, and also aggregated statistics, like means, medians, etc.

For counts, we can make a table object with _table()_ command


```r
barplottab <- table(df$Species, df$rainfall)

barplottab # Which looks like this
```

```
##             
##              Dry Wet
##   setosa      23  27
##   versicolor  25  25
##   virginica   24  26
```

```r
#Then graph it

barplot(barplottab, legend = rownames(barplottab), beside = T, col = c("blue", "red", "green"))
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-51-1.png)<!-- -->

```r
#Note what happens when you leave the "beside = T" code out

barplot(barplottab, legend = rownames(barplottab), col = c("blue", "red", "green")) 
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-51-2.png)<!-- -->

We can also use the _aggregate()_ command and _aggregate.plot()_. As we used above, aggregate performs summary stats in base R. Read more about aggregate function with  _?aggregate_ in your console.


```r
# This is a more recent package but is still pretty cool!

#install.packages("epiDisplay")
#library(epiDisplay)


aggregate.plot(df$Sepal.Width,
                              by=list(df$Species), error = "ci" ,
                              FUN="median")
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-52-1.png)<!-- -->

```r
# Get fancier

aggregate.plot(df$Sepal.Width,
                              by=list(df$Species, df$rainfall), error = "ci" , bar.col = c("steelblue1", "deeppink4", "turquoise3"),
                              FUN="mean", legend.site = "bottom")
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-52-2.png)<!-- -->

_FUN FACT_ These were my favorite three colors in 2nd grade!

### Scatter Plots


```r
plot(df$Sepal.Length, df$Petal.Width, main = "Sepal.Length by Petal.Width",
     xlab = "Sepal.Length", ylab = "Petal.Width",
     pch = 19, frame = TRUE)  #pch is the shape of the scatter point
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-53-1.png)<!-- -->

```r
# Add regression line
plot(df$Sepal.Length, df$Petal.Width, main = "Sepal.Length by Petal.Width",
     xlab = "Sepal.Length", ylab = "Petal.Width",
     pch = 10, frame = FALSE)
abline(lm(Petal.Width ~ Sepal.Length, data = df), col = "blue")
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-53-2.png)<!-- -->

Can also use _scatterplot_ from the **cars** package


```r
scatterplot(Petal.Width ~ Sepal.Length|Species, data = df)
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-54-1.png)<!-- -->

Gross- too busy, but handy to visualize quickly adding the below makes it look somewhat better


```r
scatterplot(Petal.Width ~ Sepal.Length|Species, data = df, smooth = FALSE, grid = FALSE, frame = FALSE)
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-55-1.png)<!-- -->

OK but if we want different colors? For base plotting, I've just been using random colors, but let's say I really like those three colors that were my favorite in 2nd grade.

For this I can create a vector of color names that needs to be the same length as your grouping factor(s). Since we just have 3 groups for species, we just need to pick three colors. You can use names of colors, their hexadecimal numbers, or integer number placement on color palette 

Check out more colors here: https://rstudio-pubs-static.s3.amazonaws.com/3486_79191ad32cf74955b4502b8530aad627.html


```r
#Color vector

mycolors <- c("steelblue1", "deeppink4", "turquoise3")

scatterplot(Petal.Width ~ Sepal.Length|Species, data = df, smooth = FALSE, grid = FALSE, frame = FALSE, col =mycolors)
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-56-1.png)<!-- -->


## GGPlot2 

GGplot2 is graphing package in R that can give a more polished touch (imo), but Base R graphing can literally create anything. GGplot2 can accept tidyverse outputs which can be extremely efficient.

Things to note are the order of the argument, unless using the _%>%_ operator, the command is telling R to use the ggplot package then gives the name of the dataframe. This is followed by the _aes()_ argument defining the _aesthetic_ features of the graph. Here the x and y variables are defined as well as any group specification that would apply globally to the graph. You may define group variables in subsequent lines with an additional _aes()_ command. 

Then the "+" symbol indicates that the following should be added to the plot. Here is where you define the type of "shape" R should draw using *geom_shapename*

You can define a grouping variable many ways, in the sample below we have Species group defining the "coloring" of the plot.

The use of _color_ here references a command that R should use when coloring things where a color can't be "filled in" as much as it can just have a single colored line/border. So for graphs like scatter plots and line graphs, the color command would be used in the aesthetic specification-- and similarly color is referenced again in the subsequent lines of code where we manually color with our color values of choice.


#### Geom Point/Geom Line

```r
plot <- ggplot(df, aes(x = Petal.Length, y= Petal.Width, color = Species)) +  geom_point(aes(shape = Species)) + geom_smooth(method = lm) 

plot
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-57-1.png)<!-- -->

```r
plot <- plot + ggtitle("Petal Length and Width Plot") + labs(x = "Petal.Length", y = "Petal.Width") + theme_classic()  # Add Titles and Labels as well as Theme

plot <- plot + scale_color_manual(values = mycolors)  #Modify colors manually

plot
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-57-2.png)<!-- -->

Check out R Color Brewer for palettes and Color-Blind friendly color schemes:

http://colorbrewer2.org/#type=sequential&scheme=BuGn&n=3

Fun with Shapes: http://www.sthda.com/english/wiki/ggplot2-point-shapes


#### Geom Violin and Boxplot

If we would like to draw a geom_shape that you can "fill" with color, like a boxplot, histogram, violin plot, etc. then you use _fill_ instead of _color_ in the aesthetics command-- and similarly later when the colors are manually defined.


```r
ggplot(df, aes(x = Species, y = Petal.Width, fill = Species)) +
  geom_violin() +
   scale_fill_manual(values = mycolors)
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-58-1.png)<!-- -->

You may also overlap geoms on top of other geoms. If they are similar objects (i.e., are both filler objects), then an aesthetic command does not need to be redefined, and just inherits the global grouping definition


```r
ggplot(df, aes(x = Species, y = Petal.Width, fill = Species)) +
  geom_violin() +
   scale_fill_manual(values = mycolors) +
 geom_boxplot(width =.1) 
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-59-1.png)<!-- -->

And keep adding!


```r
#Violin plots
ggplot(df, aes(x=Species, y= Petal.Width, fill = Species)) +
  geom_violin() +
  scale_color_manual(values = mycolors) +
  geom_rug(aes(color = Species), show.legend = F, alpha = .9, position = "jitter", sides = "l") + 
  guides(fill = guide_legend(title = "Group")) +
  labs(title="Your title here :)", x = "Group", y = "Petal.Width") +
  geom_boxplot(width = 0.1, fill = "white") +
  scale_fill_manual(values = mycolors) + 
   theme_classic()
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-60-1.png)<!-- -->



```r
#Violin plots v2
ggplot(df, aes(x=Species, y= Petal.Width, group = Species)) +
  geom_violin(fill = "grey", width = 1) +
   geom_boxplot(aes(group = Species), fill= mycolors, width = 0.1) +
  geom_rug(aes(color = Species), alpha = .9, position = "jitter", sides = "l") + 
  scale_color_manual(values = mycolors)+
  labs(title="Your Centered title here", x = "Group", y = "Petal.Width") +
  theme_classic() +
  theme(plot.title = element_text(hjust = 0.5)) 
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-61-1.png)<!-- -->

# 5/22/19  Slightly Fancier Plots

The plots above are nice, but visualizations are more than just traditional, static approaches and have potential to show multiple relations/trends by using varying graphical approaches visualized simultaneously. 

This is more than layering points on a line or boxplot. Much of the packages reviewed here are based with ggplot commands and work well with tidyverse. The rest of the tutorial will only include these commands. It's likely though that with Base R, you'll still have most precision over a graphic. 

### Corrplot

I'll be creating a slightly complicated corrplot which is basically a visualization of a correlation matrix. That is, let's say we want to graph the correlation between our four numeric variables, but we also want to know the specific group correlations based on their wet and dry group category. 

For this, we will need to construct pieces of what we need, two matrices, then marge them together as one. Here I'm using the _corrplot package_


```r
#Make corrplot with only the Wet data

Wetcorrplot <- df %>%
      filter(rainfall == 'Wet') %>%
      select_if(is.numeric) %>%
      cor()


corrplot(Wetcorrplot, type = "upper", order = "hclust", 
         tl.col = "black", tl.srt = 45)
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-62-1.png)<!-- -->

```r
#Make similar one with Dry data

Drycorrplot <- df %>%
      filter(rainfall == 'Dry') %>%
      select_if(is.numeric) %>%
      cor()

corrplot(Drycorrplot, type = "lower", order = "hclust", 
         tl.col = "black", tl.srt = 45)
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-62-2.png)<!-- -->

It would be great if we could compare, yes? Thinking about these matrices, we could just replace the values of the lower Wet matrix with the values from the Dry matrix, creating a merged matrix-- then plot that, noting which upper or lower parts denote which group

First rename the Wetcorrplot into something new-



```r
mergedcorrplot <- Wetcorrplot

mergedcorrplot[lower.tri(mergedcorrplot)] <- Drycorrplot[lower.tri(Drycorrplot)]


# Will give you

mergedcorrplot
```

```
##              Sepal.Length Sepal.Width Petal.Length Petal.Width
## Sepal.Length   1.00000000  -0.1426525    0.8827376   0.8111109
## Sepal.Width   -0.08267591   1.0000000   -0.4294039  -0.3568522
## Petal.Length   0.85866688  -0.4329609    1.0000000   0.9590961
## Petal.Width    0.82775901  -0.3833991    0.9675820   1.0000000
```


Then you can graph this, using the full matrix display option


```r
corrplot(mergedcorrplot, type = "full")  #Wet on top and Dry on bottom
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-64-1.png)<!-- -->

```r
#Add additional tweaks for easier readability, depending on what you want-- many options!

corrplot(mergedcorrplot, type = "full", method = 'number', title = "Wet (top) and Dry (bottom)", tl.col = 'black', tl.srt = 45, col = c("blue", "black", "red"))
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-64-2.png)<!-- -->

Let's say we only want to display correlations that are significant at the p <.05 level. 

We will create new matrices of the P-values, first duplicating the Wet p-values taken from the Wetcor list. This becomes as the future merged matrix (this means the Wet p-values are in the upper triangle of the matrix). 

```r
wetmat1 <- df %>%
  filter(rainfall == 'Wet') %>%
  dplyr::select_if(is.numeric)
  

drymat1 <- df %>%
  filter(rainfall == 'Dry') %>%
  dplyr::select_if(is.numeric)


drymat1
```

```
##    Sepal.Length Sepal.Width Petal.Length Petal.Width
## 1           5.1         3.5          1.4         0.2
## 2           4.7         3.2          1.3         0.2
## 3           4.6         3.1          1.5         0.2
## 4           4.9         3.1          1.5         0.1
## 5           5.4         3.7          1.5         0.2
## 6           5.8         4.0          1.2         0.2
## 7           5.4         3.4          1.7         0.2
## 8           4.8         3.4          1.9         0.2
## 9           5.0         3.0          1.6         0.2
## 10          5.0         3.4          1.6         0.4
## 11          4.8         3.1          1.6         0.2
## 12          5.4         3.4          1.5         0.4
## 13          5.2         4.1          1.5         0.1
## 14          5.5         4.2          1.4         0.2
## 15          4.9         3.1          1.5         0.2
## 16          5.0         3.2          1.2         0.2
## 17          5.5         3.5          1.3         0.2
## 18          4.4         3.0          1.3         0.2
## 19          5.1         3.4          1.5         0.2
## 20          5.0         3.5          1.3         0.3
## 21          4.5         2.3          1.3         0.3
## 22          4.8         3.0          1.4         0.3
## 23          5.0         3.3          1.4         0.2
## 24          6.4         3.2          4.5         1.5
## 25          6.9         3.1          4.9         1.5
## 26          5.5         2.3          4.0         1.3
## 27          4.9         2.4          3.3         1.0
## 28          6.6         2.9          4.6         1.3
## 29          5.9         3.0          4.2         1.5
## 30          6.7         3.1          4.4         1.4
## 31          5.6         2.5          3.9         1.1
## 32          5.9         3.2          4.8         1.8
## 33          6.1         2.8          4.0         1.3
## 34          6.3         2.5          4.9         1.5
## 35          6.1         2.8          4.7         1.2
## 36          6.4         2.9          4.3         1.3
## 37          5.7         2.6          3.5         1.0
## 38          5.5         2.4          3.7         1.0
## 39          5.8         2.7          3.9         1.2
## 40          6.0         2.7          5.1         1.6
## 41          6.0         3.4          4.5         1.6
## 42          6.7         3.1          4.7         1.5
## 43          5.6         3.0          4.1         1.3
## 44          5.5         2.5          4.0         1.3
## 45          5.5         2.6          4.4         1.2
## 46          5.6         2.7          4.2         1.3
## 47          5.7         2.9          4.2         1.3
## 48          5.1         2.5          3.0         1.1
## 49          5.8         2.7          5.1         1.9
## 50          7.1         3.0          5.9         2.1
## 51          6.5         3.0          5.8         2.2
## 52          4.9         2.5          4.5         1.7
## 53          6.7         2.5          5.8         1.8
## 54          6.5         3.2          5.1         2.0
## 55          6.4         2.7          5.3         1.9
## 56          6.8         3.0          5.5         2.1
## 57          5.8         2.8          5.1         2.4
## 58          7.7         2.6          6.9         2.3
## 59          6.9         3.2          5.7         2.3
## 60          6.3         2.7          4.9         1.8
## 61          6.7         3.3          5.7         2.1
## 62          7.2         3.2          6.0         1.8
## 63          6.1         3.0          4.9         1.8
## 64          6.1         2.6          5.6         1.4
## 65          7.7         3.0          6.1         2.3
## 66          6.4         3.1          5.5         1.8
## 67          6.9         3.1          5.4         2.1
## 68          6.7         3.1          5.6         2.4
## 69          6.8         3.2          5.9         2.3
## 70          6.7         3.0          5.2         2.3
## 71          6.5         3.0          5.2         2.0
## 72          5.9         3.0          5.1         1.8
```

```r
#Now both are now numeric values in dataframes-- still need to be made into matrices

  
wetcors <- rcorr(as.matrix(wetmat1))  #Creates a list containing objects about the matrix's r-value, n of sample, and p-value or correlation

drycors <- rcorr(as.matrix(drymat1))


raincors <- wetcors$P #starting with making a clone

raincors
```

```
##              Sepal.Length  Sepal.Width Petal.Length Petal.Width
## Sepal.Length           NA 2.127956e-01 0.000000e+00 0.000000000
## Sepal.Width     0.2127956           NA 8.744102e-05 0.001341086
## Petal.Length    0.0000000 8.744102e-05           NA 0.000000000
## Petal.Width     0.0000000 1.341086e-03 0.000000e+00          NA
```

```r
raincors[lower.tri(raincors)] 
```

```
## [1] 2.127956e-01 0.000000e+00 0.000000e+00 8.744102e-05 1.341086e-03
## [6] 0.000000e+00
```

```r
drycors1 <-  drycors$P

drycors1
```

```
##              Sepal.Length  Sepal.Width Petal.Length  Petal.Width
## Sepal.Length           NA 0.4899213335  0.000000000 0.0000000000
## Sepal.Width     0.4899213           NA  0.000145523 0.0008863358
## Petal.Length    0.0000000 0.0001455230           NA 0.0000000000
## Petal.Width     0.0000000 0.0008863358  0.000000000           NA
```

```r
raincors[lower.tri(raincors)] <- drycors1[lower.tri(drycors1)]


raincors #merged p-values matrix
```

```
##              Sepal.Length  Sepal.Width Petal.Length Petal.Width
## Sepal.Length           NA 0.2127956140 0.000000e+00 0.000000000
## Sepal.Width     0.4899213           NA 8.744102e-05 0.001341086
## Petal.Length    0.0000000 0.0001455230           NA 0.000000000
## Petal.Width     0.0000000 0.0008863358 0.000000e+00          NA
```

So the drycors p-values were first turned into a matrix, before selecting out the lower triangle from it, and re-writing over the lower triangle of the merged p-value matrix.

Now the p-values along the top triangle belong to the Wet group and the ones along the bottom triangle belong to the Dry group. With this matrix now merged, it can be the "key" to mapping out significant values when the _corrplot()_ command is called. 

**Importantly, the merged matrix correlation values must match a separate, similarly grouped/merged correlation or r-value matrix to work**


```r
#the new pval matrix is set equal to the p.mat

corrplot(mergedcorrplot, type = "full", title = "Wet (top) and Dry (bottom)", tl.col = 'black', tl.srt = 45, p.mat = raincors, sig.level = 0.05, insig = "blank")
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-66-1.png)<!-- -->


Alternatively we can examine all of df by treating it all as a matrix


```r
dfnum <- df #Dropping participant ID

dfnum[c(5:7)] <- lapply(dfnum[c(5:7)], as.integer) #Make factors their integer values

dfnum
```

```
##     Sepal.Length Sepal.Width Petal.Length Petal.Width Species rainfall
## 1            5.1         3.5          1.4         0.2       1        1
## 2            4.9         3.0          1.4         0.2       1        2
## 3            4.7         3.2          1.3         0.2       1        1
## 4            4.6         3.1          1.5         0.2       1        1
## 5            5.0         3.6          1.4         0.2       1        2
## 6            5.4         3.9          1.7         0.4       1        2
## 7            4.6         3.4          1.4         0.3       1        2
## 8            5.0         3.4          1.5         0.2       1        2
## 9            4.4         2.9          1.4         0.2       1        2
## 10           4.9         3.1          1.5         0.1       1        1
## 11           5.4         3.7          1.5         0.2       1        1
## 12           4.8         3.4          1.6         0.2       1        2
## 13           4.8         3.0          1.4         0.1       1        2
## 14           4.3         3.0          1.1         0.1       1        2
## 15           5.8         4.0          1.2         0.2       1        1
## 16           5.7         4.4          1.5         0.4       1        2
## 17           5.4         3.9          1.3         0.4       1        2
## 18           5.1         3.5          1.4         0.3       1        2
## 19           5.7         3.8          1.7         0.3       1        2
## 20           5.1         3.8          1.5         0.3       1        2
## 21           5.4         3.4          1.7         0.2       1        1
## 22           5.1         3.7          1.5         0.4       1        2
## 23           4.6         3.6          1.0         0.2       1        2
## 24           5.1         3.3          1.7         0.5       1        2
## 25           4.8         3.4          1.9         0.2       1        1
## 26           5.0         3.0          1.6         0.2       1        1
## 27           5.0         3.4          1.6         0.4       1        1
## 28           5.2         3.5          1.5         0.2       1        2
## 29           5.2         3.4          1.4         0.2       1        2
## 30           4.7         3.2          1.6         0.2       1        2
## 31           4.8         3.1          1.6         0.2       1        1
## 32           5.4         3.4          1.5         0.4       1        1
## 33           5.2         4.1          1.5         0.1       1        1
## 34           5.5         4.2          1.4         0.2       1        1
## 35           4.9         3.1          1.5         0.2       1        1
## 36           5.0         3.2          1.2         0.2       1        1
## 37           5.5         3.5          1.3         0.2       1        1
## 38           4.9         3.6          1.4         0.1       1        2
## 39           4.4         3.0          1.3         0.2       1        1
## 40           5.1         3.4          1.5         0.2       1        1
## 41           5.0         3.5          1.3         0.3       1        1
## 42           4.5         2.3          1.3         0.3       1        1
## 43           4.4         3.2          1.3         0.2       1        2
## 44           5.0         3.5          1.6         0.6       1        2
## 45           5.1         3.8          1.9         0.4       1        2
## 46           4.8         3.0          1.4         0.3       1        1
## 47           5.1         3.8          1.6         0.2       1        2
## 48           4.6         3.2          1.4         0.2       1        2
## 49           5.3         3.7          1.5         0.2       1        2
## 50           5.0         3.3          1.4         0.2       1        1
## 51           7.0         3.2          4.7         1.4       2        2
## 52           6.4         3.2          4.5         1.5       2        1
## 53           6.9         3.1          4.9         1.5       2        1
## 54           5.5         2.3          4.0         1.3       2        1
## 55           6.5         2.8          4.6         1.5       2        2
## 56           5.7         2.8          4.5         1.3       2        2
## 57           6.3         3.3          4.7         1.6       2        2
## 58           4.9         2.4          3.3         1.0       2        1
## 59           6.6         2.9          4.6         1.3       2        1
## 60           5.2         2.7          3.9         1.4       2        2
## 61           5.0         2.0          3.5         1.0       2        2
## 62           5.9         3.0          4.2         1.5       2        1
## 63           6.0         2.2          4.0         1.0       2        2
## 64           6.1         2.9          4.7         1.4       2        2
## 65           5.6         2.9          3.6         1.3       2        2
## 66           6.7         3.1          4.4         1.4       2        1
## 67           5.6         3.0          4.5         1.5       2        2
## 68           5.8         2.7          4.1         1.0       2        2
## 69           6.2         2.2          4.5         1.5       2        2
## 70           5.6         2.5          3.9         1.1       2        1
## 71           5.9         3.2          4.8         1.8       2        1
## 72           6.1         2.8          4.0         1.3       2        1
## 73           6.3         2.5          4.9         1.5       2        1
## 74           6.1         2.8          4.7         1.2       2        1
## 75           6.4         2.9          4.3         1.3       2        1
## 76           6.6         3.0          4.4         1.4       2        2
## 77           6.8         2.8          4.8         1.4       2        2
## 78           6.7         3.0          5.0         1.7       2        2
## 79           6.0         2.9          4.5         1.5       2        2
## 80           5.7         2.6          3.5         1.0       2        1
## 81           5.5         2.4          3.8         1.1       2        2
## 82           5.5         2.4          3.7         1.0       2        1
## 83           5.8         2.7          3.9         1.2       2        1
## 84           6.0         2.7          5.1         1.6       2        1
## 85           5.4         3.0          4.5         1.5       2        2
## 86           6.0         3.4          4.5         1.6       2        1
## 87           6.7         3.1          4.7         1.5       2        1
## 88           6.3         2.3          4.4         1.3       2        2
## 89           5.6         3.0          4.1         1.3       2        1
## 90           5.5         2.5          4.0         1.3       2        1
## 91           5.5         2.6          4.4         1.2       2        1
## 92           6.1         3.0          4.6         1.4       2        2
## 93           5.8         2.6          4.0         1.2       2        2
## 94           5.0         2.3          3.3         1.0       2        2
## 95           5.6         2.7          4.2         1.3       2        1
## 96           5.7         3.0          4.2         1.2       2        2
## 97           5.7         2.9          4.2         1.3       2        1
## 98           6.2         2.9          4.3         1.3       2        2
## 99           5.1         2.5          3.0         1.1       2        1
## 100          5.7         2.8          4.1         1.3       2        2
## 101          6.3         3.3          6.0         2.5       3        2
## 102          5.8         2.7          5.1         1.9       3        1
## 103          7.1         3.0          5.9         2.1       3        1
## 104          6.3         2.9          5.6         1.8       3        2
## 105          6.5         3.0          5.8         2.2       3        1
## 106          7.6         3.0          6.6         2.1       3        2
## 107          4.9         2.5          4.5         1.7       3        1
## 108          7.3         2.9          6.3         1.8       3        2
## 109          6.7         2.5          5.8         1.8       3        1
## 110          7.2         3.6          6.1         2.5       3        2
## 111          6.5         3.2          5.1         2.0       3        1
## 112          6.4         2.7          5.3         1.9       3        1
## 113          6.8         3.0          5.5         2.1       3        1
## 114          5.7         2.5          5.0         2.0       3        2
## 115          5.8         2.8          5.1         2.4       3        1
## 116          6.4         3.2          5.3         2.3       3        2
## 117          6.5         3.0          5.5         1.8       3        2
## 118          7.7         3.8          6.7         2.2       3        2
## 119          7.7         2.6          6.9         2.3       3        1
## 120          6.0         2.2          5.0         1.5       3        2
## 121          6.9         3.2          5.7         2.3       3        1
## 122          5.6         2.8          4.9         2.0       3        2
## 123          7.7         2.8          6.7         2.0       3        2
## 124          6.3         2.7          4.9         1.8       3        1
## 125          6.7         3.3          5.7         2.1       3        1
## 126          7.2         3.2          6.0         1.8       3        1
## 127          6.2         2.8          4.8         1.8       3        2
## 128          6.1         3.0          4.9         1.8       3        1
## 129          6.4         2.8          5.6         2.1       3        2
## 130          7.2         3.0          5.8         1.6       3        2
## 131          7.4         2.8          6.1         1.9       3        2
## 132          7.9         3.8          6.4         2.0       3        2
## 133          6.4         2.8          5.6         2.2       3        2
## 134          6.3         2.8          5.1         1.5       3        2
## 135          6.1         2.6          5.6         1.4       3        1
## 136          7.7         3.0          6.1         2.3       3        1
## 137          6.3         3.4          5.6         2.4       3        2
## 138          6.4         3.1          5.5         1.8       3        1
## 139          6.0         3.0          4.8         1.8       3        2
## 140          6.9         3.1          5.4         2.1       3        1
## 141          6.7         3.1          5.6         2.4       3        1
## 142          6.9         3.1          5.1         2.3       3        2
## 143          5.8         2.7          5.1         1.9       3        2
## 144          6.8         3.2          5.9         2.3       3        1
## 145          6.7         3.3          5.7         2.5       3        2
## 146          6.7         3.0          5.2         2.3       3        1
## 147          6.3         2.5          5.0         1.9       3        2
## 148          6.5         3.0          5.2         2.0       3        1
## 149          6.2         3.4          5.4         2.3       3        2
## 150          5.9         3.0          5.1         1.8       3        1
##     fert
## 1      1
## 2      2
## 3      1
## 4      1
## 5      1
## 6      2
## 7      1
## 8      1
## 9      2
## 10     2
## 11     2
## 12     1
## 13     2
## 14     1
## 15     1
## 16     1
## 17     2
## 18     2
## 19     1
## 20     2
## 21     1
## 22     2
## 23     2
## 24     2
## 25     2
## 26     2
## 27     2
## 28     1
## 29     2
## 30     1
## 31     2
## 32     1
## 33     2
## 34     1
## 35     1
## 36     2
## 37     1
## 38     1
## 39     2
## 40     1
## 41     2
## 42     2
## 43     2
## 44     1
## 45     1
## 46     1
## 47     1
## 48     1
## 49     2
## 50     1
## 51     1
## 52     2
## 53     2
## 54     1
## 55     2
## 56     1
## 57     2
## 58     2
## 59     1
## 60     2
## 61     1
## 62     1
## 63     1
## 64     2
## 65     1
## 66     2
## 67     1
## 68     2
## 69     2
## 70     2
## 71     1
## 72     1
## 73     1
## 74     1
## 75     1
## 76     1
## 77     1
## 78     1
## 79     2
## 80     2
## 81     2
## 82     1
## 83     2
## 84     2
## 85     1
## 86     1
## 87     2
## 88     2
## 89     2
## 90     2
## 91     1
## 92     2
## 93     2
## 94     2
## 95     2
## 96     2
## 97     2
## 98     2
## 99     2
## 100    1
## 101    2
## 102    2
## 103    2
## 104    1
## 105    2
## 106    1
## 107    2
## 108    1
## 109    2
## 110    1
## 111    1
## 112    2
## 113    2
## 114    1
## 115    1
## 116    2
## 117    2
## 118    2
## 119    1
## 120    2
## 121    2
## 122    1
## 123    2
## 124    2
## 125    2
## 126    1
## 127    1
## 128    1
## 129    2
## 130    1
## 131    1
## 132    1
## 133    1
## 134    1
## 135    2
## 136    1
## 137    2
## 138    2
## 139    2
## 140    1
## 141    1
## 142    1
## 143    1
## 144    2
## 145    1
## 146    1
## 147    1
## 148    2
## 149    1
## 150    1
```

```r
Binarycor <- cor(as.matrix(dfnum))

corrplot(Binarycor)
```

![](C:\Users\Jonni\Desktop\RCLASS~1\INTRO-~1\docs\INDEX_~1/figure-html/unnamed-chunk-67-1.png)<!-- -->

```r
Binarycorpval <-  rcorr(as.matrix(dfnum))

Binarycorpval$P 
```

```
##              Sepal.Length  Sepal.Width Petal.Length  Petal.Width
## Sepal.Length           NA 1.518983e-01 0.000000e+00 0.000000e+00
## Sepal.Width     0.1518983           NA 4.513314e-08 4.073229e-06
## Petal.Length    0.0000000 4.513314e-08           NA 0.000000e+00
## Petal.Width     0.0000000 4.073229e-06 0.000000e+00           NA
## Species         0.0000000 5.201563e-08 0.000000e+00 0.000000e+00
## rainfall        0.9718036 2.737137e-01 9.688459e-01 9.238605e-01
## fert            0.2929378 1.782768e-01 8.431262e-01 7.572252e-01
##                   Species  rainfall      fert
## Sepal.Length 0.000000e+00 0.9718036 0.2929378
## Sepal.Width  5.201563e-08 0.2737137 0.1782768
## Petal.Length 0.000000e+00 0.9688459 0.8431262
## Petal.Width  0.000000e+00 0.9238605 0.7572252
## Species                NA 0.8426547 0.8427790
## rainfall     8.426547e-01        NA 0.3300852
## fert         8.427790e-01 0.3300852        NA
```
It isn't really accurate to run a correlation analysis on the dichotomous variables, but you can quickly visualize things to spot large issues


### GGpairs
