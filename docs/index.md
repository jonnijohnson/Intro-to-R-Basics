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

I plead with people to learn R as it makes working with data easier than traditional programs, and everything can be down in a single program (i.e., data entry, cleaning, analyses, graphing). 

I had a steep learning curve and often hear that time to learn R is often what stops them. I created the following tutorial that covers some introductory data exploration, analysis, and graphing-- to hopefully show others that R is not that intimidating

</br>

### Packages and Libraries....

...We use a lot of them. Make sure that the packages are installed first using the command function: _install.packages("Name of the package")_. Packages include:

install.packages("tidyverse") -- This loads dplyr, plyr, tidyr at once, no need for multiple packages- Note 'select()' function argument is masked in dplyr

install.packages("psych")

install.packages("tableone")

install.packages("tadaatoolbox")


library(tidyverse)

library(psych)

library(tableone)

library(magrittr) -- should be built in, if not install.packages

library(car) -- is a built in package in Base R, no need to install anything special

library(tadaatoolbox) -- Only needed for making pretty Rmarkdown tables





### Loading in Data

Most of the time we will be loading in CSV files. But you can read in other types of files, SPSS, TXT, using other base argument commands. You can also load data from a browser, but that includes arguments not presented below. The data file, whatever its extension, should be saved in a folder which will become your _working directory_. It is a good idea to save scripts/markdowns, figures, and data sets in the same folder or parent folder for easy navigation. 

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


Once the data file is read, you'll want to assign it a name so that R saves it to its working environment


```r
matching <- read.csv("Matching_sample.csv", header = T, stringsAsFactors = F)

str(matching)  # Calling for the structure of this object
```

```
## 'data.frame':	96 obs. of  9 variables:
##  $ X                  : chr  "1" "1" "1" "1" ...
##  $ PTs                : chr  "022A" "038A" "045A_V2" "046A" ...
##  $ Subjects           : chr  "022A" "038A" "045A_V2" "046A" ...
##  $ RMSD..5mm          : num  0.032 0.11 0.03 0.021 0.101 0.024 0.019 0.106 0.077 0.051 ...
##  $ Age                : num  14.1 13.2 17.3 17.1 15.1 13.8 16.5 13.8 15.3 12.9 ...
##  $ WASI_NVIQ          : num  140 103 106 126 121 112 104 127 129 100 ...
##  $ Gender             : num  1 1 2 1 1 1 1 1 1 1 ...
##  $ Handedness         : num  1 2 1 1 1 2 1 1 1 1 ...
##  $ X1.ASD.m.R.2.TD.f.L: chr  "" "" "" "" ...
```

This file is loaded from the local C: drive, but you can also load directly from googlesheets

It is similar to the above dataframe, but some variables have been coerced into a double type versus numeric or character. This is fine, just means they have to be manually changed later. For the rest of this markdown, I will just be working with the first matching file.  But it is otherwise useful to know that the below can be used when your data are housed online and accessed with a URL

##### Code does not fit with R Markdown but does in individual scripts

install.packages("googlesheets")
library(googlesheets)

online <- gs_url("https://docs.google.com/spreadsheets/d/1yXNJgcvfCVb_b-dx1-ZTE_siuTQA1N4HsbKuA4L_4ns/edit#gid=782498555")

onlinedf <- gs_read_csv(online, header = T, stringAsFactors = F)

str(onlinedf)




### Basic Viewing/Exploring Commands

|This Argument.....  | ...... Performs This Function                                            |
|--------------------|--------------------------------------------------------------------------|
| _str()_            | Call for the 'str'ucture of an object, if a dataframe it return information about each column variable type, factor levels (if app), and number of variables and observations in the object/dataframe|
| _head()_           | Calls for the first 6 rows or heading of the dataframe                   |
| _tail()_           | Calls for the last 6 rows or tail end of data                            |
| _dim()_            | Calls for the dimensions of an object-- more useful for lists, matrices, and tables|
| _class()_          | Asking for how R is classifying an object (e.g., character, logic, number, integer, string, factor, double, dataframe)|
| _$_                 | This operator command can be used with the name of the dataframe to call a column of interest by name. Such as _matching$ColumnName_ |
| _view()_            | Useful to "see" what data look like in a traditional excel-type format, opens Viewer window | 


In this dataset, there appears to be some unnecessary columns and rows-- perhaps used for a temporary purpose or totalling in excel. It is common for excel files to have row entries be sums or means, as is the case here. Let's get rid of these rows.

We can see in the viewer that rows 1-40 and 51-88 are where our data are, but we otherwise have 96 rows. 

We'll create a new dataframe, **'matching2'** that only has the data we are interested in-- it is a good habit to not overwrite the original dataset read in-- that way you can always return to see if you need any additional information/variables without having to re-read the file in.




```r
matching2 <- matching[c(1:40,51:88),]  # "Create a new dataframe called 'matching2'; In it, take rows 1 through 40 and 51 through 88, and bring ALL columns"


str(matching2)
```

```
## 'data.frame':	78 obs. of  9 variables:
##  $ X                  : chr  "1" "1" "1" "1" ...
##  $ PTs                : chr  "022A" "038A" "045A_V2" "046A" ...
##  $ Subjects           : chr  "022A" "038A" "045A_V2" "046A" ...
##  $ RMSD..5mm          : num  0.032 0.11 0.03 0.021 0.101 0.024 0.019 0.106 0.077 0.051 ...
##  $ Age                : num  14.1 13.2 17.3 17.1 15.1 13.8 16.5 13.8 15.3 12.9 ...
##  $ WASI_NVIQ          : num  140 103 106 126 121 112 104 127 129 100 ...
##  $ Gender             : num  1 1 2 1 1 1 1 1 1 1 ...
##  $ Handedness         : num  1 2 1 1 1 2 1 1 1 1 ...
##  $ X1.ASD.m.R.2.TD.f.L: chr  "" "" "" "" ...
```

The _[  ]_ tell R to "look here" in this referenced spot. 

Within brackets syntax are **always** [ _Rows, Columns_ ]. R tells rows from columns be detecting the ','   

Above, we use the _c()_ command to let R know that the things in parentheses are apart of the same series; therefore, R knows not to treat the comma within the parentheses as specifying the columns. By leaving an empty space after the comma and not specifying columns, we are saying "All of the columns"

Alternatively if there is only one thing within the brackets _without a comma_, R will assume it is the column position. 

So matching[9] would be the equivalent of saying "Print the 9th column in dataframe matching" whereas matching[9,] says, "Print row 9 all columns"


Now we have a dataframe matching2. 

Using the _str()_ command you can see that now we have fewer observations. It also appears as if columns 2 and 3 potentially are the same information. Also column 9 doesn't seem informative/consistently used/excel artifact

To check that columns 2 and 3 are identical we would use the _==_ operator meaning "is equal to" -- R will print a logic output saying either TRUE or FALSE, where columns 2 and 3 either match or differ, respectively. 

Below we are using the _$_ operator to individually select the columns by name.


```r
matching2$PTs == matching2$Subjects
```

```
##  [1]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
## [12]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE  TRUE
## [23]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
## [34]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
## [45]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
## [56]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
## [67]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
## [78]  TRUE
```

```r
str(matching2)
```

```
## 'data.frame':	78 obs. of  9 variables:
##  $ X                  : chr  "1" "1" "1" "1" ...
##  $ PTs                : chr  "022A" "038A" "045A_V2" "046A" ...
##  $ Subjects           : chr  "022A" "038A" "045A_V2" "046A" ...
##  $ RMSD..5mm          : num  0.032 0.11 0.03 0.021 0.101 0.024 0.019 0.106 0.077 0.051 ...
##  $ Age                : num  14.1 13.2 17.3 17.1 15.1 13.8 16.5 13.8 15.3 12.9 ...
##  $ WASI_NVIQ          : num  140 103 106 126 121 112 104 127 129 100 ...
##  $ Gender             : num  1 1 2 1 1 1 1 1 1 1 ...
##  $ Handedness         : num  1 2 1 1 1 2 1 1 1 1 ...
##  $ X1.ASD.m.R.2.TD.f.L: chr  "" "" "" "" ...
```

