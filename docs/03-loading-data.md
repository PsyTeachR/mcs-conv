# Loading data

Part of becoming a psychologist is asking questions and gathering data to enable you to answer these questions effectively. It is very important that you understand all aspects of the research process such as experimental design, ethics, data management and visualisation. 

In this chapter, you will continue to develop reproducible scripts. This means scripts that completely and transparently perform an analysis from start to finish in a way that yields the same result for different people using the same software on different computers. And transparency is a key value of science, as embodied in the “trust but verify” motto. When you do things reproducibly, others can understand and check your work. 

This benefits science, but there is a selfish reason, too: the most important person who will benefit from a reproducible script is your future self. When you return to an analysis after two weeks of vacation, you will thank your earlier self for doing things in a transparent, reproducible way, as you can easily pick up right where you left off. The topic of open science is a big debate in the scientific community at the moment. Some classic psychological experiments have been found not to be replicable and part of the explanation for this has been a historical lack of transparency about data and analysis methods and this is a topic we're going to cover more in the lectures and in your reading.

As part of your skill development, it is important that you work with data so that you can become confident and competent in your management and analysis of data. In this course, we will work with real data that has been shared by other researchers. 

## Getting data ready to work with

In this chapter you will learn how to load the packages required to work with the data. You'll then load the data into R Studio before getting it organised into a sensible format that relates to our research question. If you can't remember what packages are, go back and revise Programming Basics.

## Activity 1: Set-up Loading Data {#ld-a1}

Before we begin working with the data we need to do some set-up. If you need help with any of these steps, you should refer to Intro to R and Programming Basics: 

* Download <a href="ahi-cesd.csv" download>`ahi-cesd.csv`</a> and <a href="participant-info.csv" download>`participant-info.csv`</a> into your chapter folder. If you are working on the server, you will need to upload the files to the server as well.  
* Open R and ensure the environment is clear.
* If you're on the server, avoid a number of issues by restarting the session - click `Session` - `Restart R` 
* Set the working directory to your chapter folder.  
* Open a new R Markdown document and save it in your working directory. Call the file "Loading Data".    
* Delete the default R Markdown welcome text and insert a new code chunk.  
* You can use the white space to take any notes that might help you for each activity.

## Activity  2: Load in the package {#ld-a2}

Today we need to use the `tidyverse` package. You will use this package in almost every single chapter of this course as the functions it contains are those we use for data wrangling, descriptive statistics, and visualisation.

* To load the `tidyverse` type the following code into your code chunk and then run it. 


```r
library(tidyverse)
```

## Open data

For this chapter we are going to be using real data from the following paper:

