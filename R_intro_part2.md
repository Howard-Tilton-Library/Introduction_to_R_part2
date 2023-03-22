---
title: "Intro to R, Part II: Projects, Packages, Processing, Plots"
author: "Mike Ellis (he/him/his) <br> PhD Candidate <br> Ecology & Evolutionary Biology <br> Tulane University <br> mellis5@tulane.edu"
date: "22 March 2023"

format: 
  html:
    toc: true
    toc-location: left
    toc-depth: 3
    number-sections: true
    number-depth: 1
    theme: lumen
    highlight-style: github
    code-overflow: wrap
    code-fold: false
    code-copy: true
    code-link: false
    code-tools: false
    code-block-border-left: "#0C3823"
    code-block-bg: "#eeeeee"
    fig-cap-location: margin
    linestretch: 1.25
    fontsize: "large"
    embed-resources: true
    df-print: paged

execute:
  echo: true
  keep-md: true
---



![](Data_In/Figures/TUL_Logos_narrow.png){fig-align="center"}

Welcome! This tutorial from Tulane University's Howard-Tilton Memorial Library is the second half of an Intro to *R* workshop. It is targeted towards faculty, post-docs, graduate students, and undergraduates new to *R*. It's goal is to teach you the basic *R* coding skills you're likely to need for every coding endeavor.

In part I, you learned the fundamentals of *R* and built a foundation for working with your own data. In this second installment, you'll learn to streamline project management, incorporate packages to increase functionality, code more efficiently in the "tidy" dialect, implement more complex conditional and iterative processes, group and summarize your data, and visualize your results with figures.

Happy coding!

<br>

# **Projects**

## Create your first project

**Projects** are convenient places to store all of the files associated with whatever it is you're working on in *RStudio*. Bundling up files in projects tends to improve your organizational efficiency, and it streamlines the normal, everyday hassle of importing data and exporting product. Projects are also easily shared with collaborators and save them the headache of replacing your local file paths with their local file paths.

::: callout-tip
## <font size="5"> Try it </font>

<font size="4"> Create a new project to manage materials for this tutorial. </font>
:::

-   *RStudio* Really encourages you to use projects, giving you three easy options to start one. Simply choose "New Project..." from the File drop-down menu, or click one of these two buttons:

![Click one of these to start a new project!](Data_In/Figures/new_projects.png)

-   When creating a new project, you have the option of putting it in a pre-existing folder, in which case it will take the name of that folder, or creating a new folder with the same project name. (You may also see an option to start a new project with version control, which is a more advanced feature beyond the scope of this tutorial).

::: callout-note
## <font size="5"> Note </font>

<font size="4"> This is more advanced, but if you're active on GitHub or hope to be someday, then you'll come to love R projects for how smoothly they integrate with GitHub repositories! </font>
:::

-   Choose "New Directory" and then "New Project" to create a folder/project somewhere that makes sense for you. Give it a meaningful but short name like "Intro_to_R\_part2". Don't check any of the optional boxes for now.

-   I recommend keeping your R project folders as organized as possible so you can navigate quickly and share clean work spaces with collaborators since sharing science often means sharing scripts, data, and R output like figures. You may want to consider adding some sub-folders to your project Here's what my setup looks like for this lesson (and most of my R projects):

![An example of clean project organization](Data_In/Figures/project_organization.png)

-   Now that you've created a project, you can always access it using the Project drop-down menu in the right side of *RStudio*. The menu will display the currently open/active project. Check to make sure your project for this tutorial is open to ensure functionality of upcoming code.

-   You can directly access files and folders in your open project through the Files tab in *RStudio*. Very convenient!

## Add R script to project

Great! Now that you've got a project up and running, start a new R script for this tutorial. You can do so using the New File button in the top left, the File drop-down menu, or -- a new option -- the "New Blank File" drop-down menu under your Files tab.

![Add and name a new R script to your project.](Data_In/Figures/new_script.png)

## Add data to project, import

We'll need some data to work on for this tutorial. I've added some additional rows to the data we created in Part I of this tutorial to highlight some new functions. You can download these new data directly from the web and into *RStudio* using `download.file()`.

When you're working in a Project, R automatically sets it as your working directory! That means you don't have to type out the entire path to access your project folder or the files therein.

::: callout-tip
## <font size="5"> Try it </font>

<font size="4"> Enter the following code into your new script to download data from the web and directly into your project directory. Then, import it. </font>
:::


::: {.cell}

```{.r .cell-code}
# Setup ----

# First, take a quick look at the help file for download.file()
?download.file

# NOTE: download.file() takes several arguments. We're most concerned with the url, or address of the hosted file for download, and "destfile", or the destination and name you'd like to give the downloaded file.

# NOTE: All of these arguments are text strings and therefore belong in quotations.

# NOTE: You MUST provide a file type in your desired file name (in this case, .csv).
```
:::


You may need to slightly alter the `destfile =` argument depending on whether or not you'd like to store these data in a project sub-folder.


::: {.cell}

```{.r .cell-code}
# I have a sub-folder in my project called "Data_In" and therefore need to specify that location before my chosen file name.
download.file(url = "https://libguides.tulane.edu/ld.php?content_id=71057769", destfile = "Data_In/intro2_bird_data.csv")

# If you don't have a similar sub-folder, you only need to provide destfile with a file name. It will automatically save to your project folder.
download.file("https://libguides.tulane.edu/ld.php?content_id=71057769", "intro2_bird_data.csv")
```
:::


These data should now appear in your *RStudio* files pane. Let's go ahead and import them.


::: {.cell}

```{.r .cell-code}
# Importing data
# Remember to insert or drop a sub-folder as needed.
# This code says "I will create an object named `birds` and assign to it the data contained in the following .csv file stored in my project or specified project sub-folder."
birds <- read.csv("Data_In/intro2_bird_data.csv")
```
:::


Don't forget to scope out your data after importing! I'd recommend entering `View()` into your console. And remember, you've also got...


::: {.cell}

```{.r .cell-code}
head(birds)
tail(birds)
str(birds)
```
:::


# **Packages**

In this tutorial, we're going to do some more complex data munging and analyses requiring specialized functions not included in base *R*. **Packages**, or **Libraries**, are open-source bundles of pre-coded functions (and oftentimes data) that we can install and access in *RStudio* to suit our own needs.

One of the most popular *R* packages is called `tidyverse`. It's actually a collection of packages geared towards making your life *much* easier...at least when it comes to data work. To access it, you need to install it (once) and then load it (once per session when re-opening *RStudio*).

## Installing

::: callout-tip
## <font size="5"> Try it </font>

<font size="4"> Install and load `tidyverse` packages, also known as libraries. </font>
:::

You can view all the packages in base *R* and all the packages you've ever installed by clicking the Packages tab in the *RStudio* pane including your Files and Plots tabs. Do so now, and underneath the tab names, you'll see buttons to Install and Update packages.

::: callout-warning
## <font size="5"> Are you online? </font>

<font size="4"> Most packages are hosted online by *CRAN, The Comprehensive R Archive Network*, so naturally you'll need an internet connection to download and install them! </font>
:::

Click that "Install" button at the top of the pane, type in `tidyverse` in the Packages bar, make sure the "Install Dependencies" box is **checked**, and select the "Install" option in the pop-up window. (For most users, the default library path to install to is a perfectly good option). *RStudio* will then take care of the rest, showing you its progress in your Console pane.

![You should see something like this when installing tidyverse.](Data_In/Figures/install.png){width="811"}

::: callout-note
## <font size="5"> Note </font>

<font size="4"> Packages are occasionally updated by their creators, often to maintain functionality when *R* or *RStudio* are updated. As a result, you'll have to update your software and/or packages every now and then. When the time comes, you'll typically get warnings from *RStudio* while loading or installing new packages. You can use the Update button at the top of the Packages tab to take care of business. </font>
:::

## Loading

The nice thing about installing packages is that you generally only have to do it once. To *access*, a package, however, you have to tell *RStudio* to open it every time you start a new session. Fortunately, that's quick and easy too. All you have to do is plug the desired package into the `library()` function.

::: callout-note
## <font size="5"> Note </font>

<font size="4"> It's good practice to put code to load libraries, aka packages, in the very beginning of your *script*, not your console! Loading all your required packages up front, not buried halfway through your document can save you and your collaborators from package conflicts down the road. </font>
:::


::: {.cell}

```{.r .cell-code}
# Running library(tidyverse) allows you to access data and functions from the collection of packages that make up the tidyverse.
# Put this somewhere near the top of your script in your Setup section.
library(tidyverse)
```

::: {.cell-output .cell-output-stderr}
```
── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
✔ dplyr     1.1.0     ✔ readr     2.1.4
✔ forcats   1.0.0     ✔ stringr   1.5.0
✔ ggplot2   3.4.1     ✔ tibble    3.2.0
✔ lubridate 1.9.2     ✔ tidyr     1.3.0
✔ purrr     1.0.1     
── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
✖ dplyr::filter() masks stats::filter()
✖ dplyr::lag()    masks stats::lag()
ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```
:::
:::


See those Conflict messages? Those show up because different package creators sometimes use the same word when naming functions with different applications. When calling a function with a conflicting name, e.g., `filter()`, *RStudio* will automatically use the version of the function from the most recently loaded library, in this case `dplyr`, which is part of `tidyverse`. If you want to use a different version of the function in your script, you can specify it by putting the package name in front of two colons and the function title, e.g., `stats::filter(your_data_here)` will override `filter(your_data_here)` from the `dplyr` package after loading `tidyverse`.

# **Welcome to the Tidyverse**

## About

Great! You can now use the `tidyverse` to create and process "tidy" data, which boils down to making sure your data are organized with one value per cell and one cell per column and row. Storing your data this way greatly streamlines munging and analysis workflows.

The `tidyverse` has a lot of strengths over base *R*. The most obvious of which that you can benefit from almost immediately are that it...

1.  reduces repetitive wordiness (like constantly having to supply your data and your data\$column names) while...
2.  simultaneously targeting user-specified data in dynamically moving data sets.
3.  And on top of that, it allows you to quickly and clearly string together functions in an obvious order without having to constantly create and reference new objects or wrap 10 functions inside of one another! More on this in the **pipes** section below.

Here are the `tidyverse` packages you now have access to and their basic purposes. When you're working with your own data and come across a complex roadblock, see if your situation can be solved by the tools in one of these packages:

-   `tidyr`
    -   Tools for reshaping your data to make it tidy. Too many values in a cell? Should your rows be columns or vice versa? This is how you get your data to where it needs to be for analysis.
-   `dplyr`
    -   Common data manipulation tools for making changes to a single data set and/or comparing and combining multiple data sets.
-   `stringr`
    -   A great toolbox for working with strings, aka non-numeric character data. Find, replace, mutate, subset, combine/separate and more.
-   `ggplot2`
    -   The gold standard for creating beautiful plots and figures in *R*.
-   `purrr`
    -   Powerful tools for applying functions to multiple vectors, columns, or lists of data at once.
-   `forcats`
    -   Tools for working with **factor** variables, i.e., categorical data like treatment or forest type.