```r
# Could also say:

matching2[2] == matching2[3] # There's one False!
```

```
##      PTs
## 1   TRUE
## 2   TRUE
## 3   TRUE
## 4   TRUE
## 5   TRUE
## 6   TRUE
## 7   TRUE
## 8   TRUE
## 9   TRUE
## 10  TRUE
## 11  TRUE
## 12  TRUE
## 13  TRUE
## 14  TRUE
## 15  TRUE
## 16  TRUE
## 17  TRUE
## 18  TRUE
## 19  TRUE
## 20  TRUE
## 21 FALSE
## 22  TRUE
## 23  TRUE
## 24  TRUE
## 25  TRUE
## 26  TRUE
## 27  TRUE
## 28  TRUE
## 29  TRUE
## 30  TRUE
## 31  TRUE
## 32  TRUE
## 33  TRUE
## 34  TRUE
## 35  TRUE
## 36  TRUE
## 37  TRUE
## 38  TRUE
## 39  TRUE
## 40  TRUE
## 51  TRUE
## 52  TRUE
## 53  TRUE
## 54  TRUE
## 55  TRUE
## 56  TRUE
## 57  TRUE
## 58  TRUE
## 59  TRUE
## 60  TRUE
## 61  TRUE
## 62  TRUE
## 63  TRUE
## 64  TRUE
## 65  TRUE
## 66  TRUE
## 67  TRUE
## 68  TRUE
## 69  TRUE
## 70  TRUE
## 71  TRUE
## 72  TRUE
## 73  TRUE
## 74  TRUE
## 75  TRUE
## 76  TRUE
## 77  TRUE
## 78  TRUE
## 79  TRUE
## 80  TRUE
## 81  TRUE
## 82  TRUE
## 83  TRUE
## 84  TRUE
## 85  TRUE
## 86  TRUE
## 87  TRUE
## 88  TRUE
```

If there were a larger dataset, you likely wouldn't want to eyeball. To find the row entry that doesn't match, we can use _which()_ and to print the contents of what's there we just index it using brackets. Since we know we want columns 2 and 3, we can concatenate them together and select out the entry. 

The which command lets us know if it in row 21 and uses the _!=_ symbols to indicate "Does not equal" - so the command below reads as "Which entries in columns 2 and 3 of matching2 are not equal"


```r
which(matching2$PTs != matching2$Subjects)
```

```
## [1] 21
```

```r
matching2[which(matching2$PTs != matching2$Subjects),]
```

```
##    X     PTs Subjects RMSD..5mm Age WASI_NVIQ Gender Handedness
## 21 1 159A_V2     159A     0.036  15       123      2          2
##    X1.ASD.m.R.2.TD.f.L
## 21
```
If this means something to us, we can decide what to do. One option is to overwrite the entry. Let's say that _V2 was some kind of data entry error. We can fix it by reassigning the value to just be "159A"


```r
matching2[21,2] <- "159A"  #In row 21, column 2, overwrite entry to read as "159A"

#Double Check

matching2[21,]
```

```
##    X  PTs Subjects RMSD..5mm Age WASI_NVIQ Gender Handedness
## 21 1 159A     159A     0.036  15       123      2          2
##    X1.ASD.m.R.2.TD.f.L
## 21
```

In reality, we are going to drop this column as it is redundant with column 3. We also don't want whatever column 9 is. We can drop things by  using minus sign  _-_ prior to a _c()_


```r
matching2 <- matching2[-c(2,9)]  #We are overwrite the existing matching2, but matching still exists.
```

_NOTE_ Because there is no comma above outside of the c() argument, R reads 2 and 9 as columns. We could also write as:

matching2[,-c(2,9)], if we wanted to retain all the rows.


We also could have subsetted these data in a single command with:

_matching2 <- matching[c(1:40,51:88), -c(2,9)]_


```r
str(matching) #Compare original
```

```
## 'data.frame':	96 obs. of  9 variables:
##  $ X                  : chr  "1" "1" "1" "1" ...
##  $ PTs                : chr  "022A" "038A" "045A_V2" "046A" ...
##  $ Subjects           : chr  "022A" "038A" "045A_V2" "046A" ...
##  $ RMSD..5mm          : num  0.032 0.11 0.03 0.021 0.101 0.024 0.019 0.106 0.077 0.051 ...
##  $ Age                : num  14.1 13.2 17.3 17.1 15.1 13.8 16.5 13.8 15.3 12.9 ...
##  $ WASI_NVIQ          : num  140 103 106 126 121 112 104 127 129 100 ...
##  $ Gender             : num  1 1 2 1 1 1 1 1 1 1 ...
##  $ Handedness         : num  1 2 1 1 1 2 1 1 1 1 ...
##  $ X1.ASD.m.R.2.TD.f.L: chr  "" "" "" "" ...
```

```r
str(matching2)  #To cleaner version
```

```
## 'data.frame':	78 obs. of  7 variables:
##  $ X         : chr  "1" "1" "1" "1" ...
##  $ Subjects  : chr  "022A" "038A" "045A_V2" "046A" ...
##  $ RMSD..5mm : num  0.032 0.11 0.03 0.021 0.101 0.024 0.019 0.106 0.077 0.051 ...
##  $ Age       : num  14.1 13.2 17.3 17.1 15.1 13.8 16.5 13.8 15.3 12.9 ...
##  $ WASI_NVIQ : num  140 103 106 126 121 112 104 127 129 100 ...
##  $ Gender    : num  1 1 2 1 1 1 1 1 1 1 ...
##  $ Handedness: num  1 2 1 1 1 2 1 1 1 1 ...
```

```r
head(matching2) # Get a glimpse~
```

```
##   X Subjects RMSD..5mm  Age WASI_NVIQ Gender Handedness
## 1 1     022A     0.032 14.1       140      1          1
## 2 1     038A     0.110 13.2       103      1          2
## 3 1  045A_V2     0.030 17.3       106      2          1
## 4 1     046A     0.021 17.1       126      1          1
## 5 1     056A     0.101 15.1       121      1          1
## 6 1     071A     0.024 13.8       112      1          2
```


### Data Modification

Gender and Handedness were originally coded as 1's and 2's, where males were coded as 1 and females as 2. Similarly for handedness, 1 = Right handed and 2 = Left handed. The X variable is whether the participants have autism (ASD) or are typically developing (TD).

We will need to overwrite these variables which are presently viewed as numbers and tell R that these are really _factors_ or a categorical variable.

We could do this individually for our columns 1,6, and 7 -- Diagnosis, Gender, and Handedness. 


```r
matching2$X <- factor(matching2$X, labels = c("ASD", "TD"))

matching2$X #check
```

```
##  [1] ASD ASD ASD ASD ASD ASD ASD ASD ASD ASD ASD ASD ASD ASD ASD ASD ASD
## [18] ASD ASD ASD ASD ASD ASD ASD ASD ASD ASD ASD ASD ASD ASD ASD ASD ASD
## [35] ASD ASD ASD ASD ASD ASD TD  TD  TD  TD  TD  TD  TD  TD  TD  TD  TD 
## [52] TD  TD  TD  TD  TD  TD  TD  TD  TD  TD  TD  TD  TD  TD  TD  TD  TD 
## [69] TD  TD  TD  TD  TD  TD  TD  TD  TD  TD 
## Levels: ASD TD
```

```r
# Below is commented out and not run; however, would be how you would reassign these variables. 


#  matching2$Gender <- factor(matching2$Gender, labels = c("Male", "Female"))
#  matching2$Handedness <- factor(matching2$Handedness, labels = c("Right", "Left"))
```


Or use _lapply()_  

_lapply_ is essentially saying apply the following command _as.factor_ and returns a list object, in this case, a factor. The reassignment is like saying: "In matching2, columns 6 and 7, reassign to this value. This value should take all contents of what's in matching2 columns 6 and 7 ( _which presently are just numbers_ ), and make them into factors"


