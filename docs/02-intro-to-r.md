# Intro to R

There are nine activities in total for this chapter, but don't worry, they are broken down into very small steps!

## Activity 1: Create the working directory {#intro-a1}

If you want to load data into R, or save the output of what you've created (which you almost always will want to do), you first need to tell R where the **working directory** is. All this means is that we tell R where the files we need (such as raw data) are located and where we want to save any files you have created. Think of it just like when you have different subjects, and you have separate folders for each topic e.g. biology, history and so on. When working with R, it's useful to have all the data sets and files you need in one folder.

We recommend making a new folder called "Research Methods R" with sub-folders for each chapter and saving any data, scripts, and portfolio files for each chapter into these folders. If you're using R on your laptop rather than the server, we suggest that you save your work onto a cloud storage server like OneDrive so that you never lose your work.

* Choose a location for your book work and then create the necessary folders for each chapter.

<div class="warning">
<p>Whatever you do, don't call the folder your keep your R work in "R". If you do this, sometimes R has an identity crisis and won't save or load your files properly.</p>
</div>

## Activity 1b: Upload data files to the server {#intro-a1b}

The main disadvantage to using the R server is that you will need to upload and download any files you are working on to and from the server (if you are using a local installation on your laptop you can skip this step).

-   Log on to the [R server](https://rstudio.psy.gla.ac.uk/)
-   In the file pane click `New folder` and create the same structure you created on your computer.

Going forward throughout this book, you can either download files to your local computer or, if you're using the server, you'll do an extra step where you also upload them to the sever. We're not going to use any data files in this session but let's try an example to make it clear how you get the data files on to the server.

-   Download <a href="ahi-cesd.csv" download>`ahi-cesd.csv`</a> and <a href="participant-info.csv" download>`participant-info.csv`</a> into the chapter folder on your computer. If you are working on the server, you will need to uploaded the files to the server as well.\
-   Click `Upload` then `Browse`and choose the folder for the chapter you're working on.
-   Click `Choose file` and go and find the data you want to upload.

Please be aware that **there is no link between your computer and the R server**. If you change files on the server, they won't magically appear on your computer and you need to be very careful when you submit your assessment files that you're submitting the right thing (last year we had lots of student submit blank files). This is the main reason we recommend installing R on your computer if you can.

## Activity 2: Set the working directory {#intro-a2}

Once you have created your folders you can set the working directory by clicking `Session` -\> `Set Working Directory` -\> `Choose Directory` and then select the relevant folder for this chapter as your working directory.

<div class="figure" style="text-align: center">
<img src="images/working-dir.png" alt="Setting the working directory" width="100%" />
<p class="caption">(\#fig:img-working-dir)Setting the working directory</p>
</div>

## R Markdown for R book work and portfolio assignments

For the R book work and portfolio assignments you will use a worksheet format called R Markdown (abbreviated as Rmd) which is a great way to create dynamic documents with embedded chunks of code. These documents are self-contained and fully reproducible (if you have the necessary data, you should be able to run someone else's analyses with the click of a button) which makes it very easy to share. This is an important part of your open science training as one of the reasons we are using R Studio is that it enables us to share open and reproducible information. Using these worksheets enables you to keep a record of all the code you write during this course, and when it comes time for the portfolio assignments, we can give you a task you can and then fill in the required code.

For more information about R Markdown feel free to have a look at their main webpage sometime <http://rmarkdown.rstudio.com>. The key advantage of R Markdown is that it allows you to write code into a document, along with regular text, and then **knit** it using the package `knitr` to create your document as either a webpage (HTML), a PDF, or Word document (.docx).

## Activity 3: Open and save a new R Markdown document {#intro-a3}

To open a new R Markdown document click the 'new item' icon and then click 'R Markdown'. You will be prompted to give it a title, call it "Intro to R". Also, change the author name to your GUID as this will be good practice for the portfolio assignments. Keep the output format as HTML.

<div class="figure" style="text-align: center">
<img src="images/new-markdown.png" alt="Opening a new R Markdown document" width="100%" />
<p class="caption">(\#fig:img-new-markdown)Opening a new R Markdown document</p>
</div>

Once you've opened a new document be sure to save it by clicking `File` -\> `Save as`. Name this file "Intro to R" as well. If you've set the working directory correctly, you should now see this file appear in your file viewer pane in the bottom right hand corner like in the example below (your file names and folders will be different).

<div class="figure" style="text-align: center">
<img src="images/file-dir.png" alt="New file in working directory" width="100%" />
<p class="caption">(\#fig:img-file-dir)New file in working directory</p>
</div>

## Activity 4: Create a new code chunk {#intro-a4}

When you first open a new R Markdown document you will see a bunch of welcome text that looks like this:

<div class="figure" style="text-align: center">
<img src="images/markdown-default.png" alt="New R Markdown text" width="100%" />
<p class="caption">(\#fig:img-markdown-default)New R Markdown text</p>
</div>

Do the following steps:\
\* Delete **everything** below line 7\
\* On line 8 type "About me"\
\* Click `Insert` -\> `R`

Your Markdown document should now look something like this:

<div class="figure" style="text-align: center">
<img src="images/new-chunk.png" alt="New R chunk" width="100%" />
<p class="caption">(\#fig:img-new-chunk)New R chunk</p>
</div>

What you have created is a **code chunk**. In R Markdown, anything written in the white space is regarded as normal text, and anything written in a grey code chunk is assumed to be code. This makes it easy to combine both text and code in one document.

<div class="warning">
<p>When you create a new code chunk you should notice that the grey box starts and ends with three back ticks ```. One common mistake is to accidentally delete these back ticks. Remember, code chunks are grey and text entry is white - if the colour of certain parts of your Markdown doesn't look right, check that you haven't deleted the back ticks.</p>
</div>

## Activity 5: Write some code {#intro-a5}

Now we're going to use the code examples you read about in Programming Basics to add some simple code to our R Markdown document. In your code chunk write the below code but replace the values of name/age/birthday with your own details). Note that text values and dates need to be contained in quotation marks but numerical values do not. Missing and/or unnecessary quotation marks are a common cause of code not working - remember this!


```r
name <- "Emily" 
age <- 35
today <- Sys.Date()
next_birthday <- as.Date("2021-07-11")
```

## Running code

When you're working in an R Markdown document, there are several ways to run your lines of code.

First, you can highlight the code you want to run and then click `Run` -\> `Run Selected Line(s)`, however this is very slow.

<div class="figure" style="text-align: center">
<img src="images/run1.png" alt="Slow method of running code" width="100%" />
<p class="caption">(\#fig:img-run1)Slow method of running code</p>
</div>

Alternatively, you can press the green "play" button at the top-right of the code chunk and this will run **all** lines of code in that chunk.

<div class="figure" style="text-align: center">
<img src="images/run2.png" alt="Slightly better method of running code" width="100%" />
<p class="caption">(\#fig:img-run2)Slightly better method of running code</p>
</div>

Even better though is to learn some of the keyboard shortcuts for R Studio. To run a single line of code, make sure that the cursor is in the line of code you want to run (it can be anywhere) and press `ctrl + enter`. If you want to run all of the code in the code chunk, press `ctrl + shift + enter`. Learn these shortcuts, they will make your life easier!

## Activity 6: Run your code {#intro-a6}

Run your code using one of the methods above. You should see the variables `name`, `age`, `today`, and `next_birthday` appear in the environment pane in the top right corner.

## Activity 7: Inline code {#intro-a7}

An incredibly useful feature of R Markdown is that R can insert values into your writing using **inline code**. If you've ever had to copy and paste a value or text from one file in to another, you'll know how easy it can be to make mistakes. Inline code avoids this. It's easier to show you what inline code does rather than to explain it so let's have a go.

First, copy and paste this text exactly (do not change *anything*) to the **white space** underneath your code chunk.


```r
My name is `r name` and I am `r age` years old. It is `r next_birthday - today` days until my birthday.
```

## Activity 8: Knitting your file {#intro-a8}

Nearly finished! As our final step we are going to "knit" our file. This simply means that we're going to compile our code into a document that is more presentable. To do this click `Knit` -\> `Knit to HMTL`. R Markdown will create a new HTML document and it will automatically save this file in your working directory.

As if by magic, that slightly odd bit of text you copied and pasted now appears as a normal sentence with the values pulled in from the objects you created.

**My name is Emily and I am 35 years old. It is -68 days until my birthday.**

We're not going to use this function very often in the rest of the course but hopefully you can see just how useful this would be when writing up a report with lots of numbers! R Markdown is an incredibly powerful and flexible format - this book was written using it! If you want to push yourself with R, additional functions and features of R Markdown would be a good place to start.

Before we finish, there are a few final things to note about knitting that will be useful for the portfolio and quantitative project:

-   R Markdown will only knit if your code works - this is a good way of checking for the portfolio assignments whether you've written legal code!\
-   You can choose to knit to a Word document rather than HTML. This can be useful for e.g., sharing with others, however, it may lose some functionality and it probably won't look as good so we'd recommend always knitting to HTML.\
-   You can choose to knit to PDF, however, unless you're using the server this requires an LaTex installation and is quite complicated. If you don't already know what LaTex is and how to use it, do not knit to PDF. If you do know how to use LaTex, you don't need us to give you instructions!
-   R will automatically open the knitted HTML file in the viewer, however, you can also navigate to the folder it is stored in and open the HTML file in your web browser (e.g., Chrome or Firefox).

## Activity 9: Make R your own {#intro-a9}

Finally, you can customise how R Studio looks to make it feel more like your own personal version. Click `Tools` - `Global Options` - `Apperance`. You can change the default font, font size, and general appearance of R Studio, including using dark mode. Play around with the settings and see which one you prefer - you're going to spend a lot of time with R, it might as well look nice!

## Finished

And you're done! On your very first time using R you've not only written functioning code but you've written a reproducible output! You could send someone else your R Markdown document and they would be able to produce exactly the same HTML document as you, just by pressing knit.

The key thing we want you to take away from this chapter is that R isn't scary. It might be very new to a lot of you, but we're going to take you through it step-by-step. You'll be amazed at how quickly you can start producing professional-looking data visualisations and analysis.

If you have any questions about anything contained in this chapter or in Programming Basics, please use the Research Methods channel on Teams.