-   `tibble`
    -   Functions for working with **tibbles**, which are what the `tidyverse` calls its new and improved versions of data frames. They're largely interchangeable and conversions happen behind the scenes, so you generally don't need to worry about it. If you see tibble, think data frame.
-   `readr`
    -   For importing tabular data in more exotic or complicated formats.

::: callout-note
## <font size="5"> Note </font>

<font size="4"> To learn more about tidy data and the `tidyverse`, explore the resources available at <https://www.tidyverse.org/learn/> and the [RStudio cheatsheets](https://posit.co/resources/cheatsheets/?type=posit-cheatsheets&_page=2/) for `tidyverse` packages. </font>
:::

## Popular `tidyverse` functions

`tidyverse` functions work pretty much the same as functions from base *R*. Supply a function with data, feed it any necessary arguments, and put the result into a container object:

`object -> my.function(data, argument = desired_manipulation)`

Below we'll create a smaller data frame to test functions on, making sure not to overwrite it (meaning don't set it equal to anything with `<-`). This way, changes we make aren't stored and we can continue to manipulate the smaller test object, getting expected -- but *temporary* -- results every time.

::: callout-tip
## <font size="5"> Try it </font>

<font size="4"> Familiarize yourself with some of the most common and useful functions in the `tidyverse`. </font>
:::

### `select()`

A useful function and removing or reordering columns by name. Specifying column names rather than column number ensures you select the right data even when columns switch places.


::: {.cell}

```{.r .cell-code}
### Tidyverse functions ----

# Before we begin, let's save a smaller data set to practice with using what we learned in Intro to R, part I
small_birds <- birds[c(1:2, 12:13, 22:23),]

# Next, remind yourself of the column names and their order
colnames(small_birds)
```

::: {.cell-output .cell-output-stdout}
```
[1] "species"        "family"         "wing_length_mm" "mass_g"        
[5] "mass_to_wing"  
```
:::