```r
matching2$Gender # 1 = Male and 2 = Female
```

```
##  [1] 1 1 2 1 1 1 1 1 1 1 1 2 1 1 1 1 1 1 2 1 2 1 2 1 2 1 1 1 1 2 1 1 2 1 1
## [36] 1 1 1 1 1 1 1 1 2 1 1 1 1 1 2 1 1 2 2 1 1 1 1 1 1 2 1 1 1 1 1 1 1 1 1
## [71] 1 1 1 1 1 1 1 1
```

```r
matching2$Handedness # 1 = Right and 2 = Left
```

```
##  [1] 1 2 1 1 1 2 1 1 1 1 1 1 1 1 1 2 1 1 1 2 2 1 1 1 1 1 2 1 1 1 1 1 1 1 1
## [36] 2 1 1 1 1 2 1 1 1 1 1 1 1 1 1 2 1 1 1 1 2 1 1 1 1 1 1 1 1 1 2 2 1 1 1
## [71] 1 1 1 1 1 1 1 1
```

```r
matching2[c(6,7)] <- lapply(matching2[c(6,7)], as.factor)

str(matching2[c(1,6,7)])
```

```
## 'data.frame':	78 obs. of  3 variables:
##  $ X         : Factor w/ 2 levels "ASD","TD": 1 1 1 1 1 1 1 1 1 1 ...
##  $ Gender    : Factor w/ 2 levels "1","2": 1 1 2 1 1 1 1 1 1 1 ...
##  $ Handedness: Factor w/ 2 levels "1","2": 1 2 1 1 1 2 1 1 1 1 ...
```

You'll notice that although Gender and Handedness are now factors like our X variable, they are still listed as 1's and 2's.

When we changed X to be a factor above, we also supplied a _labels()_ argument and specified that it was "ASD" and "TD"

**Order is _very_ important**

The order that these are listed is how R determines what the 1 equals, 2 equals, etc.

Calling for the _levels()_ of variable X reports that these are ASD and TD but for gender these are 1 and 2


```r
levels(matching2$X)
```

```
## [1] "ASD" "TD"
```

```r
levels(matching2$Gender)
```

```
## [1] "1" "2"
```

We need to "assign" labels to these levels. We are **not** changing the levels themselves, and since there currently are no labels, we cannot specify that without R printing an error message about labels not existing.

For this, we will need to say _levels()_ of the variable of interest and then assign a concatenated string as our labels. We do not use the _labels_ command here.



```r
levels(matching2$Gender) <- c("Male", "Female")
levels(matching2$Handedness) <- c("Right", "Left")

str(matching2)
```

```
## 'data.frame':	78 obs. of  7 variables:
##  $ X         : Factor w/ 2 levels "ASD","TD": 1 1 1 1 1 1 1 1 1 1 ...
##  $ Subjects  : chr  "022A" "038A" "045A_V2" "046A" ...
##  $ RMSD..5mm : num  0.032 0.11 0.03 0.021 0.101 0.024 0.019 0.106 0.077 0.051 ...
##  $ Age       : num  14.1 13.2 17.3 17.1 15.1 13.8 16.5 13.8 15.3 12.9 ...
##  $ WASI_NVIQ : num  140 103 106 126 121 112 104 127 129 100 ...
##  $ Gender    : Factor w/ 2 levels "Male","Female": 1 1 2 1 1 1 1 1 1 1 ...
##  $ Handedness: Factor w/ 2 levels "Right","Left": 1 2 1 1 1 2 1 1 1 1 ...
```

```r
head(matching2)
```

```
##     X Subjects RMSD..5mm  Age WASI_NVIQ Gender Handedness
## 1 ASD     022A     0.032 14.1       140   Male      Right
## 2 ASD     038A     0.110 13.2       103   Male       Left
## 3 ASD  045A_V2     0.030 17.3       106 Female      Right
## 4 ASD     046A     0.021 17.1       126   Male      Right
## 5 ASD     056A     0.101 15.1       121   Male      Right
## 6 ASD     071A     0.024 13.8       112   Male       Left
```

```r
tail(matching2) # Everything looks good!
```

```
##     X Subjects RMSD..5mm  Age WASI_NVIQ Gender Handedness
## 83 TD     402C     0.056 16.1        97   Male      Right
## 84 TD     407C     0.059 14.8        90   Male      Right
## 85 TD     415C     0.035 13.6       125   Male      Right
## 86 TD  418C_V2     0.082  9.2       112   Male      Right
## 87 TD     433C     0.088 14.1       112   Male      Right
## 88 TD     434C     0.084 12.2       122   Male      Right
```

Last few modifications we could consider are the variable names. Sometimes, variables have verbose names, and sometimes they are more ambiguous. Using _colnames()_  or simply _names()_ with the dataframe of interest inside will print. 



```r
colnames(matching2)
```

```
## [1] "X"          "Subjects"   "RMSD..5mm"  "Age"        "WASI_NVIQ" 
## [6] "Gender"     "Handedness"
```

```r
names(matching2)
```

```
## [1] "X"          "Subjects"   "RMSD..5mm"  "Age"        "WASI_NVIQ" 
## [6] "Gender"     "Handedness"
```

We can also assign these names all at once    **Not recommended for large datasets**

We can create a new object-- a character vector, then assign this character vector to the column names of the data frame


```r
newnames <- c("V1", "V2", "V3", "V4", "V5", "V6", "V7")

colnames(matching2) <- newnames

colnames(matching2)
```

```
## [1] "V1" "V2" "V3" "V4" "V5" "V6" "V7"
```
This would be very tedious with large data sets. Let's change back. Just re-assign it!


```r
oldnames <- c("X", "Subjects", "RMSD..5mm", "Age", "WASI_NVIQ", "Gender", "Handedness")

colnames(matching2) <- oldnames

names(matching2)
```

```
## [1] "X"          "Subjects"   "RMSD..5mm"  "Age"        "WASI_NVIQ" 
## [6] "Gender"     "Handedness"
```


While X might mean something to us, to someone else they might not get it. Also the "RMSD..5mm" is a bit annoying to type. We can just rename these columns specifically using _[  ]_ to index.


```r
colnames(matching2)[c(1,3)] <- c("Diagnosis", "RMSD")

names(matching2)
```

```
## [1] "Diagnosis"  "Subjects"   "RMSD"       "Age"        "WASI_NVIQ" 
## [6] "Gender"     "Handedness"
```


Now we are ready to check some assumptions and see if our samples are matched on our variables of interest: RMSD, Age, IQ, Gender, and Handedness

# 5/8/19: Matching Basics 


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


To start-- there is the base R _summary_ command. Calling this on our dataframe, matching2, will count factors, measure character lengths, and quartiles/means/medians of numeric variables. 


```r
summary(matching2)
```

```
##  Diagnosis   Subjects              RMSD              Age       
##  ASD:40    Length:78          Min.   :0.01900   Min.   : 7.40  
##  TD :38    Class :character   1st Qu.:0.03650   1st Qu.:11.57  
##            Mode  :character   Median :0.05850   Median :13.85  
##                               Mean   :0.06017   Mean   :13.66  
##                               3rd Qu.:0.07875   3rd Qu.:15.88  
##                               Max.   :0.12600   Max.   :18.00  
##    WASI_NVIQ        Gender   Handedness
##  Min.   : 53.0   Male  :65   Right:66  
##  1st Qu.: 97.0   Female:13   Left :12  
##  Median :104.5                         
##  Mean   :105.8                         
##  3rd Qu.:118.0                         
##  Max.   :140.0
```

Continuing the base R way, if you want to look at some summaries across groups, say comparing ASD and TD, you can use _aggregate_ and _tapply_ commands.

For each of these examples, R requires that numeric variables be treated as numbers and factors are not accepted as numbers-- but are categories. Consider this if you have 'non-numeric' error warnings and R is trying to average a category/factor.


This formula is using aggregate's built in algorithm: _aggregate(df$frequencyVaraible, by=list(df$CategoryVariable), FUN = sum/mean/sd/etc.)_


