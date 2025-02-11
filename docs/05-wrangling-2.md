# Data wrangling 2

One of the key skills in an researcher's toolbox is the ability to work with data. When you run an experiment you get lots of data in various files. For instance, it is not uncommon for an experimental software to create a new file for every participant you run and for each participant's file to contain numerous columns and rows of data, only some of which are important. Being able to wrangle that data, manipulate it into different layouts, extract the parts you need, and summarise it, is one of the most important skills we will help you learn.

Over this course you will develop your skills in working with data. This chapter focuses on organizing data using the `tidyverse` package that you have read about in earlier chapters. Over the course, you will learn the main functions for data wrangling and how to use them, and we will use a number of different datasets to give you a wide range of exposure to what Psychology is about, and to reiterate that the same skills apply across different datasets. **The skills don't change, just the data!**

There are some questions to answer as you go along to test your skills: use the example code as a guide and the solutions are at the bottom. Remember to be pro-active in your learning, work together as a community, and if you get stuck use the **[cheatsheets](https://www.rstudio.com/resources/cheatsheets/)**. The key cheatsheet for this activity is the Data Transformation with dplyr.

## Learning to wrangle: Is there a chastity belt on perception

Nearly all data in research methods is stored in two-dimensional tables, either called data-frames, tables or tibbles. There are other ways of storing data that you will discover in time but mainly we will be using tibbles (if you would like more info, type `vignette("tibble")` in the console). A tibble is really just a table of data with columns and rows of information. But within that table you can get different types of data, i.e. numeric, integer, and character.

|Type of Data     | Description                                                  |
|:------------|:-------------------------------------------------------------| 
|Numeric     | Numbers including decimals  |
|Integer     | Numbers without decimals  |
|Character     | Tends to contain letters or be words                       |
|Factor     | Nominal (categorical). Can be words or numbers (e.g., male/1, female/2)                       |


Today we are going to be using data from this paper: [Is there a Chastity Belt on Perception](http://journals.sagepub.com/doi/abs/10.1177/0956797617730892). You can read the full paper if you like, but we will summarise the paper for you. The paper asks, **does your ability to perform an action influence your perception?** For instance, does your ability to hit a tennis ball influence how fast you perceive the ball to be moving? Or to phrase another way, do expert tennis players perceive the ball moving slower than novice tennis players?  

This experiment does not use tennis players however, they used the Pong task: "a computerised game in which participants aim to block moving balls with various sizes of paddles". A bit like a very classic retro arcade game. Participants tend to estimate the balls as moving faster when they have to block it with a smaller paddle as opposed to when they have a bigger paddle. You can read the paper to get more details if you wish but hopefully that gives enough of an idea to help you understand the wrangling we will do on the data. We have cleaned up the data a little to start with. Let's begin!

## Activity 1: Set-up Data Wrangling 2 {#dw2-a1}

* Download <a href="PongBlueRedBack 1-16 Codebook.csv" download>PongBlueRedBack 1-16 Codebook.csv</a> into your chapter folder.  
* Set the working directory to your chapter folder. Ensure the environment is clear.    
* Open a new R Markdown document and save it in your working directory. Call the file "Data wrangling 2". 
* If you're on the server, avoid a number of issues by restarting the session - click `Session` - `Restart R` 
* Delete the default R Markdown welcome text and insert a new code chunk.
* Copy and paste the below code into this code chunk and then run the code.


```r
library("tidyverse")
pong_data <- read_csv("PongBlueRedBack 1-16 Codebook.csv")
summary(pong_data)
```

```
##   Participant     JudgedSpeed      PaddleLength   BallSpeed    TrialNumber    
##  Min.   : 1.00   Min.   :0.0000   Min.   : 50   Min.   :2.0   Min.   :  1.00  
##  1st Qu.: 4.75   1st Qu.:0.0000   1st Qu.: 50   1st Qu.:3.0   1st Qu.: 72.75  
##  Median : 8.50   Median :1.0000   Median :150   Median :4.5   Median :144.50  
##  Mean   : 8.50   Mean   :0.5471   Mean   :150   Mean   :4.5   Mean   :144.50  
##  3rd Qu.:12.25   3rd Qu.:1.0000   3rd Qu.:250   3rd Qu.:6.0   3rd Qu.:216.25  
##  Max.   :16.00   Max.   :1.0000   Max.   :250   Max.   :7.0   Max.   :288.00  
##  BackgroundColor      HitOrMiss       BlockNumber   
##  Length:4608        Min.   :0.0000   Min.   : 1.00  
##  Class :character   1st Qu.:0.0000   1st Qu.: 3.75  
##  Mode  :character   Median :1.0000   Median : 6.50  
##                     Mean   :0.6866   Mean   : 6.50  
##                     3rd Qu.:1.0000   3rd Qu.: 9.25  
##                     Max.   :1.0000   Max.   :12.00
```
  
`summary()` provides a quick overview of the variables in your dataset and can be a useful quick check that you have indeed imported the correct data. It will also provide some basic descriptive statistics or whether the data is character (text) data which can also be useful to check.

## Activity 2: Look at your data {#dw2-a2}

Let's have a look at the `pong_data` and see how it is organized. Click on `pong_data` in your environment.

In the dataset you will see that each row (observation) represents one trial per participant and that there were 288 trials for each of the 16 participants. The columns (variables) we have in the dataset are as follows:

| Variable       |       Type         |           Description               |
|:--------------:|:-------------------|:------------------------------------|
| Participant        | integer            | participant number                  |
| JudgedSpeed   | integer            | speed judgement (1=fast, 0=slow)    |
| PaddleLength         | integer            | paddle length (pixels)              |
| BallSpeed          | integer            | ball speed (2 pixels/4ms)          |
| TrialNumber         | integer            | trial number                        |
| BackgroundColor      | character          | background display colour           |
| HitOrMiss         | integer            | hit ball=1, missed ball=0       |
| BlockNumber  | integer            | block number (out of 12 blocks)     |

We will use this data to master our skills of the Wickham Six verbs, taking each verb in turn. You should refer to the explanations and example code in Data Wrangling 1 to help you complete these. There are **6 verbs to work through** and  after that we will briefly recap on two other functions before finishing with a quick look at pipes. Try each activity and ask your peers or your tutor if you need help.


## Activity 3: **`select()`** {#dw2-a3}

Either by inclusion (telling R all the variables you want to keep) or exclusion (telling R which variables you want to drop), select only the `Participant`, `PaddleLength`, `TrialNumber`, `BackgroundColor` and `HitOrMiss` columns from `pong_data` and store it in a new object named `select_dat`.  

## Activity 4: Reorder the variables {#dw2-a4}

`select()` can also be used to reorder the columns in a table as the new table will display the variables in the order that you wrote them. Use `select()` to keep only the columns `Participant`, `JudgedSpeed`, `BallSpeed`, `TrialNumber`, and `HitOrMiss` from `pong_data` but have them display in alphabetical order, left to right. Save this table in a new object named `reorder_dat`.

## Activity 5: **`arrange()`** {#dw2-a5}

Arrange the data by two variables: `HitOrMiss` (putting hits - 1 - first), and `JudgedSpeed` (fast judgement - 1 - first) and store this in an object named `arrange_dat`.

## Activity 6: `filter()` {#dw2-a6}

Use `filter()` to extract all Participants in the original `pong_data` that had:

* a fast speed judgement;
* for speeds 2, 4, 5, and 7;
* but missed the ball.   

Store this remaining data in a new object called `pong_fast_miss`


<div class='webex-solution'><button>Helpful Hint</button>


There are three parts to this filter so it is best to think about them individually and then combine them.

1. Filter all fast speed judgements (`JudgedSpeed`)

2. Filter for the speeds 2, 4, 5 and 7 (`BallSpeed`)

3. Filter for all Misses (`HitOrMiss`)

You could do this in three filters where each one uses the output of the preceding one, or remember that filter functions can take more than one argument. Also, because the `JudgedSpeed` and `HitOrMiss` are Integer you will need `==` instead of just `=`.

</div>


<br>

<div class="warning">
<p>The filter function is very useful but if used wrongly can give you very misleading findings. This is why it is very important to always check your data after you perform an action. Let's say you are working in comparative psychology and have run a study looking at how cats, dogs and horses perceive emotion. Let's say the data is all stored in the tibble <code>animal_data</code> and there is a column called <code>animals</code> that tells you what type of animal your participant was. Imagine you wanted all the data from just cats:</p>
<p><code>filter(animal_data, animals == "cat")</code></p>
<p>Exactly! But what if you wanted cats and dogs?</p>
<p><code>filter(animal_data, animals == "cat", animals == "dog")</code></p>
<p>Right? Wrong! This actually says "give me everything that is a cat and a dog". But nothing is a cat and a dog, that would be weird - like a dat or a cog. In fact you want everything that is either a cat <strong>or</strong> a dog:</p>
<p><code>filter(animal_data, animals %in% c("cat", "dog"))</code></p>
<p>You used this code when producing your own graph of babynames, it's a very helpful function so don't forget it exists!</p>
</div>

</div>


## Activity 7: `mutate()` {#dw2-a7}

In Data Wrangling 1, you learned how the `mutate()` function lets us create a new variable in our dataset. However, it also has another useful function in that it can be combined with `recode()` to create new columns with recoded values. For example, let's add a new column to `pong_data` in which the judged speed  is converted into a text label where `1` will become `Fast`, and `0` will become "Slow". Note that if you gave the recoded variable the same name as the original it would overwrite it.


```r
pong_data <- mutate(pong_data, 
                    JudgedSpeedLabel = recode(JudgedSpeed, 
                                                    "1" = "Fast", 
                                                    "0" = "Slow"))
```

The code here is is a bit complicated but:

* `JudgedSpeedLabel` is the name of your new column, 
* `JudgedSpeed` is the name of the old column and the one to take information from
* Fast and slow are the new codings of 1 and 0 respectively

The `mutate()` function is also handy for making some calculations on or across columns in your data. For example, say you realise you made a mistake in your experiment where your participant numbers should be 1 higher for every participant, i.e. Participant 1 should actually be numbered as Participant 2, etc. You would do something like:


```r
pong_data <- mutate(pong_data, Participant = Participant + 1)
```

Note here that you are giving the new column the same name as the old column `Participant`. What happens here is that you are **overwriting the old data with the new data**! So watch out, mutate can create a new column or overwrite an existing column, depending on what you tell it to do!  

Imagine you realise there is a mistake in your dataset and that all your trial numbers are wrong. The first trial (trial number 1) was a practice so should be excluded and your experiment actually started on trial 2. 

* Filter out all trials with the number 1 (`TrialNumber` column) from `pong_data`, 
* Then use the `mutate()` function to recount all the remaining trial numbers, starting them at one again instead of two. Overwrite `TrialNumber` in `pong_data` with this new data.

You can either do this in two separate steps and create a new object, or you can uses pipes `%>%` and do it it one line of code. 


<div class='webex-solution'><button>Helpful Hint</button>


Step 1. filter(`TrialNumber` does not equal 1) - remember to store this output in a variable?

Step 2. mutate(`TrialNumber` = TrialNumber minus 1)

</div>
  

## Activity 8: Summarising data {#dw2-a8}

Now we're going to calculate descriptive statistics for the data using `summarise()`. In this exercise we will calculate:

* The total (sum) number of hits for each combination of background colour and paddle length.
* The mean number of hits for each combination of background colour and paddle length

Because we want to produce descriptive statistics by groups (background colour and paddle length), there are two steps:

* Group the data by `BackgroundColor` and `PaddleLength` using `group_by()`.
* Then, use `summarise()` to calculate the total and mean number of hits (`HitOrMiss`) using the grouped data

We will do this using pipes to reduce the amount of code we write. Remember to try and read the code out loud and to pronounce `%>%` as 'and then'. Copy and paste the below code into a new code chunk and run the code.


```r
pong_data_hits<- pong_data %>% # take pong_data
  group_by(BackgroundColor, PaddleLength) %>% # then group it
  summarise(total_hits = sum(HitOrMiss, na.rm = TRUE),
            meanhits = mean(HitOrMiss, na.rm = TRUE)) # then summarise it
```

<div class="info">
<p>If you get what looks like an error that says <code>summarise() ungrouping output (override with .groups argument)</code>don't worry, this isn't an error it's just R telling you what it's done. This message was included in a very recent update to the <code>tidyverse</code> which is why it doesn't appear on some of the walkthrough vidoes.</p>
</div>

`summarise()` has a range of internal functions that make life really easy, e.g. `mean`, `sum`, `max`, `min`, etc. See the [dplyr cheatsheet](https://www.rstudio.com/wp-content/uploads/2015/02/data-wrangling-cheatsheet.pdf) for more examples.

<div class="info">
<p><code>na.rm = TRUE</code> is an argument that we can add when calculating descriptive statistics to tell R what to do if there are missing values. In this dataset, there are no missing values but if there were and we asked R to calculate the mean, it would return <code>NA</code> as the result because it doesn't know how to average nothing. Remember this argument exists, you will use it often and it save you a lot of time!</p>
</div>

* View `pong_data_hits` and enter the number of hits made with the small paddle (50) and the red colour background in this box: <input class='webex-solveme nospaces' size='3' data-answer='["517"]'/>

**Note:**

* The name of the column within `pong_data_hits` is `total_hits`; this is what you called it in the above code. You could have called it anything you wanted but always try to use something sensible.

* Make sure to call your variables something you (and anyone looking at your code) will understand and recognize later (i.e. not variable1, variable2, variable3. etc.), and avoid spaces (use_underscores_never_spaces). 


<div class="try">
<p>After grouping data together using the <code>group_by()</code> function and then performing a task on it, e.g. <code>filter()</code>, it can be very good practice to ungroup the data before performing another function. Forgetting to ungroup the dataset won't always affect further processing, but can really mess up other things. Again just a good reminder to always check the data you are getting out of a function a) makes sense and b) is what you expect.</p>
</div>

## Activity 9: Counting observations {#dw2-a9}

Often it is helpful to know how many observations you have, either in total, or broken down by groups. This can help you spot if something has gone wrong in a calculation, for example, if you've done something with the code and your mean or median is only being calculated using a subset of the values you intended.

There are two ways of counting the number of observations. The first uses `summarise()` and the function `n()`. The below code is the same as above, but with an extra line that will add a column called `n` to the table that contains the number of observations in each group. This is useful for when you want to add a column of counts to a table of other descriptive statistics.


```r
pong_count <- pong_data %>% 
  group_by(BackgroundColor, PaddleLength) %>% 
  summarise(total_hits = sum(HitOrMiss, na.rm = TRUE),
            meanhits = mean(HitOrMiss, na.rm = TRUE),
            n = n())
```

However, if you're just interested in counts rather than also calculating descriptives, this method is a bit clunky. Instead, we can use the function `count()` which is specifically designed to count observations and doesn't require the use of `summarise()` or `group_by()`.

To count the total number of observations in the dataset:


```r
pong_data %>% # take pong_data
  count() # and then count the observations in it
```

```
## # A tibble: 1 x 1
##       n
##   <int>
## 1  4608
```

To count the number of observations in each level of a variable:


```r
pong_data %>%
  count(BackgroundColor)
```

```
## # A tibble: 2 x 2
##   BackgroundColor     n
##   <chr>           <int>
## 1 blue             2304
## 2 red              2304
```

Which method you use will depend on whether you want to add the counts to a table of other descriptives, but both functions are useful to know.

## Pipes (**`%>%`**) {#dw2-pipes}  

Finally, a quick recap on pipes. Here is an example of code that doesn't use pipes to find how many hits there were with the large paddle length and the red background.


```r
# First we group the data accordingly, storing it in `pong_data_group`
pong_data_group <- group_by(pong_data, BackgroundColor, PaddleLength)

# And then we summarise it, storing the answer in `total_hits`
pong_data_hits <- summarise(pong_data_group, total_hits = sum(HitOrMiss))

# And filter just the red, small paddle hits
pong_data_hits_red_small <- filter(pong_data_hits, BackgroundColor == "red", PaddleLength == 250)
```

We can make our code even more efficient, using less code, by stringing our sequence of functions together using pipes. This would look like:


```r
# Same pipeline using pipes
pong_data_hits_red_small <- pong_data %>% 
  group_by(BackgroundColor, PaddleLength) %>% 
  summarise(total_hits = sum(HitOrMiss)) %>% 
  filter(BackgroundColor == "red", PaddleLength == 250) 
```

One last point on pipes is that they can be written in a single line of code but it's much easier to see what the pipe is doing if each function takes its own line. Every time you add a function to the pipeline, remember to add a `%>%` first and **note that when using separate lines for each function, the `%>%` must appear at the end of the line and not the start of the next line**. Compare the two examples below. The first won't work but the second will because the second puts the pipes at the end of the line where they need to be!


```r
# Piped version that wont work 
data_arrange <- pong_data 
                %>% filter(PaddleLength == "50")
                %>% arrange(BallSpeed) 

# Piped version that will work 
data_arrange <- pong_data %>%
                filter(PaddleLength == "50") %>%
                arrange(BallSpeed) 
```


<div class="try">
<p>Where piping becomes most useful is when we <strong>string a series of functions together</strong>, rather than using them as separate steps and having to save the data each time under a new variable name and getting ourselves all confused. In the non-piped version we have to create a new variable each time, for example, <code>data</code>, <code>data_filtered</code>, <code>data_arranged</code>, <code>data_grouped</code>, <code>data_summarised</code> just to get to the final one we actually want, which was <code>data_summarised</code>. This creates a lot of variables and tibbles in our environment and can make everything unclear and eventually slow down our computer. The piped version however uses one variable name, saving space in the environment, and is clear and easy to read. With pipes we skip unnecessary steps and avoid cluttering our environment.</p>
</div>

### Finished! {#dw2-fin}

We have now learned a number of functions and verbs that you will need as you progress through this book.  You will use them in the next chapter so be sure to go over these and try them out to make yourself more comfortable with them.  If you have any questions please post them on Teams. **Happy Wrangling!**

## Activity solutions {#dw2-sols}

Below you will find the solutions to the above questions. Only look at them after giving the questions a good try and speaking to the tutor about any issues.

### Activity 3 {#dw2-a3sol}


<div class='webex-solution'><button>Solution Task 3</button>


```r
# To include variables:
select_dat <- select(pong_data, Participant, PaddleLength, TrialNumber, BackgroundColor, HitOrMiss)

# To exclude variables:
select_dat <-select(pong_data, -JudgedSpeed, -BallSpeed, -BlockNumber)
```

</div>
  

**click the tab to see the solution**
<br>

### Activity 4 {#dw2-a4sol}


<div class='webex-solution'><button>Solution Activity 4</button>


```r
reorder_dat <- select(pong_data, BallSpeed, HitOrMiss, JudgedSpeed, Participant, TrialNumber)
```

</div>
  

**click the tab to see the solution**
<br>


### Activity 5 {#dw2-a5sol}


<div class='webex-solution'><button>Solution Task 2</button>


```r
arrange_dat <- arrange(pong_data, desc(HitOrMiss), desc(JudgedSpeed))
```

</div>
  

**click the tab to see the solution**
<br>

### Activity 6 {#dw2-a6sol}


<div class='webex-solution'><button>Solution Activity 6</button>


```r
pong_fast_miss< - filter(pong_data, 
                         JudgedSpeed == 1, 
                         BallSpeed %in% c("2", "4", "5", "7"), 
                         HitOrMiss == 0)
```

</div>
  

**click the tab to see the solution**
<br>

### Activity 7 {#dw2-a7sol}


<div class='webex-solution'><button>Solution Activity 7 4</button>


```r
# this is the solution if you used two separate steps

pong_data_filt <- filter(pong_data, TrialNumber >= 2) 
# you can call this variable anything, as long as it makes sense to yourself and others

pong_data <- mutate(pong_data_filt, TrialNumber = TrialNumber - 1)

# this is the solution if you used pipes

pong_data<- filter(pong_data, TrialNumber >= 2) %>%
  mutate(TrialNumber = TrialNumber - 1)
```

</div>
  

**click the tab to see the solution**
<br>


## Debugging tips {#dw2-debug}

* Make sure you have spelt the data file name **exactly** as it is shown. Spaces and everything. Do not change the name of the csv file, fix your code instead. If you have a different name for your file than someone else then your code is not reproducible.
* Remember when uploading data we use `read_csv()` which has an underscore, whereas the data file itself will have a dot in its name, `filename.csv`. 
* Finally, check that the datafile is actually in the folder you have set as your working directory. 

## Test yourself {#dw2-test}

1. What type of data would these most likely be:

* Male = <select class='webex-select'><option value='blank'></option><option value='answer'>Character</option><option value=''>Numeric</option><option value=''>Integer</option></select>

* 7.15 = <select class='webex-select'><option value='blank'></option><option value=''>Character</option><option value='answer'>Numeric</option><option value=''>Integer</option></select>

* 137 = <select class='webex-select'><option value='blank'></option><option value=''>Character</option><option value=''>Numeric</option><option value='answer'>Integer</option></select>


<div class='webex-solution'><button>Explain these answers</button>

There is a lot of different types of data and as well as different types of levels of measurements and it can get very confusing. It's important to try to remember which is which because you can only do certain types of analyses on certain types of data and certain types of measurements. For instance, you can't take the average of Characters just like you can't take the average of Categorical data. Likewise, you can do any maths on Numeric data, just like you can on Interval and Ratio data. Integer data is funny in that sometimes it is Ordinal and sometimes it is Interval, sometimes you should take the median, sometimes you should take the mean. The main point is to always know what type of data you are using and to think about what you can and cannot do with them.

</div>


<br>

2. Which of the Wickham Six would you use to sort columns from smallest to largest: <select class='webex-select'><option value='blank'></option><option value=''>select</option><option value=''>filter</option><option value=''>mutate</option><option value='answer'>arrange</option><option value=''>group_by</option><option value=''>summarise</option></select>

3. Which of the Wickham Six would you use to calculate the mean of a column: <select class='webex-select'><option value='blank'></option><option value=''>select</option><option value=''>filter</option><option value=''>mutate</option><option value=''>arrange</option><option value=''>group_by</option><option value='answer'>summarise</option></select>

4. Which of the Wickham Six would you use to remove certain observations - e.g. remove all males: <select class='webex-select'><option value='blank'></option><option value=''>select</option><option value='answer'>filter</option><option value=''>mutate</option><option value=''>arrange</option><option value=''>group_by</option><option value=''>summarise</option></select> 

5. What does this line of code say? `data %>% filter() %>% group_by() %>% summarise()`: <select class='webex-select'><option value='blank'></option><option value=''>take the data and then group it and then filter it and then summarise it</option><option value='answer'>take the data and then filter it and then group it and then summarise it</option><option value=''>take the data and then summarise it and then filter it and then group it</option><option value=''>take the data and then group it and then summarise it and then filter it</option></select>  