```{.r .cell-code}
# select() allows you to select columns to keep.
# You can also reorder them at the same time.
# Here we select three columns from the birds data frame, moving family before species: 
select(small_birds, family, species, mass_g)
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["family"],"name":[1],"type":["chr"],"align":["left"]},{"label":["species"],"name":[2],"type":["chr"],"align":["left"]},{"label":["mass_g"],"name":[3],"type":["dbl"],"align":["right"]}],"data":[{"1":"Falconidae","2":"American Kestrel","3":"108.7","_rn_":"1"},{"1":"Falconidae","2":"American Kestrel","3":"111.2","_rn_":"2"},{"1":"Corvidae","2":"American Crow","3":"455.7","_rn_":"12"},{"1":"Corvidae","2":"American Crow","3":"460.0","_rn_":"13"},{"1":"Turdidae","2":"American Robin","3":"72.1","_rn_":"22"},{"1":"Turdidae","2":"American Robin","3":"84.1","_rn_":"23"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::

```{.r .cell-code}
# You can also specify columns to remove by putting a minus or hyphen in front of their names
select(small_birds, -species, -family)
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["wing_length_mm"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["mass_g"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["mass_to_wing"],"name":[3],"type":["dbl"],"align":["right"]}],"data":[{"1":"175.3","2":"108.7","3":"0.62","_rn_":"1"},{"1":"179.2","2":"111.2","3":"0.62","_rn_":"2"},{"1":"295.7","2":"455.7","3":"1.54","_rn_":"12"},{"1":"279.7","2":"460.0","3":"1.64","_rn_":"13"},{"1":"120.8","2":"72.1","3":"0.60","_rn_":"22"},{"1":"135.4","2":"84.1","3":"0.62","_rn_":"23"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::

```{.r .cell-code}
# Combining with c() to remove a list of columns
select(small_birds, -c(family, mass_g, mass_to_wing))
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[2],"type":["dbl"],"align":["right"]}],"data":[{"1":"American Kestrel","2":"175.3","_rn_":"1"},{"1":"American Kestrel","2":"179.2","_rn_":"2"},{"1":"American Crow","2":"295.7","_rn_":"12"},{"1":"American Crow","2":"279.7","_rn_":"13"},{"1":"American Robin","2":"120.8","_rn_":"22"},{"1":"American Robin","2":"135.4","_rn_":"23"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::
:::


::: callout-note
## <font size="5"> Note </font>

<font size="4"> There are several additional ways to select columns based on things like variable class and starting letter. Run `?select` for more info. If you're only looking to reorder columns, also check out the help page for `relocate()`! </font>
:::

### `arrange()`

This function can be used to sort your data alphabetically or numerically


::: {.cell}

```{.r .cell-code}
# Arranging by wing_length_mm
arrange(small_birds, wing_length_mm)
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["family"],"name":[2],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["mass_g"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["mass_to_wing"],"name":[5],"type":["dbl"],"align":["right"]}],"data":[{"1":"American Robin","2":"Turdidae","3":"120.8","4":"72.1","5":"0.60"},{"1":"American Robin","2":"Turdidae","3":"135.4","4":"84.1","5":"0.62"},{"1":"American Kestrel","2":"Falconidae","3":"175.3","4":"108.7","5":"0.62"},{"1":"American Kestrel","2":"Falconidae","3":"179.2","4":"111.2","5":"0.62"},{"1":"American Crow","2":"Corvidae","3":"279.7","4":"460.0","5":"1.64"},{"1":"American Crow","2":"Corvidae","3":"295.7","4":"455.7","5":"1.54"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::

```{.r .cell-code}
# Notice that arrange sorted by ascending order.
# Wrap your column name in the desc() function to sort from largest to smallest
arrange(small_birds, desc(wing_length_mm))
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["family"],"name":[2],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["mass_g"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["mass_to_wing"],"name":[5],"type":["dbl"],"align":["right"]}],"data":[{"1":"American Crow","2":"Corvidae","3":"295.7","4":"455.7","5":"1.54"},{"1":"American Crow","2":"Corvidae","3":"279.7","4":"460.0","5":"1.64"},{"1":"American Kestrel","2":"Falconidae","3":"179.2","4":"111.2","5":"0.62"},{"1":"American Kestrel","2":"Falconidae","3":"175.3","4":"108.7","5":"0.62"},{"1":"American Robin","2":"Turdidae","3":"135.4","4":"84.1","5":"0.62"},{"1":"American Robin","2":"Turdidae","3":"120.8","4":"72.1","5":"0.60"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::

```{.r .cell-code}
# It works the same with character class columns
arrange(small_birds, desc(species))
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["family"],"name":[2],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["mass_g"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["mass_to_wing"],"name":[5],"type":["dbl"],"align":["right"]}],"data":[{"1":"American Robin","2":"Turdidae","3":"120.8","4":"72.1","5":"0.60"},{"1":"American Robin","2":"Turdidae","3":"135.4","4":"84.1","5":"0.62"},{"1":"American Kestrel","2":"Falconidae","3":"175.3","4":"108.7","5":"0.62"},{"1":"American Kestrel","2":"Falconidae","3":"179.2","4":"111.2","5":"0.62"},{"1":"American Crow","2":"Corvidae","3":"295.7","4":"455.7","5":"1.54"},{"1":"American Crow","2":"Corvidae","3":"279.7","4":"460.0","5":"1.64"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::

```{.r .cell-code}
# And you can sort by multiple columns, too.
# For example, you could sort alphabetically by species name first and then by largest to smallest wing length for each species.
arrange(small_birds, species, desc(wing_length_mm))
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["family"],"name":[2],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["mass_g"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["mass_to_wing"],"name":[5],"type":["dbl"],"align":["right"]}],"data":[{"1":"American Crow","2":"Corvidae","3":"295.7","4":"455.7","5":"1.54"},{"1":"American Crow","2":"Corvidae","3":"279.7","4":"460.0","5":"1.64"},{"1":"American Kestrel","2":"Falconidae","3":"179.2","4":"111.2","5":"0.62"},{"1":"American Kestrel","2":"Falconidae","3":"175.3","4":"108.7","5":"0.62"},{"1":"American Robin","2":"Turdidae","3":"135.4","4":"84.1","5":"0.62"},{"1":"American Robin","2":"Turdidae","3":"120.8","4":"72.1","5":"0.60"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::
:::


### `rename()`

A quick and easy way to rename your columns.


::: {.cell}

```{.r .cell-code}
# Here we'll temporarily rename our column names. It's temporary because we haven't overwritten our object with <-
# NOTE: In your arguments, new name must precede old name. It won't work if you put the old name before the new name.
rename(small_birds, wing = wing_length_mm, weight = mass_g)
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["family"],"name":[2],"type":["chr"],"align":["left"]},{"label":["wing"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["weight"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["mass_to_wing"],"name":[5],"type":["dbl"],"align":["right"]}],"data":[{"1":"American Kestrel","2":"Falconidae","3":"175.3","4":"108.7","5":"0.62","_rn_":"1"},{"1":"American Kestrel","2":"Falconidae","3":"179.2","4":"111.2","5":"0.62","_rn_":"2"},{"1":"American Crow","2":"Corvidae","3":"295.7","4":"455.7","5":"1.54","_rn_":"12"},{"1":"American Crow","2":"Corvidae","3":"279.7","4":"460.0","5":"1.64","_rn_":"13"},{"1":"American Robin","2":"Turdidae","3":"120.8","4":"72.1","5":"0.60","_rn_":"22"},{"1":"American Robin","2":"Turdidae","3":"135.4","4":"84.1","5":"0.62","_rn_":"23"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::
:::


### `mutate()`

A broad method to "create, modify, and delete columns". This is one of the most important functions in the `tidyverse`. When you start making permanent changes to your data, you'll likely use this one a lot.


::: {.cell}

```{.r .cell-code}
# You can use mutate to create new columns using existing columns.
# They'll get tacked on to the end of your data frame.
# Just create a new column name and set it equal to whatever you want. Here, we'll convert wing length in mm to cm.
mutate(small_birds, wing_cm = wing_length_mm / 10)
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["family"],"name":[2],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["mass_g"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["mass_to_wing"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["wing_cm"],"name":[6],"type":["dbl"],"align":["right"]}],"data":[{"1":"American Kestrel","2":"Falconidae","3":"175.3","4":"108.7","5":"0.62","6":"17.53","_rn_":"1"},{"1":"American Kestrel","2":"Falconidae","3":"179.2","4":"111.2","5":"0.62","6":"17.92","_rn_":"2"},{"1":"American Crow","2":"Corvidae","3":"295.7","4":"455.7","5":"1.54","6":"29.57","_rn_":"12"},{"1":"American Crow","2":"Corvidae","3":"279.7","4":"460.0","5":"1.64","6":"27.97","_rn_":"13"},{"1":"American Robin","2":"Turdidae","3":"120.8","4":"72.1","5":"0.60","6":"12.08","_rn_":"22"},{"1":"American Robin","2":"Turdidae","3":"135.4","4":"84.1","5":"0.62","6":"13.54","_rn_":"23"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::

```{.r .cell-code}
# Or you can alter existing columns by setting them equal to an altered version of themselves.
# Here, we'll set replace the family column by setting it equal to a capitalized version of itself.
mutate(small_birds, family = str_to_upper(family))
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["family"],"name":[2],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["mass_g"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["mass_to_wing"],"name":[5],"type":["dbl"],"align":["right"]}],"data":[{"1":"American Kestrel","2":"FALCONIDAE","3":"175.3","4":"108.7","5":"0.62","_rn_":"1"},{"1":"American Kestrel","2":"FALCONIDAE","3":"179.2","4":"111.2","5":"0.62","_rn_":"2"},{"1":"American Crow","2":"CORVIDAE","3":"295.7","4":"455.7","5":"1.54","_rn_":"12"},{"1":"American Crow","2":"CORVIDAE","3":"279.7","4":"460.0","5":"1.64","_rn_":"13"},{"1":"American Robin","2":"TURDIDAE","3":"120.8","4":"72.1","5":"0.60","_rn_":"22"},{"1":"American Robin","2":"TURDIDAE","3":"135.4","4":"84.1","5":"0.62","_rn_":"23"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::

```{.r .cell-code}
# NOTE: str_to_upper() is also part of the tidyverse! it comes from the stringr package.

# You can also remove columns by setting them equal to NULL, which has the same effect as using select(-column_name)
mutate(small_birds, family = NULL)
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["mass_g"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["mass_to_wing"],"name":[4],"type":["dbl"],"align":["right"]}],"data":[{"1":"American Kestrel","2":"175.3","3":"108.7","4":"0.62","_rn_":"1"},{"1":"American Kestrel","2":"179.2","3":"111.2","4":"0.62","_rn_":"2"},{"1":"American Crow","2":"295.7","3":"455.7","4":"1.54","_rn_":"12"},{"1":"American Crow","2":"279.7","3":"460.0","4":"1.64","_rn_":"13"},{"1":"American Robin","2":"120.8","3":"72.1","4":"0.60","_rn_":"22"},{"1":"American Robin","2":"135.4","3":"84.1","4":"0.62","_rn_":"23"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::
:::


### `separate()`

A common problem with untidy data is too many values per cell. `separate()` is one function to break down a very simple multi-value cell into its component parts, but there are many more options for more complex situations in `tidyr` to look into if needed. The inverse of `separate()` is `unite()`.


::: {.cell}

```{.r .cell-code}
# In this scenario, we want to break up the species column into its component parts assigning one word to each column. Conveniently for us, there are currently two words in each row of the "species" column, so this will be straightforward.
# Name the data you'd like to alter, specify the column (species), then name the new columns you'd like to separate species into, then define the separating character (there's a single space between American and next word, so that's a natural separator to pick).
separate(small_birds, species, into = c("Descriptor", "Bird"), sep = " ")
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["Descriptor"],"name":[1],"type":["chr"],"align":["left"]},{"label":["Bird"],"name":[2],"type":["chr"],"align":["left"]},{"label":["family"],"name":[3],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["mass_g"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["mass_to_wing"],"name":[6],"type":["dbl"],"align":["right"]}],"data":[{"1":"American","2":"Kestrel","3":"Falconidae","4":"175.3","5":"108.7","6":"0.62","_rn_":"1"},{"1":"American","2":"Kestrel","3":"Falconidae","4":"179.2","5":"111.2","6":"0.62","_rn_":"2"},{"1":"American","2":"Crow","3":"Corvidae","4":"295.7","5":"455.7","6":"1.54","_rn_":"12"},{"1":"American","2":"Crow","3":"Corvidae","4":"279.7","5":"460.0","6":"1.64","_rn_":"13"},{"1":"American","2":"Robin","3":"Turdidae","4":"120.8","5":"72.1","6":"0.60","_rn_":"22"},{"1":"American","2":"Robin","3":"Turdidae","4":"135.4","5":"84.1","6":"0.62","_rn_":"23"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::
:::


### `filter()` (lagniappe `str_detect()`)

This function is nearly identical to `subset()`, a base *R* function you saw in part I that allows you to reduce your data set down to just those rows meeting a specified criteria. The biggest difference is that `filter()`, as part of the `tidyverse`, can be strung together with other `tidyverse` functions. You'll see examples of this coming up in the pipes section.


::: {.cell}

```{.r .cell-code}
# filter() works the same way as subset().
# Here are some easy examples
filter(small_birds, species != "American Crow")
filter(small_birds, mass_g >= 110)
```
:::

::: {.cell}
::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["family"],"name":[2],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["mass_g"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["mass_to_wing"],"name":[5],"type":["dbl"],"align":["right"]}],"data":[{"1":"American Kestrel","2":"Falconidae","3":"175.3","4":"108.7","5":"0.62"},{"1":"American Kestrel","2":"Falconidae","3":"179.2","4":"111.2","5":"0.62"},{"1":"American Robin","2":"Turdidae","3":"120.8","4":"72.1","5":"0.60"},{"1":"American Robin","2":"Turdidae","3":"135.4","4":"84.1","5":"0.62"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["family"],"name":[2],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["mass_g"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["mass_to_wing"],"name":[5],"type":["dbl"],"align":["right"]}],"data":[{"1":"American Kestrel","2":"Falconidae","3":"179.2","4":"111.2","5":"0.62"},{"1":"American Crow","2":"Corvidae","3":"295.7","4":"455.7","5":"1.54"},{"1":"American Crow","2":"Corvidae","3":"279.7","4":"460.0","5":"1.64"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::
:::


Okay, here's a more complex example where we return only species names that contain the letters "ro" by combining `filter()` with `str_detect()`, another tidyverse function.


::: {.cell}

```{.r .cell-code}
# This should return Crows and Robins, but notice that the "r" is capitalized in Robin and not Crow.
# str_detect() is sensitive to capitalization, so before we search for the "ro" string, we can convert all letters in the species column to lowercase using str_to_lower(). 
filter(small_birds, str_detect(str_to_lower(species), "ro"))
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["family"],"name":[2],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["mass_g"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["mass_to_wing"],"name":[5],"type":["dbl"],"align":["right"]}],"data":[{"1":"American Crow","2":"Corvidae","3":"295.7","4":"455.7","5":"1.54"},{"1":"American Crow","2":"Corvidae","3":"279.7","4":"460.0","5":"1.64"},{"1":"American Robin","2":"Turdidae","3":"120.8","4":"72.1","5":"0.60"},{"1":"American Robin","2":"Turdidae","3":"135.4","4":"84.1","5":"0.62"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::
:::


### `count()`

A handy function for counting the number of observations (aka rows) for each group (aka levels of a categorical variable). For example, our data include two categorical variables: family and species. We can use `count()` to determine the number of rows per species and per family.

The benefits of this function are better highlighted with our full data set, so we'll use `birds` here instead of `small_birds`.


::: {.cell}

```{.r .cell-code}
# count() returns the number of rows in each level of a categorical variable.
# How many observations, or rows, are in each family?
count(birds, family)
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["family"],"name":[1],"type":["chr"],"align":["left"]},{"label":["n"],"name":[2],"type":["int"],"align":["right"]}],"data":[{"1":"Corvidae","2":"33"},{"1":"Falconidae","2":"11"},{"1":"Turdidae","2":"19"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::

```{.r .cell-code}
# How many observations do we have of each species?
# We can use the "sort" argument to arrange from most rows to fewest.
count(birds, species, sort = TRUE)
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["n"],"name":[2],"type":["int"],"align":["right"]}],"data":[{"1":"Blue Jay","2":"12"},{"1":"American Kestrel","2":"11"},{"1":"Fish Crow","2":"11"},{"1":"American Crow","2":"10"},{"1":"American Robin","2":"10"},{"1":"Eastern Bluebird","2":"9"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::

```{.r .cell-code}
# You can also count by multiple groups, e.g., how many rows are in each species in each family?
# In this case, it yields the same results as the previous code, though sorting it will split up families, so we'll avoid that here.
count(birds, family, species)
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["family"],"name":[1],"type":["chr"],"align":["left"]},{"label":["species"],"name":[2],"type":["chr"],"align":["left"]},{"label":["n"],"name":[3],"type":["int"],"align":["right"]}],"data":[{"1":"Corvidae","2":"American Crow","3":"10"},{"1":"Corvidae","2":"Blue Jay","3":"12"},{"1":"Corvidae","2":"Fish Crow","3":"11"},{"1":"Falconidae","2":"American Kestrel","3":"11"},{"1":"Turdidae","2":"American Robin","3":"10"},{"1":"Turdidae","2":"Eastern Bluebird","3":"9"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::
:::


### `summarize()`

This function can be used to generate multiple summary statistics at once. You can feed it any summary statistic and apply it to whichever column you want. There are even more advanced options to apply the summary statistics to multiple columns.

This can be useful at the level of the entire data set, but it's even more powerful when it's applied to grouped data! More on that soon. For now, test it out on the whole `birds` data set.


::: {.cell}

```{.r .cell-code}
# Create summary statistics with summarize()
# Here we're summarizing all species lumped together.
# You probably recognize most of these functions
  # n() gives your sample size
  # mean() gives your arithmetic mean
  # sd() gives standard deviation
  # min() gives the smallest value
  # max() gives the largest value
  # median() gives your sample size
summarize(birds,
          observations = n(),
          wing_average = mean(wing_length_mm),
          wing_sd = sd(wing_length_mm),
          mass_min = min(mass_g),
          mass_max = max(mass_g),
          mass_median = median(mass_g))
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["observations"],"name":[1],"type":["int"],"align":["right"]},{"label":["wing_average"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["wing_sd"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["mass_min"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["mass_max"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["mass_median"],"name":[6],"type":["dbl"],"align":["right"]}],"data":[{"1":"63","2":"186.6175","3":"75.87526","4":"25","5":"508.5","6":"106.4"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::

```{.r .cell-code}
# NOTE: You're not limited to these statistics! The help section for summarize() lists more, but you can also come up with your own!
```
:::


## Pipes (Are the Best)

### About

Now that you've familiarized yourself with some of the useful `tidyverse` functions, it's time to learn how to combine them! **Pipes** tell *R* to *"send the object or output preceding this pipe through to the next function."* The `tidyverse` packages use a special operator, **`%>%`**, to signify the pipe. When you see **`%>%`**, think of the things plumbers work with. Data flow through code pipes just like water flows through physical pipes -- from beginning to end.

Pipes have a number of major benefits:

1.  They allow you to send the output of one function directly into another function. This greatly streamlines your workflow by eliminating the need to constantly save and call objects. That means less typing and less code to read.
2.  Less saving and calling of objects means less room for error when referencing the nth iteration of an object you've altered.
3.  Pipes also allow you to combine functions in a fast, transparent way. This means you can run 10 or more functions at once without wrapping them inside a million parentheses. Instead, you can read them like normal text in most languages: from left to right and top to bottom.
4.  Lastly, pipes unlock useful analytic and processing tools like the ability to group and summarize data!

<br> In essence, pipes take you from this...

`object_2 -> f_1(object_1)` <br> `object_3 -> f_2(object_2)` <br> `object_4 -> f_3(object_3)` <br> `...` <br> `object_y -> f_x(object_x)`

<br>

To this...

![](Data_In/Figures/piping.png){width="500" height="30"}

### Practice

::: callout-tip
## <font size="5"> Try it </font>

<font size="4"> Practice using pipes to combine functions and process data. </font>
:::


::: {.cell}

```{.r .cell-code}
# Pipe Practice ----

# At it's most basic, you can pipe objects into a single function.
# Pipe small_birds into select() to reduce columns.
# NOTE: The %>% pipe tells R that small_birds is being fed into select(), therefore you don't have to put the object name inside the function parentheses. Now, all you need to put in the function are the arguments!
small_birds %>% 
  select(species, wing_length_mm)
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[2],"type":["dbl"],"align":["right"]}],"data":[{"1":"American Kestrel","2":"175.3","_rn_":"1"},{"1":"American Kestrel","2":"179.2","_rn_":"2"},{"1":"American Crow","2":"295.7","_rn_":"12"},{"1":"American Crow","2":"279.7","_rn_":"13"},{"1":"American Robin","2":"120.8","_rn_":"22"},{"1":"American Robin","2":"135.4","_rn_":"23"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::

```{.r .cell-code}
# Let's add to that to create a longer pipeline.
  # Pipe small_birds into select().
  # Next, pipe the output of select() into arrange().
  # Notice how the final output contains the effects of both functions.
small_birds %>% 
  select(species, wing_length_mm) %>%
  arrange(species, desc(wing_length_mm))
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[2],"type":["dbl"],"align":["right"]}],"data":[{"1":"American Crow","2":"295.7"},{"1":"American Crow","2":"279.7"},{"1":"American Kestrel","2":"179.2"},{"1":"American Kestrel","2":"175.3"},{"1":"American Robin","2":"135.4"},{"1":"American Robin","2":"120.8"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::

```{.r .cell-code}
# You can carry on this way for as long as you'd like.
  # Pipe small_birds into select()
  # Select the species and wing length columns, then pipe the output to arrange()
  # Sort by species and then descending wing length with arrange(), then pipe to mutate()
  # Use mutate() to add a column converting wing length to cm rounded to the nearest whole number, then pipe to filter()
  # Finally, filter out all individuals with wing length less than 14cm.
small_birds %>%
  select(species, wing_length_mm) %>%
  arrange(species, desc(wing_length_mm)) %>%
  mutate(wing_cm = round(wing_length_mm / 10)) %>%
  filter(wing_cm >= 14)
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["wing_cm"],"name":[3],"type":["dbl"],"align":["right"]}],"data":[{"1":"American Crow","2":"295.7","3":"30"},{"1":"American Crow","2":"279.7","3":"28"},{"1":"American Kestrel","2":"179.2","3":"18"},{"1":"American Kestrel","2":"175.3","3":"18"},{"1":"American Robin","2":"135.4","3":"14"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::
:::


::: callout-warning
## <font size="5"> Not Working? </font>

<font size="4"> Most errors and unexpected results when piping mean one of two things:

1.  You broke your pipeline by forgetting a pipe operator (`%>%`) somewhere in the middle after one of your functions.
2.  You put a pipe (`%>%`) at the end of the last function in your pipeline. Pipelines must end, so if you put `%>%` after your last function, *R* will keep searching and searching for more functions to run!

</font>
:::

### `case_match()`

Piping makes it easier to use more complex, but incredibly useful, functions. One example is `case_match()`. This function allows you to replace matching values in a column while leaving the rest unchanged.


::: {.cell}

```{.r .cell-code}
# Here we'll replace all common names with their scientific names.
# To do so, pipe your data into mutate()
# Mutate the species column by setting it equal to an altered version of itself in which you replace common names with scientific names using case_match().
small_birds %>% 
  mutate(species = case_match(species, 
                              "American Kestrel" ~ "F. sparverius",
                              "American Crow" ~ "C. brachyrhynchos",
                              "American Robin" ~ "T. migratorius"))
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["family"],"name":[2],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["mass_g"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["mass_to_wing"],"name":[5],"type":["dbl"],"align":["right"]}],"data":[{"1":"F. sparverius","2":"Falconidae","3":"175.3","4":"108.7","5":"0.62","_rn_":"1"},{"1":"F. sparverius","2":"Falconidae","3":"179.2","4":"111.2","5":"0.62","_rn_":"2"},{"1":"C. brachyrhynchos","2":"Corvidae","3":"295.7","4":"455.7","5":"1.54","_rn_":"12"},{"1":"C. brachyrhynchos","2":"Corvidae","3":"279.7","4":"460.0","5":"1.64","_rn_":"13"},{"1":"T. migratorius","2":"Turdidae","3":"120.8","4":"72.1","5":"0.60","_rn_":"22"},{"1":"T. migratorius","2":"Turdidae","3":"135.4","4":"84.1","5":"0.62","_rn_":"23"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::

```{.r .cell-code}
# If you don't want to replace all values, you can set non-target values to their existing, default value.
# You can replace multiple values with a single value, too.
small_birds %>%
  mutate(species = case_match(species,
                             c("American Kestrel", 
                               "American Robin") ~ "Not A Crow",
                             .default = species))
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["family"],"name":[2],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["mass_g"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["mass_to_wing"],"name":[5],"type":["dbl"],"align":["right"]}],"data":[{"1":"Not A Crow","2":"Falconidae","3":"175.3","4":"108.7","5":"0.62","_rn_":"1"},{"1":"Not A Crow","2":"Falconidae","3":"179.2","4":"111.2","5":"0.62","_rn_":"2"},{"1":"American Crow","2":"Corvidae","3":"295.7","4":"455.7","5":"1.54","_rn_":"12"},{"1":"American Crow","2":"Corvidae","3":"279.7","4":"460.0","5":"1.64","_rn_":"13"},{"1":"Not A Crow","2":"Turdidae","3":"120.8","4":"72.1","5":"0.60","_rn_":"22"},{"1":"Not A Crow","2":"Turdidae","3":"135.4","4":"84.1","5":"0.62","_rn_":"23"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::

```{.r .cell-code}
# It works with numbers, too!
small_birds %>%
  mutate(mass_to_wing = case_match(mass_to_wing,
                                   0.62 ~ 0.65,
                                   .default = mass_to_wing))
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["family"],"name":[2],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["mass_g"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["mass_to_wing"],"name":[5],"type":["dbl"],"align":["right"]}],"data":[{"1":"American Kestrel","2":"Falconidae","3":"175.3","4":"108.7","5":"0.65","_rn_":"1"},{"1":"American Kestrel","2":"Falconidae","3":"179.2","4":"111.2","5":"0.65","_rn_":"2"},{"1":"American Crow","2":"Corvidae","3":"295.7","4":"455.7","5":"1.54","_rn_":"12"},{"1":"American Crow","2":"Corvidae","3":"279.7","4":"460.0","5":"1.64","_rn_":"13"},{"1":"American Robin","2":"Turdidae","3":"120.8","4":"72.1","5":"0.60","_rn_":"22"},{"1":"American Robin","2":"Turdidae","3":"135.4","4":"84.1","5":"0.65","_rn_":"23"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::
:::


# **Grouping & Summarizing Data**

You've now had a brief introduction to grouped data with `count()`, and you've also seen how *R* can be used to summarize data. The `tidyverse` and its pipes make it easy to *summarize your data by group*. Think about this as calculating the same statistics on each level of a categorical variable rather than all of the levels lumped together. If we want to find average body sizes for every single species, we can do that all at once!

::: callout-tip
## <font size="5"> Try it </font>

<font size="4"> Practice grouping and summarizing our full data set by group. </font>
:::

## `group_by()`


::: {.cell}

```{.r .cell-code}
# Grouping & Summarizing ----

# We used count() above to determine the number of observations per family.
# In that instance, we basically defined species as a group! 
count(birds, family, species)

# group_by() makes defining a group more explicit.
# Here we pipe birds into group_by, which defines two groups: 1 = family, 2 = species
# Next, we pipe our grouped data into count(). Because it's already grouped, we don't need to type a group into count(). The %>% pipe tells count() to count number of rows based on our family and species groups.
birds %>%
  group_by(family, species) %>%
  count()

# NOTE: Same results!
# The only differences are that our output now tells us our groups, and tidyverse converts to a tibble, which is just a fancy data frame.
```
:::

::: {.cell}
::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["family"],"name":[1],"type":["chr"],"align":["left"]},{"label":["species"],"name":[2],"type":["chr"],"align":["left"]},{"label":["n"],"name":[3],"type":["int"],"align":["right"]}],"data":[{"1":"Corvidae","2":"American Crow","3":"10"},{"1":"Corvidae","2":"Blue Jay","3":"12"},{"1":"Corvidae","2":"Fish Crow","3":"11"},{"1":"Falconidae","2":"American Kestrel","3":"11"},{"1":"Turdidae","2":"American Robin","3":"10"},{"1":"Turdidae","2":"Eastern Bluebird","3":"9"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["family"],"name":[1],"type":["chr"],"align":["left"]},{"label":["species"],"name":[2],"type":["chr"],"align":["left"]},{"label":["n"],"name":[3],"type":["int"],"align":["right"]}],"data":[{"1":"Corvidae","2":"American Crow","3":"10"},{"1":"Corvidae","2":"Blue Jay","3":"12"},{"1":"Corvidae","2":"Fish Crow","3":"11"},{"1":"Falconidae","2":"American Kestrel","3":"11"},{"1":"Turdidae","2":"American Robin","3":"10"},{"1":"Turdidae","2":"Eastern Bluebird","3":"9"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::
:::


## `summarize()`

In the code chunk above, using `group_by()` was a bit less efficient than using `count()` on its own, but it highlighted how groups work in the `tidyverse`. They really shine when combined with `summarize()`. This function can pump out just about any statistic you can dream up for a group of data. Test it for yourself!


::: {.cell}

```{.r .cell-code}
# Use summarize to find the rounded means of each family
birds %>%
  group_by(family) %>%
  summarize(n = n(),
            avg_mass = round(mean(mass_g), 1),
            avg_wing = round(mean(wing_length_mm), 1),
            avg_mtow = round(mean(mass_to_wing), 1))
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["family"],"name":[1],"type":["chr"],"align":["left"]},{"label":["n"],"name":[2],"type":["int"],"align":["right"]},{"label":["avg_mass"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["avg_wing"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["avg_mtow"],"name":[5],"type":["dbl"],"align":["right"]}],"data":[{"1":"Corvidae","2":"33","3":"263.6","4":"229.4","5":"1.1"},{"1":"Falconidae","2":"11","3":"115.3","4":"184.4","5":"0.6"},{"1":"Turdidae","2":"19","3":"55.6","4":"113.5","5":"0.5"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::
:::


After summarizing your data, you can ungroup your output by piping it into `ungroup()`. Then, you can continue processing!


::: {.cell}

```{.r .cell-code}
# Now calculate means for each species.
# Pipe summary output into ungroup() to ungroup it.
# Pipe ungrouped output into arrange() to sort it by descending species mass within each family.
birds %>%
  group_by(family, species) %>%
  summarize(n = n(),
            avg_mass = round(mean(mass_g), 1),
            avg_wing = round(mean(wing_length_mm), 1),
            avg_mtow = round(mean(mass_to_wing), 1)) %>%
  ungroup() %>%
  arrange(family, desc(avg_mass))
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["family"],"name":[1],"type":["chr"],"align":["left"]},{"label":["species"],"name":[2],"type":["chr"],"align":["left"]},{"label":["n"],"name":[3],"type":["int"],"align":["right"]},{"label":["avg_mass"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["avg_wing"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["avg_mtow"],"name":[6],"type":["dbl"],"align":["right"]}],"data":[{"1":"Corvidae","2":"American Crow","3":"10","4":"441.3","5":"284.8","6":"1.5"},{"1":"Corvidae","2":"Fish Crow","3":"11","4":"292.9","5":"284.2","6":"1.0"},{"1":"Corvidae","2":"Blue Jay","3":"12","4":"88.8","5":"133.1","6":"0.7"},{"1":"Falconidae","2":"American Kestrel","3":"11","4":"115.3","5":"184.4","6":"0.6"},{"1":"Turdidae","2":"American Robin","3":"10","4":"81.3","5":"132.0","6":"0.6"},{"1":"Turdidae","2":"Eastern Bluebird","3":"9","4":"27.0","5":"93.0","6":"0.3"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::

```{.r .cell-code}
# We've now determined the number of observations for each family and each species using count(), but what if we want to see how many species are in each family?
# n_distinct() provides the number of different observations in each group while n() provides the number of all observations in each group.
birds %>% 
  group_by(family) %>% 
  summarize(n_species = n_distinct(species),
            n_individuals = n())
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["family"],"name":[1],"type":["chr"],"align":["left"]},{"label":["n_species"],"name":[2],"type":["int"],"align":["right"]},{"label":["n_individuals"],"name":[3],"type":["int"],"align":["right"]}],"data":[{"1":"Corvidae","2":"3","3":"33"},{"1":"Falconidae","2":"1","3":"11"},{"1":"Turdidae","2":"2","3":"19"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::
:::


# **Relational Joins**

Folks often work with more than one data set, and sometimes you'll need to combine information into a single data frame. But you usually want to combine based on a relationship between the two. Imagine you have the following data sets:

::: {#tbl-panel layout-ncol="2"}
| Patient   | Medication |
|-----------|------------|
| Patient 1 | Meds X     |
| Patient 2 | Meds Y     |
| Patient 3 | Meds Z     |

: Medication Treatment {#tbl-first}

| Patient     | Reaction  |
|-------------|-----------|
| *Patient 3* | *Died*    |
| Patient 2   | Recovered |
| Patient 1   | Recovered |

: Medication Response {#tbl-second}

Medication Trials
:::

If you weren't paying attention to the order of patients in each column, and just pasted the Reaction Column from the 2nd table onto the first, you'd get something like this.

| Patient   | Medication | Reaction  |
|-----------|------------|-----------|
| Patient 1 | Meds X     | *Died*    |
| Patient 2 | Meds Y     | Recovered |
| Patient 3 | Meds Z     | Recovered |

You might think Medication X was no good, when actually it was Medication Z used on Patient 3 that was an issue! Relational joins solve this issue and more by matching shared values in multiple data frames, then adding in the appropriate new data based on those matches.

There are several types of relational joins; we'll use `left_join()`, which adds data from *y* into *x* as new columns. Since *x* comes before *y*, *x* is on the left and *y* is on the right. Imagine our original data is in your left hand and our new data in your right; `left_join()` takes information from your right hand and adds it to the data in your left hand.

::: callout-tip
## <font size="5"> Try it </font>

<font size="4"> Download and import some new data, then use a relational join to combine it with our existing data. </font>
:::


::: {.cell}

```{.r .cell-code}
# Relational Joins ----

# Goal: Make a single column for scientific names by
  # 1. Downloading and importing a new data frame.
  # 2. Matching common names and adding in genus and species columns from the second data set.
  # 3. Uniting the genus and species columns into a single column.

# First, download the new data to your project directory. Remove the sub-folder if you don't have one, or edit it if it's different from mine.
download.file(url = "https://libguides.tulane.edu/ld.php?content_id=71057889", destfile = "Data_In/intro2_bird_names.csv")

# Next, import and examine the data
names <- read.csv("Data_In/intro2_bird_names.csv")

str(names)
```

::: {.cell-output .cell-output-stdout}
```
'data.frame':	12 obs. of  3 variables:
 $ common_name: chr  "American Robin" "Hermit Thrush" "Eastern Bluebird" "Peregrin Falcon" ...
 $ genus      : chr  "Turdus" "Catharus" "Sialia" "Falco" ...
 $ species    : chr  "migratorius" "guttatus" "sialis" "peregrinus" ...
```
:::
:::


Notice that our new data frame has fewer rows (only 10) and several species that aren't found in the data we've been working with. We want the information from the "genus" and "species" columns, but not all of it. We only want scientific names for the species in the data set we're working with. `left_join()` will bring in what matches and leave behind what doesn't.


::: {.cell}

```{.r .cell-code}
# The arguments left_join() must have are...
  # 1. x = left hand data
  # 2. y = right hand data to add in
  # 3. by = join_by(columns containing values to match by)
    # If matching columns have the same name, use that in join_by().
    # If they don't, use join_by(left_column = right_column)

# NOTE: All columns will be added from the new data frame unless otherwise specified. Are there any column naming conflicts?
colnames(birds)
colnames(names)
```
:::

::: {.cell ref-label='joins2'}

:::

::: {.cell}

```{.r .cell-code}
# Let's rename the species column in the birds object to avoid importing a second column named species.
# We can rename it "common_name" to match up with the same column in the names object.
birds <- birds %>%
  rename(common_name = species)

# To make the join permanent, create a new object equal to an altered version of birds with <- 
  # Pipe birds into the left_join function
  # The pipe tells left_join() that x = birds
  # supply y, in this case y = names object
  # join_by the column with the shared name
  # pipe the newly joined data into unite() to combine the genus and species columns
  # pipe into relocate() to rearrange the order of the columns.
birds_latin <- birds %>%
  left_join(names, by = join_by(common_name)) %>%
  unite(latin_name, c("genus", "species"), sep = " ") %>%
  relocate(common_name, latin_name)
```
:::


::: callout-note
## <font size="5"> Note </font>

<font size="4"> Because we created a new object above (`birds_latin`), if you're feeling fuzzy about any of the steps in the pipeline, you can examine output at each stage by running the original data through the pipeline in pieces, then adding one additional segment of the pipe at a time! E.g.,

`birds %>% left_join()`

`birds %>% left_join() %>% unite()`

`birds %>% left_join() %>% unite() %>% select()`

</font>
:::

# **If/Else Statements**

Often we find ourselves in a situation where we need to create a new column, or edit an existing column, to contain information that is based on values in another column. `if_else()` is a handy tool for doing just that.

It tells *R*,

> *if x meets my condition, then assign value y, otherwise (aka else), assign value z.*

It's main arguments are

1.  Input data
2.  Condition (`==`, `!=`, `</>`, `<=/>=`, `%in%`, etc.)
3.  Value to assign if condition = TRUE
4.  Value to assign if condition = FALSE

::: callout-tip
## <font size="5"> Try it </font>

<font size="4"> Use `if_else()` to fill new columns in `small_birds` with values based on conditions in another column. **Save these changes for use in the next section** </font>
:::


::: {.cell}

```{.r .cell-code}
# If_else ----

# Here's a straightforward case where we want to create a habitat column and assign the appropriate habitat type to each bird species. Crows and Robins live in the same habitat, and kestrels live in a different habitat, so there are only two possible conditions/choices: if and else.

# Overwrite small birds with an altered version of itself using <-
# Pipe small_birds into mutate()
# Use mutate() to create a new column called habitat.
# If species == "American Kestrel", then habitat == "grasslands.
# Otherwise, habitat == "open woodlands".
# Pipe output into relocate() to improve visibility of changes.
small_birds <- small_birds %>%
  mutate(habitat = if_else(species == "American Kestrel", "grasslands", "open woodlands")) %>%
  relocate(species, habitat)

small_birds
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["habitat"],"name":[2],"type":["chr"],"align":["left"]},{"label":["family"],"name":[3],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["mass_g"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["mass_to_wing"],"name":[6],"type":["dbl"],"align":["right"]}],"data":[{"1":"American Kestrel","2":"grasslands","3":"Falconidae","4":"175.3","5":"108.7","6":"0.62","_rn_":"1"},{"1":"American Kestrel","2":"grasslands","3":"Falconidae","4":"179.2","5":"111.2","6":"0.62","_rn_":"2"},{"1":"American Crow","2":"open woodlands","3":"Corvidae","4":"295.7","5":"455.7","6":"1.54","_rn_":"12"},{"1":"American Crow","2":"open woodlands","3":"Corvidae","4":"279.7","5":"460.0","6":"1.64","_rn_":"13"},{"1":"American Robin","2":"open woodlands","3":"Turdidae","4":"120.8","5":"72.1","6":"0.60","_rn_":"22"},{"1":"American Robin","2":"open woodlands","3":"Turdidae","4":"135.4","5":"84.1","6":"0.62","_rn_":"23"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::

```{.r .cell-code}
# Here's the same code but flipping the if and else to highlight how you can use %in% to match multiple values at once.
# It says if the value in the species column is in this vector, then assign "open woodlands", otherwise assign "grasslands"
small_birds %>%
  mutate(habitat =
           if_else(species %in% c("American Robin", "American Crow"), "open woodlands", "grasslands")) %>%
  relocate(species, habitat)
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["habitat"],"name":[2],"type":["chr"],"align":["left"]},{"label":["family"],"name":[3],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["mass_g"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["mass_to_wing"],"name":[6],"type":["dbl"],"align":["right"]}],"data":[{"1":"American Kestrel","2":"grasslands","3":"Falconidae","4":"175.3","5":"108.7","6":"0.62","_rn_":"1"},{"1":"American Kestrel","2":"grasslands","3":"Falconidae","4":"179.2","5":"111.2","6":"0.62","_rn_":"2"},{"1":"American Crow","2":"open woodlands","3":"Corvidae","4":"295.7","5":"455.7","6":"1.54","_rn_":"12"},{"1":"American Crow","2":"open woodlands","3":"Corvidae","4":"279.7","5":"460.0","6":"1.64","_rn_":"13"},{"1":"American Robin","2":"open woodlands","3":"Turdidae","4":"120.8","5":"72.1","6":"0.60","_rn_":"22"},{"1":"American Robin","2":"open woodlands","3":"Turdidae","4":"135.4","5":"84.1","6":"0.62","_rn_":"23"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::
:::


What do you do if you have more than two possible conditions or outcomes, i.e., you have an "if" and multiple "elses"? One way to deal with that is by using nested `if_else()` statements.

In essence...

> *1. If x meets my first condition,* <br> 2. then assign value y. <br> 3. Otherwise, if x meets my second condition, <br> 4. then assign value z. <br> 5. Otherwise x meets neither condition 1 nor 2, so assign value a.

In practice, this works by setting our first "else" or "otherwise" value to a second `if_else()`.


::: {.cell}

```{.r .cell-code}
# Each of these three species has a different diet, so there is more than one "else" option. The pipeline is the same as above, but we replace our first "else" value with another if_else()

# Save these changes with <-
# If species == Kestrel, then diet == "Small Animals", 
# otherwise if species == Crow, then diet == "Omnivore",
# otherwise, diet == "Insects & berries".
small_birds <- small_birds %>%
  mutate(diet = 
           if_else(species == "American Kestrel", "small animals",
                  ifelse(species == "American Crow", "omnivore", "insects & berries"))) %>%
  relocate(species, habitat, diet)

small_birds
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["habitat"],"name":[2],"type":["chr"],"align":["left"]},{"label":["diet"],"name":[3],"type":["chr"],"align":["left"]},{"label":["family"],"name":[4],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["mass_g"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["mass_to_wing"],"name":[7],"type":["dbl"],"align":["right"]}],"data":[{"1":"American Kestrel","2":"grasslands","3":"small animals","4":"Falconidae","5":"175.3","6":"108.7","7":"0.62","_rn_":"1"},{"1":"American Kestrel","2":"grasslands","3":"small animals","4":"Falconidae","5":"179.2","6":"111.2","7":"0.62","_rn_":"2"},{"1":"American Crow","2":"open woodlands","3":"omnivore","4":"Corvidae","5":"295.7","6":"455.7","7":"1.54","_rn_":"12"},{"1":"American Crow","2":"open woodlands","3":"omnivore","4":"Corvidae","5":"279.7","6":"460.0","7":"1.64","_rn_":"13"},{"1":"American Robin","2":"open woodlands","3":"insects & berries","4":"Turdidae","5":"120.8","6":"72.1","7":"0.60","_rn_":"22"},{"1":"American Robin","2":"open woodlands","3":"insects & berries","4":"Turdidae","5":"135.4","6":"84.1","7":"0.62","_rn_":"23"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::
:::


# **Iteration**

## Background

**Iteration** in coding is the act of running multiple inputs through the same process in order to avoid duplication of code and effort (and thereby reducing bugs). It's a huge topic -- so huge, in fact, that it merits a guide of its own...more than we can cover here. Instead, we focus on two goals for this section:

1.  Making sure you know iterative tools exist.
2.  Providing the briefest of introductions to those tools so you know which avenues to explore to learn more.

There are three common ways to approach iteration in *R*, ordered below from oldest and most work-intensive to newest and least work-intensive.

1.  Loops in base *R* (for, while, and repeat)
2.  Functions that contain pre-packaged for loops
    i)  The `apply()` family in base *R*
    ii) `tidyverse` functions like `across()` and `map()` from the `dplyr` and `purrr` packages respectively.

**For Loops** are the most common of the original loops in base *R*. They can be a bit to wrap your head around and require a lot of typing, which means they provide more room for user error. They do, however, fulfill their job of processing multiple inputs at a time. For loops loop all the way through all your input data; the other loops only loop through your inputs while a certain condition is true, or they loop over and over again until they reach a defined condition.

The **`apply()`** family in base *R* and the **`map()`** family from the `tidyverse` are very similar. They both save you from having to write loops, they take similar inputs and provide similar outputs, and there are multiple versions of each type. Though it tends to be a little more abstract because more happens behind the scenes, the `map()` family offers three main improvements over the `apply()` family:

1.  Naming conventions in the `map()` family are more intuitive.
2.  `map()` allows for more shortcuts (less typing).
3.  Since it's part of the tidyverse, the `map()` family can be worked into pipelines.

Which of these tools you end up using may change on a case-by-case basis and tends to be a matter of personal preference. They all achieve the same end result, but it's important to know a bit about each to understand additional guidance you come across when delving deeper into your own analyses.

::: callout-tip
## <font size="5"> Try it </font>

<font size="4"> Explore an example of duplication (bad) and learn how to fix it with each of the three types of iterative tools (good).</font>
:::

## Duplication

First, the duplication. Notice that we've got inconsistent capitalization in the character class columns of `small_birds`.


::: {.cell}

```{.r .cell-code}
small_birds
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["habitat"],"name":[2],"type":["chr"],"align":["left"]},{"label":["diet"],"name":[3],"type":["chr"],"align":["left"]},{"label":["family"],"name":[4],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["mass_g"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["mass_to_wing"],"name":[7],"type":["dbl"],"align":["right"]}],"data":[{"1":"American Kestrel","2":"grasslands","3":"small animals","4":"Falconidae","5":"175.3","6":"108.7","7":"0.62","_rn_":"1"},{"1":"American Kestrel","2":"grasslands","3":"small animals","4":"Falconidae","5":"179.2","6":"111.2","7":"0.62","_rn_":"2"},{"1":"American Crow","2":"open woodlands","3":"omnivore","4":"Corvidae","5":"295.7","6":"455.7","7":"1.54","_rn_":"12"},{"1":"American Crow","2":"open woodlands","3":"omnivore","4":"Corvidae","5":"279.7","6":"460.0","7":"1.64","_rn_":"13"},{"1":"American Robin","2":"open woodlands","3":"insects & berries","4":"Turdidae","5":"120.8","6":"72.1","7":"0.60","_rn_":"22"},{"1":"American Robin","2":"open woodlands","3":"insects & berries","4":"Turdidae","5":"135.4","6":"84.1","7":"0.62","_rn_":"23"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::
:::


If we wanted to clean that up for future publication, we could use `str_to_sentence()` from the `tidyverse` to capitalize only the first letter of each cell. We're only dealing with a handful of columns, so we could use the duplication approach and copy-paste it, changing column names:


::: {.cell}

```{.r .cell-code}
# Look at all the duplication work here. It's a lot of copy-pasting, a lot of overwriting, and a lot of object names to change.
# That's a lot of room for human error.

# To start off, create a new object to edit called small_birds_dup (for duplication)
# Then overwrite each column with an altered version of itself.
small_birds_dup <- small_birds
small_birds_dup$species <- str_to_sentence(small_birds_dup$species)
small_birds_dup$habitat <- str_to_sentence(small_birds_dup$habitat)
small_birds_dup$diet <- str_to_sentence(small_birds_dup$diet)
small_birds_dup$family <- str_to_sentence(small_birds_dup$family)
```
:::


Or we could duplicate effort with `mutate()`:


::: {.cell}

```{.r .cell-code}
# This approach somewhat reduces the amount of typing and overwriting we're doing, so there's a little bit less room for error.
small_birds %>%
  mutate(species = str_to_sentence(species)) %>%
  mutate(habitat = str_to_sentence(habitat)) %>%
  mutate(diet = str_to_sentence(diet)) %>%
  mutate(family = str_to_sentence(family))
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["habitat"],"name":[2],"type":["chr"],"align":["left"]},{"label":["diet"],"name":[3],"type":["chr"],"align":["left"]},{"label":["family"],"name":[4],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["mass_g"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["mass_to_wing"],"name":[7],"type":["dbl"],"align":["right"]}],"data":[{"1":"American kestrel","2":"Grasslands","3":"Small animals","4":"Falconidae","5":"175.3","6":"108.7","7":"0.62","_rn_":"1"},{"1":"American kestrel","2":"Grasslands","3":"Small animals","4":"Falconidae","5":"179.2","6":"111.2","7":"0.62","_rn_":"2"},{"1":"American crow","2":"Open woodlands","3":"Omnivore","4":"Corvidae","5":"295.7","6":"455.7","7":"1.54","_rn_":"12"},{"1":"American crow","2":"Open woodlands","3":"Omnivore","4":"Corvidae","5":"279.7","6":"460.0","7":"1.64","_rn_":"13"},{"1":"American robin","2":"Open woodlands","3":"Insects & berries","4":"Turdidae","5":"120.8","6":"72.1","7":"0.60","_rn_":"22"},{"1":"American robin","2":"Open woodlands","3":"Insects & berries","4":"Turdidae","5":"135.4","6":"84.1","7":"0.62","_rn_":"23"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::
:::


## For Loops

Using a for loop, we only have to call `str_to_sentence()` once...but there's more work required to put together the for loop itself. This would be less of a sacrifice if you were altering 10 or 20 columns at once, but it's good practice!

A basic for loop consists of the following parts:

1.  Defined input parameters, meaning what objects the for loop will process and for how many iterations.
2.  An open curly bracket to contain the for loop.
3.  Any conditions you'd like to set for skipping objects or breaking/exiting the loop. (Omit if none).
4.  The actual instruction to skip (`next`) or exit (`break`) if needed.
5.  The process or function(s) you want to run
6.  A closed curly bracket to contain the for loop.


::: {.cell}

```{.r .cell-code}
# The first thing we need to do is define a new object that we can edit. Let's call it small_birds_for
small_birds_for <- small_birds

# Next, you should familiarize yourself with a couple pieces of every for loop.

# seq_along() will tell you the number of columns of your object and provide an index number for each one. e.g., there are 7 columns numbered 1:7
seq_along(small_birds_for)
```

::: {.cell-output .cell-output-stdout}
```
[1] 1 2 3 4 5 6 7
```
:::

```{.r .cell-code}
# double brackets after a data frame reference a column by its index number or order.
# E.g., column 3 of small_birds_for is diet.
small_birds_for[[3]]
```

::: {.cell-output .cell-output-stdout}
```
[1] "small animals"     "small animals"     "omnivore"         
[4] "omnivore"          "insects & berries" "insects & berries"
```
:::
:::

::: {.cell}

```{.r .cell-code}
# Now for the for loop itself.

# Here's what it looks like altogether. I'll insert comments in a blown out version below.
for (i in seq_along(small_birds_for)) {
  if (is.numeric(small_birds_for[[i]])){
    next
    }
  small_birds_for[[i]] <- str_to_sentence(small_birds_for[[i]])
}

for (i in seq_along(small_birds_for)) {
# First line sets the for loop and input parameters (objects and number of iterations)
  # i is a real number starting at 1 and increasing by 1 until it reaches the end value.
  # You could replace it with (i in 1:10), for example, and it would climb from 1:10.
  # Here it ends at 7 because small_birds_for has 7 columns.
# End with an open curly bracket.

  if (is.numeric(small_birds_for[[i]])){
# The next row sets a condition. We can't capitalize numbers, so we want to prevent this loop from running on our columns containing numeric data, like mass_g.
# This will check if a given column, identified by its index number, is numeric.
  # Instructions for if() are contained in curly brackets also, so open one here.
    
    next
# Provide instructions to if().
  # Our instructions are simple: if the condition is met, then skip that column with "next"

    }
# Conditional instructions complete. Close if() with another curly bracket.

  small_birds_for[[i]] <- str_to_sentence(small_birds_for[[i]])
# Replace each column not skipped with an altered version of itself where 1st letter is capitalized.

}
# Close the curly brackets to denote the end of the for loop code.


# Success? Call the object to check your work
small_birds_for
```
:::

::: {.cell}
::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["habitat"],"name":[2],"type":["chr"],"align":["left"]},{"label":["diet"],"name":[3],"type":["chr"],"align":["left"]},{"label":["family"],"name":[4],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["mass_g"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["mass_to_wing"],"name":[7],"type":["dbl"],"align":["right"]}],"data":[{"1":"American kestrel","2":"Grasslands","3":"Small animals","4":"Falconidae","5":"175.3","6":"108.7","7":"0.62","_rn_":"1"},{"1":"American kestrel","2":"Grasslands","3":"Small animals","4":"Falconidae","5":"179.2","6":"111.2","7":"0.62","_rn_":"2"},{"1":"American crow","2":"Open woodlands","3":"Omnivore","4":"Corvidae","5":"295.7","6":"455.7","7":"1.54","_rn_":"12"},{"1":"American crow","2":"Open woodlands","3":"Omnivore","4":"Corvidae","5":"279.7","6":"460.0","7":"1.64","_rn_":"13"},{"1":"American robin","2":"Open woodlands","3":"Insects & berries","4":"Turdidae","5":"120.8","6":"72.1","7":"0.60","_rn_":"22"},{"1":"American robin","2":"Open woodlands","3":"Insects & berries","4":"Turdidae","5":"135.4","6":"84.1","7":"0.62","_rn_":"23"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::
:::


## `apply()` family

This group of functions tackles iteration by applying other functions to your inputs, essentially running a for loop behind the scenes. Its a lot less work on your part, but it still has a number of components to wrap your head around. Variations include `apply()`, `lapply()`, `sapply()`, `vapply()` and more. They differ mostly in their accepted input and output types. Run `?lapply` in your Console for more info.

At their most basic, the only arguments these functions need are an input and a function. Sadly, none of the options return output as data frames by default, so you have to put the pieces back together with `as.data.frame()`. They also don't handle conditions very well, so if you don't want to process every column in a data frame, you'll have to create a function. For example...


::: {.cell}

```{.r .cell-code}
# Here we use lapply which returns a list object (which can be a list of vectors, columns, data frames, other lists, etc.).
# It will pull our data frame apart, so we wrap it in as.data.frame to convert the output from a list back into a data frame after the iteration.
# We only want to edit character columns, so we need to create a function  using a different type of if else statement that says:
  # "If x is a character, capitalize the first letter of every value, otherwise, return x (which should be numeric or anything but character class)
# You can run this without as.data.frame() to examine the list output.
as.data.frame(lapply(small_birds, function(x) {if(is.character(x)) str_to_sentence(x) else x }))
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["habitat"],"name":[2],"type":["chr"],"align":["left"]},{"label":["diet"],"name":[3],"type":["chr"],"align":["left"]},{"label":["family"],"name":[4],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["mass_g"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["mass_to_wing"],"name":[7],"type":["dbl"],"align":["right"]}],"data":[{"1":"American kestrel","2":"Grasslands","3":"Small animals","4":"Falconidae","5":"175.3","6":"108.7","7":"0.62"},{"1":"American kestrel","2":"Grasslands","3":"Small animals","4":"Falconidae","5":"179.2","6":"111.2","7":"0.62"},{"1":"American crow","2":"Open woodlands","3":"Omnivore","4":"Corvidae","5":"295.7","6":"455.7","7":"1.54"},{"1":"American crow","2":"Open woodlands","3":"Omnivore","4":"Corvidae","5":"279.7","6":"460.0","7":"1.64"},{"1":"American robin","2":"Open woodlands","3":"Insects & berries","4":"Turdidae","5":"120.8","6":"72.1","7":"0.60"},{"1":"American robin","2":"Open woodlands","3":"Insects & berries","4":"Turdidae","5":"135.4","6":"84.1","7":"0.62"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::
:::


## `map()` family and `across()`

The most complicated parts of tackling iteration with the `purrr` package are figuring out which functions to use and what the shorthand means. A quick look in the help files or at the [cheat sheet](https://posit.co/resources/cheatsheets/?type=posit-cheatsheets&_page=2/) solves both questions. For example, there's `map()` for working with single lists, `map_if()` for working with lists and conditions, `map2()` for two lists, `pmap()` for many lists, `map_chr()` for character vector output, `map_dfc()` and `map_dfr()` for data frame output, `modify()` for same output type as input type, `modify_if()` for same output type as input type with conditions, and more. It's a huge list! We're going to use 'modify_if()' to process only character columns and return a data frame.

As for the shorthand, it's all about reducing how much you need to type. You still need to provide input and a function, but you can use shortcuts for both of those, which are highlighted below.

Look at how simple this is. It achieves the same thing as our big, complicated for loop:


::: {.cell}

```{.r .cell-code}
# Pipe your input into modify_if
# Supply your condition. If it's a function that yields TRUE/FALSE without needing any other arguments, just put the function name. Here, we use is.character.
# Supply your function. If your function doesn't need any arguments, again, you just need the function name.
small_birds %>%
  modify_if(is.character, str_to_sentence)
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["habitat"],"name":[2],"type":["chr"],"align":["left"]},{"label":["diet"],"name":[3],"type":["chr"],"align":["left"]},{"label":["family"],"name":[4],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["mass_g"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["mass_to_wing"],"name":[7],"type":["dbl"],"align":["right"]}],"data":[{"1":"American kestrel","2":"Grasslands","3":"Small animals","4":"Falconidae","5":"175.3","6":"108.7","7":"0.62"},{"1":"American kestrel","2":"Grasslands","3":"Small animals","4":"Falconidae","5":"179.2","6":"111.2","7":"0.62"},{"1":"American crow","2":"Open woodlands","3":"Omnivore","4":"Corvidae","5":"295.7","6":"455.7","7":"1.54"},{"1":"American crow","2":"Open woodlands","3":"Omnivore","4":"Corvidae","5":"279.7","6":"460.0","7":"1.64"},{"1":"American robin","2":"Open woodlands","3":"Insects & berries","4":"Turdidae","5":"120.8","6":"72.1","7":"0.60"},{"1":"American robin","2":"Open woodlands","3":"Insects & berries","4":"Turdidae","5":"135.4","6":"84.1","7":"0.62"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::
:::


In more complicated cases where you may need arguments in your functions, you'll have to provide an input before the arguments, but you can use a simple `.` to stand in for your input rather than typing it all out. In these instances, you might also have to define your function. With `lapply()`, we did that with `function(x)`. You can use the same thing here or a number of other options shown below, all of which yield the same result.


::: {.cell}

```{.r .cell-code}
# purrr package shorthand options shown from longest to fastest short cut to lay out their evolution. They all yield the same results.

small_birds %>%
  modify_if(is.character, function(x) str_to_sentence(x))

small_birds %>%
  modify_if(is.character, \(x) str_to_sentence(x))

small_birds %>%
  modify_if(is.character, \(.) str_to_sentence(.))

small_birds %>%
  modify_if(is.character, ~ str_to_sentence(.))
# This last option is generally regarded as the preferred notation.
```
:::


Lastly, there's one more option from the `tidyverse`'s `dplyr` package that you may come across. It's called `across()`. It uses the `purrr` shorthand to denote functions and inputs and can be paired with `mutate()` and a `where()` that sets the condition.


::: {.cell}

```{.r .cell-code}
# Basically, "mutate across all columns where the column is character class by applying the str_to_sentence() function."
small_birds %>%
  mutate(across(where(is.character), ~ str_to_sentence(.)))
```

::: {.cell-output-display}

`````{=html}
<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["species"],"name":[1],"type":["chr"],"align":["left"]},{"label":["habitat"],"name":[2],"type":["chr"],"align":["left"]},{"label":["diet"],"name":[3],"type":["chr"],"align":["left"]},{"label":["family"],"name":[4],"type":["chr"],"align":["left"]},{"label":["wing_length_mm"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["mass_g"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["mass_to_wing"],"name":[7],"type":["dbl"],"align":["right"]}],"data":[{"1":"American kestrel","2":"Grasslands","3":"Small animals","4":"Falconidae","5":"175.3","6":"108.7","7":"0.62","_rn_":"1"},{"1":"American kestrel","2":"Grasslands","3":"Small animals","4":"Falconidae","5":"179.2","6":"111.2","7":"0.62","_rn_":"2"},{"1":"American crow","2":"Open woodlands","3":"Omnivore","4":"Corvidae","5":"295.7","6":"455.7","7":"1.54","_rn_":"12"},{"1":"American crow","2":"Open woodlands","3":"Omnivore","4":"Corvidae","5":"279.7","6":"460.0","7":"1.64","_rn_":"13"},{"1":"American robin","2":"Open woodlands","3":"Insects & berries","4":"Turdidae","5":"120.8","6":"72.1","7":"0.60","_rn_":"22"},{"1":"American robin","2":"Open woodlands","3":"Insects & berries","4":"Turdidae","5":"135.4","6":"84.1","7":"0.62","_rn_":"23"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
`````

:::
:::


## To learn more...

::: callout-note
## <font size="5"> Note </font>

<font size="4"> To learn more about iterative tools, and *R* in general, see Dr. Hadley Wickham's excellent guide, [*R for Data Science*](https://r4ds.had.co.nz/index.html). Wickham is author of the `tidyverse` and the chief scientist behind *RStudio*! </font>
:::

# **Analyses (but not really)**

*R* can run basically any analysis you can think of, and someone, somewhere, has probably already bundled up that analysis into a neat little function. (But if not, you can write your own function!) Since the field of statistics is immense and every project calls for an individualized approach, we won't dive too deeply into analyses here. This guide is about teaching you the coding basics that you'll need on *every* project, not unique projects.

Just know that your there's a package out there for you, probably even several. For example, there are stand-alone packages for generalized linear mixed models, for checking model assumptions, for machine learning, Bayesian statistics, for entire fields, like package `vegan` for community ecologists, and on and on and on.

...And then there's this:
`library(help = "stats")`

Base *R* ships with a huge number of statistical functions to run in the `stats` package. It covers the basics like Chi-squared tests, t-tests, ANOVAs, linear models, rank-sum, heatmaps and so on. A lot of them are quick and easy once you've got your data cleaned. Here's a super fast example of a t-test -- so fast it only takes 3 lines of code:

**Are Kestrels significantly heavier on average than Robins?**


::: {.cell}

```{.r .cell-code}
# Filter out kestrels
kestrel <- filter(birds, common_name == "American Kestrel")

# Filter out robins
robin <- filter(birds, common_name == "American Robin")

# t-test on their mass columns
t.test(kestrel$mass_g, robin$mass_g)
```

::: {.cell-output .cell-output-stdout}
```

	Welch Two Sample t-test

data:  kestrel$mass_g and robin$mass_g
t = 10.656, df = 15.113, p-value = 1.998e-08
alternative hypothesis: true difference in means is not equal to 0
95 percent confidence interval:
 27.21723 40.81732
sample estimates:
mean of x mean of y 
 115.3273   81.3100 
```
:::
:::


**They sure are!**

# **Producing Figures in R**

Like analyses, plots in *R* are really only limited by your imagination...and how well you handle making lots of minute tweaks over and over again until you get a result that makes you happy. The trickiest thing about them is that they have lots of component parts to fiddle with -- more than most people will ever learn. The nice thing is that you don't have to learn all the ins and outs of plotting in R, just how to use your resources:

-   [`ggplot2` Data Visualization Cheatsheet from Posit/RStudio](https://posit.co/wp-content/uploads/2022/10/data-visualization-1.pdf)
-   [*R for Data Science*](https://r4ds.had.co.nz/index.html) contains a whole chapter on how to use `ggplot2` written by the package's lead author.
-   If you like that chapter, you'll be happy to learn they followed it up with a whole book: [*ggplot2: Elegant Graphics for Data Analysis*](https://ggplot2-book.org/).
-   [The R Graph Gallery](https://r-graph-gallery.com/index.html) provides a huge number of example plots of various types and provides the code for how to make them. All you have to do is adapt the code to your own data.
-   [Stackoverflow.com](https://stackoverflow.com) for all your coding questions.

The links I provide above are all for making plots in `ggplot2`, part of the `tidyverse`. While you can make plots in base *R*, it tends to be a headache-inducing experience. Your options are limited, aesthetics aren't great, different plots use different language, and things often don't work as expected. The end result is that most folks prefer plotting in `ggplot2`, and it's a good place to start learning.

Again, we won't delve too deeply into the specifics because the possibilities are endless, but below are a few quick examples using our bird data.

All figures made in `ggplot2` use the same grammar structure:

-   First, always start the plot with `ggplot()` and supply it with some data. Then add (+) from there.
-   Aesthetics, `aes()`, are provided either in `ggplot()` or in the geometry. They tell the plot which data to put on which axis and how to define groups.
-   Geometry tells `ggplot()` which type of plot to make, e.g., `geom_point()`, `geom_bar()`, `geom_boxplot()`, `geom_errorbar()` etc.
-   Themes supply `ggplot()` with a pre-packaged format, e.g., `theme_classic()`, `theme_dark()`, `theme_minimal()`.
-   A separate `theme()` function can be used to manually modify basically any aspect of the figure. Search for it in the help section to learn about its many arguments.
-   Titles and axis labels with various functions like `ggtitle()`, `xlab()`, and more.
-   `scale_...()` functions handle additional aesthetic tweaks and allow you to adjust color palettes using separate packages.
-   Facets allow you to combine multiple plots into a single figure with `facet_wrap()`and `facet_grid()`.
-   There are probably more, but this should cover most cases!

::: callout-tip
## <font size="5"> Try it </font>

<font size="4"> Try your hand at making some plots. Tweak values here and there to see how it affects your output. </font>
:::

## Barplot with error bars

Since `ggplot2` is part of the `tidyverse`, we can pipe our data right into it and even alter it in the pipeline before plotting. This first example uses `group_by()` and `summarize()` to calculate means, then it `ungroup()`s our data, re-orders the categories with `fct_reorder()` for ordered plotting, and pipes the altered data into a plot.

The code for the plot is quite long, but don't let it intimidate you, it's just a bunch of little, stand-alone pieces strung together with (+) I'll provide it all together and then blown out with comments.


::: {.cell}

```{.r .cell-code}
birds %>%
  group_by(family, common_name) %>%
  summarize(mean.wing = mean(wing_length_mm),
            sd.wing = sd(wing_length_mm)) %>%
  ungroup() %>%
  mutate(common_name = 
           fct_reorder(common_name, desc(mean.wing))) %>%
  ggplot(aes(x = common_name,
                y = mean.wing,
                fill = family)) +
  geom_bar(stat = "identity",
           alpha = 0.7) +
  geom_errorbar(aes(x = common_name,
                    ymin = mean.wing - sd.wing,
                    ymax = mean.wing + sd.wing),
                width = 0.2, 
                color = "black",
                alpha = 0.9,
                linewidth = 0.7) +
  theme_bw() +
  theme(axis.text.x = element_text(angle = 30, 
                                   hjust = 1),
        plot.title = element_text(size=14)) +
  xlab("Species") +
  ylab("Mean wing length (mm)") +
  ggtitle("Figure 1. Mean wing length of some birds")


# And now annotated:

# Supply data
birds %>%
  # Group it by family and common name
  group_by(family, common_name) %>%
  # Summarize it to get means (bars) and sds(error bars)
  summarize(mean.wing = mean(wing_length_mm),
            sd.wing = sd(wing_length_mm)) %>%
  # Un-group to continue altering
  ungroup() %>%
  # Change the order to plot by descending order
  mutate(common_name = 
           fct_reorder(common_name, desc(mean.wing))) %>%
  # Pipe into ggplot and set x axis, y axis, and fill color of each bar
  ggplot(aes(x = common_name,
                y = mean.wing,
                fill = family)) +
  # Tell ggplot we want a bar chart.
  # stat="identity" says we'll supply a value, no need to count.
  geom_bar(stat = "identity",
           # alpha alters transparency. 0 = clear, 1 = opaque.
           alpha = 0.7) +
  # Provide error bar geometry and aesthetics
  # ymin/ymax are length of error bars above and below the bar itself
  geom_errorbar(aes(x = common_name,
                    ymin = mean.wing - sd.wing,
                    ymax = mean.wing + sd.wing),
                # more self-explanatory aesthetics
                width = 0.2, 
                color = "black",
                alpha = 0.9,
                size = 0.7) +
  # Provide a theme
  theme_bw() +
  # Alter a couple individual elements of the theme
  # This first one changes how x axis text appears
  theme(axis.text.x = element_text(angle = 30, 
                                   hjust = 1),
        # Change size of plot title
        plot.title = element_text(size=14)) +
  # x axis label
  xlab("Species") +
  # y axis label
  ylab("Mean wing length (mm)") +
  # plot title.
  ggtitle("Figure 1. Mean wing length of some birds")
```
:::

::: {.cell}
::: {.cell-output-display}
![](R_intro_part2_files/figure-html/barplot2-1.png){width=672}
:::
:::


## Violin-wrapped box plot

You may have heard that statisticians are pushing to phase out box plots in favor of violin plots because box plots hide important patterns like the difference between normal and biomodal distributions. Here's a plot that combines the best of both worlds and adds our data ponits on top with a few comments inserted in between. It combines three geometries: box plots, violin plots, and point plots. They don't need `aes()` aesthetics because already provide them to `ggplot()`, and they're the same for each geometry.

Remember to tweak values in here and run the code a few times over to see how your plot changes!


::: {.cell}

```{.r .cell-code}
birds %>%
  mutate(common_name = 
           fct_reorder(common_name, wing_length_mm)) %>%
  ggplot(aes(x = common_name,
             y = wing_length_mm,
             fill = family)) +
  geom_violin(width = 0.6,
                # alpha = 0 means complete transparency to prevent the violins from taking on a colored fill.
                alpha = 0) +
  geom_point(size = 2, 
               alpha = 0.6,
               aes(color = family),
               # This tells the points to offset a bit for visibility
               position = position_jitterdodge(dodge.width = 0.9,
                                      jitter.width = 0.1)) +
  geom_boxplot(width=0.2, 
                 color="black", 
                 # black boxplot lines with low transparency
                 alpha=0.1) +
  theme_classic() +
  # You can locate the legend inside the figure with 0-1 coordinates for left/right and top/bottom.
  theme(legend.position = c(0.15, 0.8),
          plot.title = element_text(size=16)) +
    ggtitle("Figure 2. A violin-wrapped boxplot of bird species wing length") +
    xlab("Species") + 
    ylab("Wing Length (mm)") + 
    theme(axis.text.x = element_text(angle = 30, hjust = 1))
```

::: {.cell-output-display}
![](R_intro_part2_files/figure-html/violin-plot-1.png){width=672}
:::
:::


## Histogram

A huge number of *R* packages have been developed to build off of or integrate with `ggplot2`. Some provide even fancier or more specific graph types, others provide palettes or other aesthetics. Below, we install and load a package that's designed for making color-blind-friendly plots.

Rather than cluttering up a single figure and making it unreadable. This figure also takes advantage of `facet_wrap()` to create small sub-plot histograms for each species.


::: {.cell}

```{.r .cell-code}
# Install and load viridis for color-blind-friendly plots. Look back at the 2nd section of this document if you've forgotten how to install packages.
library(viridis)

birds %>%
  mutate(common_name = 
           fct_reorder(common_name, wing_length_mm)) %>%
  # I didn't like that "family" was lower case in the figure, so I renamed and capitalized it. Remember to use the new name when defining aes().
  rename(Family = family) %>%
  ggplot(aes(x = wing_length_mm,
             color = Family,
             fill = Family)) +
  geom_histogram(alpha = 0.6,
                 binwidth = 5) +
  # create one small plot per species in the figure
  facet_wrap(~common_name) + 
  # setting histogram bars to use color-blind-friendly colors
  scale_fill_viridis(discrete = TRUE) +
  # sets histogram lines to color-blind-friendly
  scale_color_viridis(discrete = TRUE) +
  theme_classic() +
  theme(legend.position = "right",
        # defining spacing between facets
        panel.spacing = unit(0.4, "lines"),
        # and size of facet grid subtitle text
        strip.text.x = element_text(size = 8),
        plot.title = element_text(size = 14)) +
  xlab("Wing Length (mm)") +
  ylab("Number of Individuals") +
  ggtitle("Figure 3. Faceted histogram of wing length")
```

::: {.cell-output-display}
![](R_intro_part2_files/figure-html/histogram-1.png){width=672}
:::
:::


## Saving/Exporting

If you want to use your figures in documents produced outside of *R*, you'll probably want to save and export them. Plots can be saved in objects in *R* just like data frames. Let's copy-paste the histogram plot above and save it as an object called "histobirds". Then we'll call `ggsave()` to save the object.

By default `ggsave()` will save the plot whose code you most recently ran *unless* you explicitly supply the argument `plot = object_name`, in this case, "histobirds". PDF output will generally give you the highest quality results, but use PNG if you need to save to Microsoft Word or PowerPoint or something along those lines. Useful arguments include width, height, units, dpi among others.


::: {.cell}

```{.r .cell-code}
# Save the plot
histobirds <- birds %>%
  mutate(common_name = 
           fct_reorder(common_name, wing_length_mm)) %>%
  # I didn't like that "family" was lower case in the figure, so I renamed and capitalized it. Remember to use the new name when defining aes().
  rename(Family = family) %>%
  ggplot(aes(x = wing_length_mm,
             color = Family,
             fill = Family)) +
  geom_histogram(alpha = 0.6,
                 binwidth = 5) +
  # create one small plot per species in the figure
  facet_wrap(~common_name) + 
  # setting histogram bars to use color-blind-friendly colors
  scale_fill_viridis(discrete = TRUE) +
  # sets histogram lines to color-blind-friendly
  scale_color_viridis(discrete = TRUE) +
  theme_classic() +
  theme(legend.position = "right",
        # defining spacing between facets
        panel.spacing = unit(0.4, "lines"),
        # and size of facet grid subtitle text
        strip.text.x = element_text(size = 8),
        plot.title = element_text(size = 14)) +
  xlab("Wing Length (mm)") +
  ylab("Number of Individuals") +
  ggtitle("Figure 3. Faceted histogram of wing length")


# And now export it with ggsave()
# I have a sub-folder in my project called Plots that I'd like to save to. Change/delete it if you've got something different.
ggsave(plot = histobirds, "Plots/histobirds.pdf", width = 6, height = 4, units = "in")
```
:::


The End :)