```r
aggregate(RMSD ~Diagnosis, matching2, mean)  # is the same as
```

```
##   Diagnosis       RMSD
## 1       ASD 0.06260000
## 2        TD 0.05760526
```

```r
aggregate(matching2$RMSD, by=list(matching2$Diagnosis), FUN = mean )
```

```
##   Group.1          x
## 1     ASD 0.06260000
## 2      TD 0.05760526
```

```r
aggregate(RMSD ~ Diagnosis + Gender, matching2, mean) # Additional categories can be added to the formula
```

```
##   Diagnosis Gender       RMSD
## 1       ASD   Male 0.06153125
## 2        TD   Male 0.05596970
## 3       ASD Female 0.06687500
## 4        TD Female 0.06840000
```

Or we could use _tapply_ 


```r
tapply(matching2$RMSD, matching2$Diagnosis, FUN = mean)
```

```
##        ASD         TD 
## 0.06260000 0.05760526
```

Multiple summary statistics? There's a function for that.


```r
tapply(matching2$RMSD, matching2$Diagnosis, FUN = function(x) c( MN= mean(x), SD = sd(x)))
```

```
## $ASD
##         MN         SD 
## 0.06260000 0.02696512 
## 
## $TD
##         MN         SD 
## 0.05760526 0.02695596
```


This individual/self-defined function also works with aggregate command as well. Here the _with()_ begins specifying the dataframe, so we don't need to specify this later in the aggregate formula


```r
with(matching2, aggregate(RMSD ~ Diagnosis + Gender, FUN = function(x) c(MN = mean(x), SD = sd(x))))
```

```
##   Diagnosis Gender    RMSD.MN    RMSD.SD
## 1       ASD   Male 0.06153125 0.02697787
## 2        TD   Male 0.05596970 0.02559600
## 3       ASD Female 0.06687500 0.02831677
## 4        TD Female 0.06840000 0.03622568
```


Here we are using some tidyr commands and the **%>%** operator. This lets R know to "keep going" as a flow from the previous line to the next. It saves the additional writing from Base R commands of calling the dataframe each time we reference a column

The tableone package is handy for providing descriptive statistics on each variable. However, because some of our variables are factors, R will count them as categories.

Below is saying:  "Create the object tabave. Take matching2 -- then group by Diagnosis -- summarize the variable if it is numeric, and return the mean and standard deviation"

The initial assignment only assigns-- to see what we created-- call it's name~


```r
tabave <- matching2 %>%
            group_by(Diagnosis) %>%
            summarise_if(is.numeric, c(mean, sd))
tabave
```

```
## # A tibble: 2 x 7
##   Diagnosis RMSD_fn1 Age_fn1 WASI_NVIQ_fn1 RMSD_fn2 Age_fn2 WASI_NVIQ_fn2
##   <fct>        <dbl>   <dbl>         <dbl>    <dbl>   <dbl>         <dbl>
## 1 ASD         0.0626    13.7          106.   0.0270    2.72          18.6
## 2 TD          0.0576    13.7          105.   0.0270    2.66          14.2
```

This prints out, by diagnosis, our group means-- listed first with a "_fn1" -- followed by the group SD with a "_fn2"

_Note_ This did not print out anything with Gender or Handedness. That is because these variables are not numeric-- they are factors. This requries the *summarise_if()* command. 

Alternative summarise (or summarize! both are accepted) commands include: summarize(), summarize_at, summarize_all.   

In older R code you will sometimes see summarize_each, but this command is deprecated in the recent dplyr updates.

There are other summary type commands-- such as using the **tableone** package


```r
vars <- colnames(matching2)
vars <- vars[c(3:7)]
vars
```

```
## [1] "RMSD"       "Age"        "WASI_NVIQ"  "Gender"     "Handedness"
```

```r
CreateTableOne(vars = vars, data = matching2, strata = "Diagnosis", test = T) #Have true test to obtain p values
```

```
##                        Stratified by Diagnosis
##                         ASD            TD             p      test
##   n                         40             38                    
##   RMSD (mean (SD))        0.06 (0.03)    0.06 (0.03)   0.416     
##   Age (mean (SD))        13.66 (2.72)   13.67 (2.66)   0.982     
##   WASI_NVIQ (mean (SD)) 106.15 (18.55) 105.45 (14.21)  0.852     
##   Gender = Female (%)        8 (20.0)       5 (13.2)   0.612     
##   Handedness = Left (%)      7 (17.5)       5 (13.2)   0.828
```


## Chi-Square Tests


```r
chisq.test(matching2$Diagnosis, matching2$Gender)
```

```
## 
## 	Pearson's Chi-squared test with Yates' continuity correction
## 
## data:  matching2$Diagnosis and matching2$Gender
## X-squared = 0.25658, df = 1, p-value = 0.6125
```

```r
chisq.test(matching2$Diagnosis, matching2$Handedness)
```

```
## 
## 	Pearson's Chi-squared test with Yates' continuity correction
## 
## data:  matching2$Diagnosis and matching2$Handedness
## X-squared = 0.047234, df = 1, p-value = 0.8279
```

```r
tadaa_chisq(matching2, x = Diagnosis, y = Gender, print = "markdown")
```

```
## Warning: Unknown or uninitialised column: 'cramers'.
```

Table 1: **Pearson's Chi-squared test with Yates' continuity correction**


| $\chi^2$ |  p   | df | Odds Ratio | $\phi$ |
|:--------:|:----:|:--:|:----------:|:------:|
|   0.26   | .612 | 1  |    0.61    |  0.09  |


<br>


<br>



## Variance Assumptions: Bartlett's and Levene's

Before confirming that our means match, we need to check some assumptions about homogeneity of variance for each group. To do this, we can run Bartlett's test or Levene's test. For here we just specify _bartlett.test_ wrapped around our variable of interest, in this case RMSD, and our grouping variable Diagnosis. We then specify that the data file we are referring to is matching2

Alternative way of writing this test using _leveneTest()_ similarly with a (y, group) ordering and specify the dataframe directly.

Bartlett's test and Levene's are similar in testing for equal variances across groups for a continuous variable, with Levene's test being less influenced by outliers. This isn't a 


```r
bartlett.test(RMSD ~ Diagnosis, matching2) 
```

```
## 
## 	Bartlett test of homogeneity of variances
## 
## data:  RMSD by Diagnosis
## Bartlett's K-squared = 4.3313e-06, df = 1, p-value = 0.9983
```

```r
leveneTest(matching2$RMSD, matching2$Diagnosis)
```

```
## Levene's Test for Homogeneity of Variance (center = median)
##       Df F value Pr(>F)
## group  1  0.2494  0.619
##       76
```


Plotting normal qq-plots is also fairly easy. Here are examples with three methods-- you'll notice that once we start using tidyverse the output becomes a dataframe/double value. This will need to be unlisted first then changed to a number. This step is avoided in the other two methods because the RMSD variable is a number, but once it is filtered and selected it is created into its own dataframe.


```r
# Base R way

qqnorm(matching2$RMSD[matching2$Diagnosis == "ASD"], main = "Q-QPlot for ASD RMSD"); qqline(matching2$RMSD[matching2$Diagnosis == "ASD"])
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-28-1.png)<!-- -->

```r
#Tidy way

TDRMSD <- matching2 %>%
dplyr::filter(Diagnosis == "TD") %>%
  dplyr::select(RMSD) 

qqnorm(as.numeric(unlist(TDRMSD)), main = "Q-QPlot for TD RMSD"); qqline(as.numeric(unlist(TDRMSD)))
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-28-2.png)<!-- -->