[Woodworth, R.J., O'Brien-Malone, A., Diamond, M.R. and Schüz, B., 2018. Data from, ‘Web-based Positive Psychology Interventions: A Reexamination of Effectiveness’. Journal of Open Psychology Data, 6(1).](https://openpsychologydata.metajnl.com/articles/10.5334/jopd.35/)

We recommend that you read through this paper and open up the .csv files in order to understand the data better but briefly, the files contains data from two scales, the  Authentic Happiness Inventory (AHI) and the Center for Epidemiological Studies Depression (CES-D) scale, as well as demographic information about participants. 

## Activity 3: Read in data {#ld-a3}

Now we can read in the data. To do this we will use the function `read_csv()` that allows us to read in .csv files. There are also functions that allow you to read in .xlsx files and other formats, however in this course we will only use .csv files.

* First, we will create an object called `dat` that contains the data in the `ahi-cesd.csv` file. Then, we will create an object called `info` that contains the data in the `participant-info.csv`.


```r
dat <- read_csv ("ahi-cesd.csv")
pinfo <- read_csv("participant-info.csv")
```


<div class="danger">
<p>There is also a function called <code>read.csv()</code>. Be very careful NOT to use this function instead of <code>read_csv()</code> as they have different ways of naming columns. For the portfolio tasks, unless your results match our exactly you will not get the marks which means you need to be careful to use the right functions.</p>
</div>

## Activity 4: Check yo' data {#ld-a4}

You should now see that the objects `dat` and `pinfo` have appeared in the environment pane. Whenever you read data into R you should always do an initial check to see that your data looks like you expected. There are several ways you can do this, try them all out to see how the results differ.

* In the environment pane, click on `dat` and `pinfo`. This will open the data to give you a spreadsheet-like view (although you can't edit it like in Excel)  
* In the environment pane, click the small blue play button to the left of `dat` and `pinfo`. This will show you the structure of the object information including the names of all the variables in that object and what type they are (also see `str(pinfo)`) 
* Use `summary(pinfo)`
* Use `head(pinfo)`
* Just type the name of the object you want to view, e.g., `dat`.

## Activity 5: Join the files together {#ld-a5}

We have two files, `dat` and `info` but what we really want is a single file that has both the data and the demographic information about the participants. R makes this very easy by using the function `inner_join()`.

Remember to use the help function `?inner_join` if you want more information about how to use a function and to use tab auto-complete to help you write your code.

The below code will create a new object `all_dat` that has the data from both `dat` and `pinfo` and it will use the columns `id` and `intervention` to match the participants' data. 

* Type and run this code and then view the new dataset using one of the methods from Activity 4.


```r
all_dat <- inner_join(x = dat, # the first table you want to join
                      y = pinfo, # the second table you want to join
                      by = c("id", "intervention")) # columns the two tables have in common
```

## Activity 6: Pull out variables of interest {#ld-a6}

Our final step is to pull our variables of interest. Very frequently, datasets will have more variables and data than you actually want to use and it can make life easier to create a new object with just the data you need.

In this case, the file contains the responses to each individual question on both the AHI scale and the CESD scale as well as the total score (i.e., the sum of all the individual responses). For our analysis, all we care about is the total scores, as well as the demographic information about participants.

To do this we use the `select()` function to create a new object named `summarydata`.


```r
summarydata <- select(.data = all_dat, # name of the object to take data from
                      ahiTotal, cesdTotal, sex, age, educ, income, occasion,elapsed.days) # all the columns you want to keep
```

* Type and run the above code and then run `head(summarydata)`. If everything has gone to plan it should look something like this:


| ahiTotal | cesdTotal | sex | age | educ | income | occasion | elapsed.days |
|:--------:|:---------:|:---:|:---:|:----:|:------:|:--------:|:------------:|
|    32    |    50     |  1  | 46  |  4   |   3    |    5     |    182.03    |
|    34    |    49     |  1  | 37  |  3   |   2    |    2     |    14.19     |
|    34    |    47     |  1  | 37  |  3   |   2    |    3     |    33.03     |
|    35    |    41     |  1  | 19  |  2   |   1    |    0     |     0.00     |
|    36    |    36     |  1  | 40  |  5   |   2    |    5     |    202.10    |
|    37    |    35     |  1  | 49  |  4   |   1    |    0     |     0.00     |

Finally, try knitting the file to HTML. And that's it, well done! Remember to save your Markdown in your chapter folder and make a note of any mistakes you made and how you fixed them. You have started on your journey to become a confident and competent member of the open scientific community! 

### Finished! {#ld-fin}

There is no portfolio assessment this week, instead, use the time to get comfortable with what we've covered already and revise the activities and support materials presented so far if needed. If you're feeling comfortable with R, you can work your way through this book at your own pace or push yourself by using the additional resources highlighted in Programming Basics.

If you're using the R server, we strongly recommend that you download a copy of any files you have been working on so that you have a local back-up.

## Debugging tips {#ld-debug}

* When you downloaded the files did you save the file names **exactly** as they were originally? If you download the file more than once you will find your computer may automatically add a number to the end of the file name. `data.csv` is not the same as `data(1).csv`. Pay close attention to names!
* Have you used the **exact** same object names as we did in each activity? Remember, `name` is different to `Name`. In order to make sure you can follow along with this book, pay special attention to ensuring you use the same object names as we do.  
* Have you used quotation marks where needed?  
* Have you accidentally deleted any back ticks (```) from the beginning or end of code chunks?


## Debugging exercises {#ld-debugex}

These exercises will produce errors. Try to solve the errors yourself, and then make a note of what the error message was and how you solved it - you might find it helpful to create a new file just for error solving notes. You will find that you make the same errors in R over and over again so whilst this might slow you down initially, it will greatly speed you up in the long-run. 

1. Restart the R session (`Session - Restart R`). This should unload any packages you had loaded and also clear the environment. Make sure that the working directory is set to the right folder and then run the below code:


```r
dat <- read_csv("ahi-cesd.csv")
```

This will produce the error `could not find function "read_csv"`. Once you figure out how to fix this error, make a note of it.


<div class='webex-solution'><button>Solution</button>

When you restarted the session, you unloaded all the packages you previously had loaded. The function `read_csv()` is part of the `tidyverse` package which means that in order for the code to run, you need to run `library(tidyverse)` to reload the package so that you can use the function.

</div>
 

2. Make sure that the working directory is set to the right folder and then run the below code:


```r
library(tidyverse)
dat <- read_csv("ahi-cesd")
```

This will produce the error `Error: 'ahi-cesd' does not exist in current working directory`. Once you figure out how to fix this error, make a note of it.


<div class='webex-solution'><button>Solution</button>

When loading data, you need to provide the full file name, including the file extension. In this case, the error was caused by writing `ahi-cesd` instead of `ahi-cesd.csv`. As far as R is concerned, these are two completely different files and only one of them exists in the working directory.

</div>
 

3. Make sure that the working directory is set to the right folder and then run the below code:


```r
library(tidyverse)
dat <- read_csv ("ahi-cesd.csv")
pinfo <- read_csv("participant-info.csv")
all_dat <- inner_join(x = dat, 
                      y = pinfo, 
                      by = "id", "intervention") 
summary(all_dat)
```


Look at the summary for `all_dat`. You will see that R has duplicated the `intervention` variable, so that there is now an `intervention.x` and an `intervention.y` that contain the same data. Once you figure out how to fix this error, make a note of it.



<div class='webex-solution'><button>Solution</button>

If you want to join two tables that have mulitple columns in common, you need to use the `c()` command to list all of the variables. The code above hasn't done this, it's just listed `id` and `intervention` without enclosing them with `c()`.

</div>
 


## Test yourself {#ld-test}

1. When loading in a .csv file, which function should you use? 

<select class='webex-select'><option value='blank'></option><option value='answer'>read_csv()</option><option value=''>read.csv()</option></select>


<div class='webex-solution'><button>Explain this answer</button>

Remember, in this course we use `read_csv()` and it is important for the portfolio assignment that you use this function otherwise you may find that the variable names are slightly different and you won't get the marks

</div>
 

<br>

2. The function `inner_join()` takes the arguments `x`, `y`, `by`. What does `by` do?

<select class='webex-select'><option value='blank'></option><option value=''>Specifies the first table to join</option><option value=''>Specifies the second table to join</option><option value='answer'>Specifies the column to join by that both tables have in common</option></select>

3. What does the function `select()` do? 
<br>
<select class='webex-select'><option value='blank'></option><option value=''>Keeps only the observations you specify</option><option value='answer'>Keeps only the variables you specify</option><option value=''>Keeps only the objects you specify</option></select>