```r
## A GGPLOT way

matching2 %>%
  ggplot(aes(sample = RMSD)) +
  geom_qq() + geom_qq_line() +
  facet_wrap(~Diagnosis, scales = "free_y")
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-28-3.png)<!-- -->

## Skewedness and Kurtosis

We might already have an idea if the data are skewed from the Q-Q Plots, but we can also print these values directly for the whole data set

We can use _describeBy()_ and just like with summarize- the variable will need to be numeric. Below we are asking R to describe columns 3-5, all numeric, and by Diagnosis Group.



```r
describeBy(matching2[3:5], group = matching2$Diagnosis)
```

```
## 
##  Descriptive statistics by group 
## group: ASD
##           vars  n   mean    sd median trimmed   mad   min    max range
## RMSD         1 40   0.06  0.03   0.06    0.06  0.03  0.02   0.11  0.09
## Age          2 40  13.66  2.72  13.80   13.73  3.41  7.40  18.00 10.60
## WASI_NVIQ    3 40 106.15 18.55 104.00  106.75 18.53 53.00 140.00 87.00
##            skew kurtosis   se
## RMSD       0.10    -1.32 0.00
## Age       -0.17    -0.92 0.43
## WASI_NVIQ -0.39     0.20 2.93
## -------------------------------------------------------- 
## group: TD
##           vars  n   mean    sd median trimmed   mad   min    max range
## RMSD         1 38   0.06  0.03   0.06    0.06  0.03  0.02   0.13   0.1
## Age          2 38  13.67  2.66  14.00   13.78  2.82  8.10  17.70   9.6
## WASI_NVIQ    3 38 105.45 14.21 105.50  105.97 12.60 62.00 129.00  67.0
##            skew kurtosis   se
## RMSD       0.57    -0.57 0.00
## Age       -0.41    -0.79 0.43
## WASI_NVIQ -0.52     0.44 2.30
```

We can also use regular _describe_ which can handle factors


```r
describe(matching2[-2])  #Just droping Subject ID column
```

```
## matching2[-2] 
## 
##  6  Variables      78  Observations
## ---------------------------------------------------------------------------
## Diagnosis 
##        n  missing distinct 
##       78        0        2 
##                       
## Value        ASD    TD
## Frequency     40    38
## Proportion 0.513 0.487
## ---------------------------------------------------------------------------
## RMSD 
##        n  missing distinct     Info     Mean      Gmd      .05      .10 
##       78        0       54        1  0.06017  0.03097  0.02385  0.02600 
##      .25      .50      .75      .90      .95 
##  0.03650  0.05850  0.07875  0.09730  0.10175 
## 
## lowest : 0.019 0.021 0.023 0.024 0.026, highest: 0.100 0.101 0.106 0.110 0.126
## ---------------------------------------------------------------------------
## Age 
##        n  missing distinct     Info     Mean      Gmd      .05      .10 
##       78        0       52    0.999    13.66    3.076    9.125    9.910 
##      .25      .50      .75      .90      .95 
##   11.575   13.850   15.875   17.130   17.600 
## 
## lowest :  7.4  8.1  8.7  9.2  9.6, highest: 17.5 17.6 17.7 17.8 18.0
## ---------------------------------------------------------------------------
## WASI_NVIQ 
##        n  missing distinct     Info     Mean      Gmd      .05      .10 
##       78        0       43    0.999    105.8    18.39     83.4     87.7 
##      .25      .50      .75      .90      .95 
##     97.0    104.5    118.0    126.0    129.0 
## 
## lowest :  53  62  70  80  84, highest: 126 127 129 134 140
## ---------------------------------------------------------------------------
## Gender 
##        n  missing distinct 
##       78        0        2 
##                         
## Value        Male Female
## Frequency      65     13
## Proportion  0.833  0.167
## ---------------------------------------------------------------------------
## Handedness 
##        n  missing distinct 
##       78        0        2 
##                       
## Value      Right  Left
## Frequency     66    12
## Proportion 0.846 0.154
## ---------------------------------------------------------------------------
```

## Correlations

Useful links for correlation basics:


http://www.sthda.com/english/wiki/correlation-matrix-a-quick-start-guide-to-analyze-format-and-visualize-a-correlation-matrix-using-r-software


https://cran.r-project.org/web/packages/corrplot/vignettes/corrplot-intro.html


There are several ways to print or _summarize_ correlations of a variable set. First the **tidy** way!


Methods that are accepted include: "pearson", "spearman", or "kendall"


```r
# Need to drop non-numeric variables which are in columns 1,2,6, and 7

cor_matrix <- matching2 %>%
                group_by(Diagnosis) %>%
                  do(as.data.frame(cor(.[,-c(1,2,6,7)], method="spearman", use="pairwise.complete.obs")))

#
# to add row names
#
cor_matrix1 <- cor_matrix %>%  
              data.frame(row=rep(colnames(.)[-1], n_groups(.))) 
#
# calculate correlations and display in column format
#
num_col=ncol(matching2[,-c(1,2,6,7)])
out_indx <-  which(upper.tri(diag(num_col))) 
cor_cols <- matching2 %>% group_by(Diagnosis) %>%
            do(melt(cor(.[,-c(1,2,6,7)], method="spearman", use="pairwise.complete.obs"), value.name="cor")[out_indx,])

ncol(matching2[,-c(1,2,6,7)])
```

```
## [1] 3
```

```r
cor_matrix1
```

```
##   Diagnosis        RMSD         Age   WASI_NVIQ       row
## 1       ASD  1.00000000 -0.36602534 -0.08738912      RMSD
## 2       ASD -0.36602534  1.00000000 -0.06561224       Age
## 3       ASD -0.08738912 -0.06561224  1.00000000 WASI_NVIQ
## 4        TD  1.00000000 -0.25498357 -0.21914882      RMSD
## 5        TD -0.25498357  1.00000000 -0.27920478       Age
## 6        TD -0.21914882 -0.27920478  1.00000000 WASI_NVIQ
```


Not bad, but it would be nice to know if some of these are significant or not. We can use _cor.test_.  Here I'm defining is as an object **examplecor** so you can print the object by calling its name:


```r
examplecor <- cor.test(matching2$RMSD,matching2$Age)
```

And as for its structure to understand what R has created-- unsurprisingly, it's a list!. But this is good to know as it will let us index individual aspects of the list if we would like more direct comparisons using it in conjunction with dplyr


```r
str(examplecor)
```

```
## List of 9
##  $ statistic  : Named num -3.01
##   ..- attr(*, "names")= chr "t"
##  $ parameter  : Named int 76
##   ..- attr(*, "names")= chr "df"
##  $ p.value    : num 0.00356
##  $ estimate   : Named num -0.326
##   ..- attr(*, "names")= chr "cor"
##  $ null.value : Named num 0
##   ..- attr(*, "names")= chr "correlation"
##  $ alternative: chr "two.sided"
##  $ method     : chr "Pearson's product-moment correlation"
##  $ data.name  : chr "matching2$RMSD and matching2$Age"
##  $ conf.int   : num [1:2] -0.512 -0.112
##   ..- attr(*, "conf.level")= num 0.95
##  - attr(*, "class")= chr "htest"
```

```r
examplecor$conf.int
```

```
## [1] -0.5116297 -0.1118309
## attr(,"conf.level")
## [1] 0.95
```

```r
matching2 %>%
  group_by(Diagnosis) %>%
  dplyr::summarize(cor = cor.test(RMSD,Age)$estimate, p.value = cor.test(RMSD, Age)$p.value, LowerCInt  = cor.test(RMSD, Age)$conf.int[[1]], UpperCInt  = cor.test(RMSD, Age)$conf.int[[2]])
```

```
## # A tibble: 2 x 5
##   Diagnosis    cor p.value LowerCInt UpperCInt
##   <fct>      <dbl>   <dbl>     <dbl>     <dbl>
## 1 ASD       -0.368  0.0196    -0.609   -0.0635
## 2 TD        -0.284  0.0840    -0.553    0.0392
```

```r
#Ungrouped by commenting out the grouping part of the argument

matching2 %>%
  #group_by(Diagnosis) %>%
  dplyr::summarize(cor = cor.test(RMSD,Age)$estimate, p.value = cor.test(RMSD, Age)$p.value, LowerCInt  = cor.test(RMSD, Age)$conf.int[[1]], UpperCInt  = cor.test(RMSD, Age)$conf.int[[2]])
```

```
##          cor     p.value  LowerCInt  UpperCInt
## 1 -0.3262428 0.003557044 -0.5116297 -0.1118309
```

OK, but what if we wanted a correlation matrix _and_ p-values?


For this we can use the Hmisc package using the _rcorr()_ command. 


This requires that a matrix be fed in and outputs a list, the first item, 'r',  being the correlation value and the second item, 'p', being the corresponding p-value



```r
ASDmat1 <- matching2 %>%
  filter(Diagnosis == 'ASD') %>%
  dplyr::select_if(is.numeric)
  

TDmat1 <- matching2 %>%
  filter(Diagnosis == 'TD') %>%
  dplyr::select_if(is.numeric)

#Now both are now numeric values in dataframes-- still need to be made into matrices

  
TDcors <- rcorr(as.matrix(TDmat1))  #Creates a list containing objects about the matrix's r-value, n of sample, and p-value or correlation


#As index by its structure

str(TDcors)
```

```
## List of 3
##  $ r: num [1:3, 1:3] 1 -0.284 -0.339 -0.284 1 ...
##   ..- attr(*, "dimnames")=List of 2
##   .. ..$ : chr [1:3] "RMSD" "Age" "WASI_NVIQ"
##   .. ..$ : chr [1:3] "RMSD" "Age" "WASI_NVIQ"
##  $ n: int [1:3, 1:3] 38 38 38 38 38 38 38 38 38
##   ..- attr(*, "dimnames")=List of 2
##   .. ..$ : chr [1:3] "RMSD" "Age" "WASI_NVIQ"
##   .. ..$ : chr [1:3] "RMSD" "Age" "WASI_NVIQ"
##  $ P: num [1:3, 1:3] NA 0.084 0.0371 0.084 NA ...
##   ..- attr(*, "dimnames")=List of 2
##   .. ..$ : chr [1:3] "RMSD" "Age" "WASI_NVIQ"
##   .. ..$ : chr [1:3] "RMSD" "Age" "WASI_NVIQ"
##  - attr(*, "class")= chr "rcorr"
```


We can then bind the vectors from the list of separated groups, ASD and TD, and compare.



```r
ASDcors <- rcorr(as.matrix(ASDmat1))


cbind(ASDcors$r,ASDcors$P,TDcors$r, TDcors$P)
```

```
##                  RMSD        Age   WASI_NVIQ       RMSD        Age
## RMSD       1.00000000 -0.3677081 -0.08950919         NA 0.01958063
## Age       -0.36770811  1.0000000 -0.12476030 0.01958063         NA
## WASI_NVIQ -0.08950919 -0.1247603  1.00000000 0.58282764 0.44305970
##           WASI_NVIQ       RMSD        Age  WASI_NVIQ       RMSD        Age
## RMSD      0.5828276  1.0000000 -0.2840247 -0.3393771         NA 0.08396134
## Age       0.4430597 -0.2840247  1.0000000 -0.2847243 0.08396134         NA
## WASI_NVIQ        NA -0.3393771 -0.2847243  1.0000000 0.03711561 0.08316823
##            WASI_NVIQ
## RMSD      0.03711561
## Age       0.08316823
## WASI_NVIQ         NA
```
Pretty messy, but there are other methods to that have more to do with visualization. _rcorr_ works well, when you are looking at full sample descriptives or not binding columns from different groups. 


One package, Performance Analytics, quickly gives visualizations and r/p values; however, specification is limited as of now in separting groups on the same graph. * I currently don't have a more recent version of R to run this package-- I'm updating, don't judge. As such, I'm skipping this part for now.



```r
#install.packages("PerformanceAnayltics")
#library()
```

#### Point Biserial Correlations




# 5/15/19: Plots! lots-ah plots-ah! {.tabset .tabset-fade .tabset-pill}

## Quick Base R plots

### Histograms

Before making fancy plots with ggplot2 package, we can use Base R's plot commands to quickly visualize the distributions of the data with histograms - either whole group:


```r
hist(matching2$WASI_NVIQ)
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-39-1.png)<!-- -->

Or by levels of a categorical variable of interest:


```r
# A tidy way

TDhist <- matching2 %>%  
dplyr::filter(Diagnosis == "TD") %>%
  dplyr::select(WASI_NVIQ) %>%
            lapply(hist, main = "Histogram Example", xlab = "TD", col.main = "red", cex.main = 2, col.lab = "darkblue")
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-40-1.png)<!-- -->

```r
### Other way with more Base R

hist(matching2$WASI_NVIQ[matching2$Diagnosis == "ASD"], xlab = "ASD", main = "Other Histogram Example", col = "purple")
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-40-2.png)<!-- -->




There are also boxplots, lines, and bar graphs. But personally I think ggplot looks nicer.

### Boxplots


```r
boxplot(matching2$Age, data =  matching2)
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-41-1.png)<!-- -->

```r
boxplot(matching2$Age~matching2$Diagnosis, data =  matching2, ylab = "Age in Years")
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-41-2.png)<!-- -->


Want it horizontal? Or with notches?


```r
boxplot(matching2$Age~matching2$Diagnosis, data =  matching2, xlab = "Age in Years", horizontal = TRUE, notch = TRUE, frame = FALSE)  #Note since this is being horizontal now we have to change our former "y lab" to an "x lab"
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-42-1.png)<!-- -->


Can't forget the colors


```r
boxplot(Age~Diagnosis, data =  matching2, ylab = "Age in Years", col = "grey", border = c("blue", "red"), frame = F)
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-43-1.png)<!-- -->





### Bar Plots

Bar plots can depict counts, frequencies, and also aggregated statistics, like means, medians, etc.

For counts, we can make a table object with _table()_ command


```r
barplottab <- table(matching2$Diagnosis, matching2$Gender)

barplottab # Which looks like this
```

```
##      
##       Male Female
##   ASD   32      8
##   TD    33      5
```

```r
#Then graph it

barplot(barplottab, legend = rownames(barplottab), beside = T, col = c("blue", "green"))
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-44-1.png)<!-- -->

```r
#Note what happens when you leave the "beside = T" code out

barplot(barplottab, legend = rownames(barplottab), col = c("blue", "green")) 
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-44-2.png)<!-- -->

We can also use the _aggregate()_ command and _aggregate.plot()_. As we used above, aggregate performs summary stats in base R. Read more about aggregate function with  _?aggregate_ in your console.


```r
# This is a more recent package but is still pretty cool!

#install.packages("epiDisplay")
suppressMessages(library(epiDisplay))


aggregate.plot(matching2$WASI_NVIQ,
                              by=list(matching2$Diagnosis), error = "ci" ,
                              FUN="median")
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-45-1.png)<!-- -->

```r
# Get fancier

aggregate.plot(matching2$WASI_NVIQ,
                              by=list(matching2$Diagnosis, matching2$Gender), error = "ci" , bar.col = c("purple", "goldenrod"),
                              FUN="mean", legend.site = "bottom")
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-45-2.png)<!-- -->

### Scatter Plots


```r
plot(matching2$RMSD, matching2$WASI_NVIQ, main = "RMSD by IQ",
     xlab = "RMSD", ylab = "NonVerbal IQ on WASI",
     pch = 19, frame = TRUE)  #pch is the shape of the scatter point
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-46-1.png)<!-- -->

```r
# Add regression line
plot(matching2$RMSD, matching2$WASI_NVIQ, main = "RMSD by IQ",
     xlab = "RMSD", ylab = "NonVerbal IQ on WASI",
     pch = 10, frame = FALSE)
abline(lm(WASI_NVIQ ~ RMSD, data = matching2), col = "blue")
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-46-2.png)<!-- -->

Can also use _scatterplot_ from the **cars** package


```r
scatterplot(WASI_NVIQ ~ RMSD|Diagnosis, data = matching2)
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-47-1.png)<!-- -->

Gross- too busy, but handy to visualize quickly adding the below makes it look somewhat better


```r
scatterplot(WASI_NVIQ ~ RMSD|Diagnosis, data = matching2, smooth = FALSE, grid = FALSE, frame = FALSE)
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-48-1.png)<!-- -->

OK but if we want different colors? For base plotting, you'll need to create a vector of color names that needs to be the same length as your grouping factor(s). Since we just have 2, pick the two best colors. You can use names of colors, their hexadecimal numbers, or integer number placement on color palette 

Check out more colors here: https://rstudio-pubs-static.s3.amazonaws.com/3486_79191ad32cf74955b4502b8530aad627.html


```r
#Color vector

mycolors <- c("#0000CD","#B03060")

scatterplot(WASI_NVIQ ~ RMSD|Diagnosis, data = matching2, smooth = FALSE, grid = FALSE, frame = FALSE, col =mycolors)
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-49-1.png)<!-- -->


## GGPlot2 

GGplot2 is graphing package in R that can give a more polished touch, but Base R graphing can literally create anything. GGplot2 can accept tidyverse outputs which can be extremely efficient.

Things to note are the order of the argument, unless using the _%>%_ operator, the command is telling R to use the ggplot package then gives the name of the dataframe. This is followed by the _aes()_ argument defining the _aesthetic_ features of the graph. Here the x and y variables are defined as well as any group specification that would apply globally to the graph. You may define group variables in subsequent lines with an additional _aes()_ command. 

Then the "+" symbol indicates that the following should be added to the plot. Here is where you define the type of "shape" R should draw using *geom_shapename*

You can define a grouping variable many ways, in the sample below we have Diagnosis group defining the "coloring" of the plot.

The use of _color_ here references a command that R should use when coloring things where a color can't be "filled in" as much as it can just have a single colored line/border. So for graphs like scatter plots and line graphs, the color command would be used in the aesthetic specification-- and similarly color is referenced again in the subsequent lines of code where we manually color with our color values of choice.


#### Geom Point/Geom Line

```r
plot <- ggplot(matching2, aes(x = Age, y= WASI_NVIQ, color = Diagnosis)) +  geom_point(aes(shape = Diagnosis)) + geom_smooth(method = lm) 

plot
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-50-1.png)<!-- -->

```r
plot <- plot + labs(main = "Title", x = "Age", y = "IQ") + theme_classic()  # Add Titles and Labels as well as Theme

plot <- plot + scale_color_manual(values = c("firebrick3", "dodgerblue3"))  #Modify colors manually

plot
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-50-2.png)<!-- -->

Check out R Color Brewer for palettes and Color-Blind friendly color schemes:

http://colorbrewer2.org/#type=sequential&scheme=BuGn&n=3

Fun with Shapes: http://www.sthda.com/english/wiki/ggplot2-point-shapes


#### Geom Violin and Boxplot

If we would like to draw a geom_shape that you can "fill" with color, like a boxplot, histogram, violin plot, etc. then you use _fill_ instead of _color_ in the aesthetics command-- and similarly later when the colors are manually defined.


```r
ggplot(matching2, aes(x = Diagnosis, y = WASI_NVIQ, fill = Diagnosis)) +
  geom_violin() +
   scale_fill_manual(values = c("firebrick3", "dodgerblue3"))
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-51-1.png)<!-- -->

You may also overlap geoms on top of other geoms. If they are similar objects (i.e., are both filler objects), then an aesthetic command does not need to be redefined, and just inherits the global grouping definition


```r
ggplot(matching2, aes(x = Diagnosis, y = WASI_NVIQ, fill = Diagnosis)) +
  geom_violin() +
   scale_fill_manual(values = c("firebrick3", "dodgerblue3")) +
 geom_boxplot(width =.1) 
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-52-1.png)<!-- -->

And keep adding!


```r
#Violin plots
ggplot(matching2, aes(x=Diagnosis, y= WASI_NVIQ, fill = Diagnosis)) +
  geom_violin() +
  scale_color_manual(values = c("firebrick3","dodgerblue3")) +
  geom_rug(aes(color = Diagnosis), show.legend = F, alpha = .9, position = "jitter", sides = "l") + 
  guides(fill = guide_legend(title = "Group")) +
  labs(title="Your title here :)", x = "Group", y = "WASI-II NVIQ") +
  geom_boxplot(width = 0.1, fill = "white") +
  scale_fill_manual(values = c("firebrick3","dodgerblue3")) + 
   theme_classic()
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-53-1.png)<!-- -->

Similar to the above chart with some subtle changes to tweak the chart to get it to show what we would like. For charts that involve multiple colors, it may be a good idea to define a vector of color names, and then coloring by that vector as shown in the code below.


```r
prettycols <- c("firebrick3", "dodgerblue3")

#Violin plots v2
ggplot(matching2, aes(x=Diagnosis, y= WASI_NVIQ, group = Diagnosis)) +
  geom_violin(fill = "grey") +
   geom_boxplot(aes(group = Diagnosis), fill= prettycols, width = 0.2) +
  geom_rug(aes(color = Diagnosis), alpha = .9, position = "jitter", sides = "l") + 
  scale_color_manual(values = prettycols, labels = c("ASD","TD"))+
  labs(title="Your Centered title here", x = "Group", y = "WASI-II NVIQ") +
  theme_classic() +
  theme(plot.title = element_text(hjust = 0.5)) 
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-54-1.png)<!-- -->

# 5/22/19  Fancier Plots for the Fancier Pants

The plots above are nice, but visualizations are more than just traditional, static approaches and have potential to show multiple relations/trends by using varying graphical approaches visualized simultaneously. 

This is more than layering points on a line or boxplot. Much of the packages reviewed here are based with ggplot commands and work well with tidyverse. The rest of the tutorial will only include these commands. It's likely though that with Base R, you'll still have most precision over a graphic. 

### Corrplot


```r
#install.packages("corrplot")

#Library code is called above at top of document


str(TDcors)
```

```
## List of 3
##  $ r: num [1:3, 1:3] 1 -0.284 -0.339 -0.284 1 ...
##   ..- attr(*, "dimnames")=List of 2
##   .. ..$ : chr [1:3] "RMSD" "Age" "WASI_NVIQ"
##   .. ..$ : chr [1:3] "RMSD" "Age" "WASI_NVIQ"
##  $ n: int [1:3, 1:3] 38 38 38 38 38 38 38 38 38
##   ..- attr(*, "dimnames")=List of 2
##   .. ..$ : chr [1:3] "RMSD" "Age" "WASI_NVIQ"
##   .. ..$ : chr [1:3] "RMSD" "Age" "WASI_NVIQ"
##  $ P: num [1:3, 1:3] NA 0.084 0.0371 0.084 NA ...
##   ..- attr(*, "dimnames")=List of 2
##   .. ..$ : chr [1:3] "RMSD" "Age" "WASI_NVIQ"
##   .. ..$ : chr [1:3] "RMSD" "Age" "WASI_NVIQ"
##  - attr(*, "class")= chr "rcorr"
```

```r
ASDcorrplot <- matching2 %>%
      filter(Diagnosis == 'ASD') %>%
      select_if(is.numeric) %>%
      cor()


corrplot(ASDcorrplot, type = "upper", order = "hclust", 
         tl.col = "black", tl.srt = 45)
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-55-1.png)<!-- -->

```r
TDcorrplot <- matching2 %>%
      filter(Diagnosis == 'TD') %>%
      select_if(is.numeric) %>%
      cor()

corrplot(TDcorrplot, type = "lower", order = "hclust", 
         tl.col = "black", tl.srt = 45)
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-55-2.png)<!-- -->

It would be great if we could compare, yes? Thinking about these matrices, we could just replace the values of the lower ASD matrix with the values from the TD matrix, creating a merged matrix-- then plot that, noting which upper or lower parts denote which group

First rename the ASDcorrplot into something new-



```r
mergedcorrplot <- ASDcorrplot

mergedcorrplot[lower.tri(mergedcorrplot)] <- TDcorrplot[lower.tri(TDcorrplot)]


# Will give you

mergedcorrplot
```

```
##                 RMSD        Age   WASI_NVIQ
## RMSD       1.0000000 -0.3677081 -0.08950919
## Age       -0.2840247  1.0000000 -0.12476030
## WASI_NVIQ -0.3393771 -0.2847243  1.00000000
```


Then you can graph this, using the full matrix display option


```r
corrplot(mergedcorrplot, type = "full")  #ASD on top and TD on bottom
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-57-1.png)<!-- -->

```r
#Add additional tweaks for easier readability, depending on what you want-- many options!

corrplot(mergedcorrplot, type = "full", method = 'number', title = "ASD (top) and TD (bottom)", tl.col = 'black', tl.srt = 45, col = c("blue", "black", "red"))
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-57-2.png)<!-- -->

Let's say we only want to display correlations that are significant at the p <.05 level. To do this we will need to modify the original matrices of ASDcors and TDcors-- used from the correlation discussion above.

We will create new matrices of the P-values, first duplicating the ASD p-values taken from the ASDcor list. This becomes as the future merged matrix (this means the ASD p-values are in the upper triangle of the matrix). 

Then TDcors$P is turn into its own matrix, before selecting out the lower triangle from it, and re-writing over the lower triangle of the merged p-value matrix.

Now the p-values along the top triangle belong to the ASD group and the ones along the bottom triangle belong to the TD group. With this matrix now merged, it can be the "key" to mapping out significant values when the _corrplot()_ command is called. 

**Importantly, the merged matrix correlation values must match a separate, similarly grouped merged to work**


```r
# From before

ASDcors$P
```

```
##                 RMSD        Age WASI_NVIQ
## RMSD              NA 0.01958063 0.5828276
## Age       0.01958063         NA 0.4430597
## WASI_NVIQ 0.58282764 0.44305970        NA
```

```r
mergedpvals <- ASDcors$P




#From before


TDcors$P
```

```
##                 RMSD        Age  WASI_NVIQ
## RMSD              NA 0.08396134 0.03711561
## Age       0.08396134         NA 0.08316823
## WASI_NVIQ 0.03711561 0.08316823         NA
```

```r
TDpvals <- TDcors$P


mergedpvals[lower.tri(mergedpvals)] <- TDpvals[lower.tri(TDpvals)]

mergedpvals
```

```
##                 RMSD        Age WASI_NVIQ
## RMSD              NA 0.01958063 0.5828276
## Age       0.08396134         NA 0.4430597
## WASI_NVIQ 0.03711561 0.08316823        NA
```


```r
#the new pval matrix is set equal to the p.mat

corrplot(mergedcorrplot, type = "full", method = 'number', title = "ASD (top) and TD (bottom)", tl.col = 'black', tl.srt = 45, col = c("blue", "black", "red"), p.mat = mergedpvals, sig.level = 0.05, insig = "blank")
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-59-1.png)<!-- -->


Alternatively we can examine all of matching2 by treating it all as a matrix


```r
matchingnum <- matching2[-2] #Dropping participant ID

matchingnum[c(1,5,6)] <- lapply(matchingnum[c(1,5,6)], as.integer) #Make factors their integer values

matchingnum
```

```
##    Diagnosis  RMSD  Age WASI_NVIQ Gender Handedness
## 1          1 0.032 14.1       140      1          1
## 2          1 0.110 13.2       103      1          2
## 3          1 0.030 17.3       106      2          1
## 4          1 0.021 17.1       126      1          1
## 5          1 0.101 15.1       121      1          1
## 6          1 0.024 13.8       112      1          2
## 7          1 0.019 16.5       104      1          1
## 8          1 0.106 13.8       127      1          1
## 9          1 0.077 15.3       129      1          1
## 10         1 0.051 12.9       100      1          1
## 11         1 0.032 13.9       125      1          1
## 12         1 0.098 10.9       115      2          1
## 13         1 0.071 13.4        84      1          1
## 14         1 0.078 10.4        97      1          1
## 15         1 0.041 10.8        96      1          1
## 16         1 0.034 17.0       103      1          2
## 17         1 0.026 17.8       102      1          1
## 18         1 0.048 14.1        88      1          1
## 19         1 0.096 18.0        80      2          1
## 20         1 0.075 13.5       109      1          2
## 21         1 0.036 15.0       123      2          2
## 22         1 0.040 17.2        97      1          1
## 23         1 0.061 15.5        53      2          1
## 24         1 0.073  9.6       118      1          1
## 25         1 0.093 16.8       114      2          1
## 26         1 0.095  7.4       102      1          1
## 27         1 0.078 11.0        86      1          2
## 28         1 0.049 16.6        89      1          1
## 29         1 0.101 12.1       120      1          1
## 30         1 0.042 10.5       140      2          1
## 31         1 0.090  9.2        99      1          1
## 32         1 0.048 17.5       112      1          1
## 33         1 0.079 13.4        90      2          1
## 34         1 0.063 14.4       100      1          1
## 35         1 0.045 11.5       104      1          1
## 36         1 0.072 14.4        70      1          2
## 37         1 0.047 11.5       134      1          1
## 38         1 0.097 11.8       125      1          1
## 39         1 0.061 12.0        93      1          1
## 40         1 0.064 10.0       110      1          1
## 51         2 0.050 16.6       120      1          2
## 52         2 0.031 16.6       126      1          1
## 53         2 0.055 12.6       129      1          1
## 54         2 0.100 12.4        88      2          1
## 55         2 0.031 12.9       103      1          1
## 56         2 0.038 14.8       101      1          1
## 57         2 0.024 15.5        92      1          1
## 58         2 0.066 13.9       106      1          1
## 59         2 0.054 12.9       106      1          1
## 60         2 0.066 13.8        90      2          1
## 61         2 0.030 16.0       114      1          2
## 62         2 0.067 14.9       110      1          1
## 63         2 0.031 14.1       112      2          1
## 64         2 0.110  8.7       103      2          1
## 65         2 0.068 11.0       118      1          1
## 66         2 0.096  9.7       107      1          2
## 67         2 0.021 15.3       111      1          1
## 68         2 0.026  8.7       127      1          1
## 69         2 0.055 11.3        99      1          1
## 70         2 0.038 16.0       111      1          1
## 71         2 0.035 10.9       122      2          1
## 72         2 0.077  8.1       123      1          1
## 73         2 0.090 16.2        92      1          1
## 74         2 0.061 17.6       104      1          1
## 75         2 0.126 15.0        62      1          1
## 76         2 0.059 14.4        98      1          2
## 77         2 0.040 17.7        87      1          2
## 78         2 0.058 17.6       105      1          1
## 79         2 0.023 11.2       102      1          1
## 80         2 0.041 13.0        85      1          1
## 81         2 0.092 13.0        95      1          1
## 82         2 0.026 17.1       101      1          1
## 83         2 0.056 16.1        97      1          1
## 84         2 0.059 14.8        90      1          1
## 85         2 0.035 13.6       125      1          1
## 86         2 0.082  9.2       112      1          1
## 87         2 0.088 14.1       112      1          1
## 88         2 0.084 12.2       122      1          1
```

```r
Binarycor <- cor(as.matrix(matchingnum))

corrplot(Binarycor)
```

![](C:\Users\Jonni\Desktop\R Class\Intro-to-R-Basics\docs\index_files/figure-html/unnamed-chunk-60-1.png)<!-- -->

```r
Binarycorpval <-  rcorr(as.matrix(matchingnum))

Binarycorpval$P # Only Age and RMSD are significantly correlated
```

```
##            Diagnosis        RMSD         Age  WASI_NVIQ    Gender
## Diagnosis         NA 0.416017895 0.982326430 0.85211499 0.4242550
## RMSD       0.4160179          NA 0.003557044 0.09556762 0.2871002
## Age        0.9823264 0.003557044          NA 0.09602150 0.9700871
## WASI_NVIQ  0.8521150 0.095567624 0.096021504         NA 0.4700171
## Gender     0.4242550 0.287100215 0.970087073 0.47001707        NA
## Handedness 0.6008653 0.835268456 0.331517586 0.47643584 0.4063273
##            Handedness
## Diagnosis   0.6008653
## RMSD        0.8352685
## Age         0.3315176
## WASI_NVIQ   0.4764358
## Gender      0.4063273
## Handedness         NA
```

### GGpairs
