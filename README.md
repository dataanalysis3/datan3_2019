## Data analysis in social science 3, University of Exeter (2019)

### Module outline

#### Classes

- Thursday, 2.30-4.30pm, Clayden Computational Lab (except 28 February)
- The 28 February class has been moved to Friday 1 March (10.30am-12.30pm, WSL 220)

#### Lecturer

- Dr Alexey Bessudnov (a.bessudnov [at] exeter.ac.uk)
- Peer-to-peer teaching assistant: Max Shilling (mas254 [at] exeter.ac.uk)

#### Office hours

- Location: Clayden 1.05
- Tuesday, 11am-12pm
- Friday, 4-5pm

#### Aims of the module

This is a fourth module in the data analysis in social science series. In the Introduction to Social Data you learned the basics of descriptive statistics and R. Data Analysis 1 introduced you to statistical inference. Data Analysis 2 covered linear regression analysis. In Data Analysis 3 we are not going to learn new statistical techniques, but will focus on how to apply the techniques you already know to the analysis of real-life data sets and how to produce good statistical reports.

This is a skill that you may need in a variety of jobs where data analytic expertise is required, such as market analysis, policy analysis in various fields, web analytics, data journalism, academic research, etc.

You already know how to use R to describe data and estimate simple statistical models. However, real-life data rarely come in the form of a perfectly formatted csv file ready for the analysis. The real life data sets often need to be reshaped, merged, recoded, aggregated and modified in various ways before you can even start your analysis. Unless you know how to do this you will not be able to conduct independent statistical analysis.

In this module we will use data from the Understanding Society, a large household panel study conducted in the UK. We will work with longitudinal data, which introduces a number of technical challenges.

We will use R, and you are expected to know the basics of data analysis in R already. The pre-requisites for this module are POL/SOC1041 and POL/SOC2077.

The only way to learn data analysis is to do data analysis. I will not be able to teach you this, but I can guide your independent learning. I use the "flipped classroom" model of teaching. This means that you are expected to read and master the required material BEFORE you come to class and we will often use class time to do exercises and check solutions rather than introduce new material.

#### Software

All classes will be held in a computer lab, but I encourage you to bring and use your own laptops. The difference is that you have full administrative rights on your laptops and it will be easier for you to install software and use version control.

You will need to install the following software.

- [R](https://www.r-project.org/) (please update your distribution if you have got it already installed)
- [R Studio](https://www.rstudio.com/)
- [Git](https://git-scm.com/)
- [LaTeX](https://www.latex-project.org/)

All this software is free. 

#### Attendance

This is a technical module, and it will require effort and time commitment from you. As with other technical skills, missing some initial bits means that you may not be able to catch up. Attendance in this module is crucial. If you do not attend classes you will not be able to do well. If you know that you cannot work hard in this optional module I ask you to change it and choose another module instead.

#### Assessment

The assessment for this module is a statistical report of 2,000 words (50% of your mark) and five short statistical assignments (10% each).

Statistical assignments will usually be programming exercises. You will need to complete them using R and Github and submit using Github Classroom so that each assignment is a separate repository. You will also need to submit links to your repositories via eBart. I will explain the procedure in more detail in class, and we will have a test (not graded) submission to make sure you understand how it works.

I will release problems for assignments one week before each deadline. Assignments must be your individual work and you are not allowed to work on them together with other students.

The deadlines for statistical assignments are as following.

- Test submission: 29 January, 2pm (not part of your mark)
- Assignment 1: 5 February, 2pm
- Assignment 2: 19 February, 2pm
- Assignment 3: 5 March, 2pm
- Assignment 4: 12 March, 2pm
- Assignment 5: 26 March, 2pm

Please see the list of Github repositories with the assignments here: https://github.com/dataanalysis3/datan3_2019/blob/master/assignments.md

The marking criteria for statistical assignments are correctness of your code and of substantive interpretations (where applicable).

The 3-week turnaround rule applies to statistical assignments, but my goal is to mark them and give you feedback within one week from submission.

For the final statistical report you will conduct independent analysis of the Understanding Society data and produce a report of 2,000 words describing the results of your analysis.

The deadline for the final statistical report is 9 April, 2pm. I will publish more details describing your task at least one month before the submission deadline (i.e. no later than 9 March). You will receive your marks and feedback by 3 May.

The marking criteria for statistical reports are the following: originality of approach, complexity of analysis, correctness of code, correctness of interpretations, knowledge of background literature, style and accuracy.

Late submissions up to two weeks after the deadline will be capped at 40\%. Submissions that are late for more than two weeks will not be accepted.

#### Homework

I assign homework for each class and you need to complete it before coming to class. Please see details here: https://github.com/dataanalysis3/datan3_2019/blob/master/homework.md

#### Syllabus plan 

The plan below is flexible and I may change some topics and dates as we proceed.

- 17 January. Introduction to the module.
- 24 January. Data analysis workflow. Reading data into R.
- 31 January. Reproducible research. R Markdown.
- 7 February. Data transformation.
- 14 February. Relational data.
- 21 February. Iteration.
- 1 March. Functions.
- 7 March. Reshaping and cleaning longitudinal data.
- 14 March. Data visualisation (1).
- 21 March. Data visualisation (2).
- 28 March. Longitudinal modelling.

#### Reading list

The module has a website: <http://abessudnov.net/dataanalysis3/>. Please note that the website accompanied the module as delivered in 2018, and in 2019 we will do some things differently.

The main text for this module is Grolemund and Wickham's *R for Data Science*.

- G.Grolemund & H.Wickham. R for Data Science. Freely available at <http://r4ds.had.co.nz/>.

Solutions for exercises in R for Data Science:

- J.B.Arnold. [R for Data Science: Exercise Solutions](https://jrnold.github.io/r4ds-exercise-solutions/)

For details on how to use R Markdown see:

- Y.Xie, J.J.Allaire, G.Grolemund. (2018). [R Markdown: The Definitive Guide](https://bookdown.org/yihui/rmarkdown/).

The guide on using Git and Github with R Studio:

- J.Bryant et al. [Happy Git and GitHub for the useR](https://happygitwithr.com/).

Data visualisation.

- H.Wickham. (2009). ggplot2: Elegant graphics for data analysis. N.Y.: Springer. [Available in the university library as an ebook.]
- W.Chang. (2013). R Graphics Cookbook. O'Reilly. [Available in the university library as an ebook.]
- K.Healy. (2018). Data Visualization: A Practical Introduction. Princeton University Press. This is a new book and I just ordered it for the library. You can access the draft of the book here: https://socviz.co/  and the code for the graphs is available here: https://github.com/kjhealy/dataviz .
- The BBC Visual and Data Journalism cookbook for R Graphics: https://bbc.github.io/rcookbook/

R Priogramming.

- R.Peng. (2016). R Programming for Data Science. https://bookdown.org/rdpeng/rprogdatascience/

- H.Wickham. (2014). Advanced R. http://adv-r.had.co.nz/

Machine learning.

- G.James et al. (2013, 8th printing 2017). An Introduction to Statistical Learning with Applications in R. Springer. Freely available at http://www-bcf.usc.edu/~gareth/ISL/
- The caret package. Online textbook: http://topepo.github.io/caret/index.html


There are many other resources that can help you with R. [DataCamp](https://www.datacamp.com/) is an online learning platform that covers most topics in this module. Also see a list of other resources here <https://www.tidyverse.org/learn/>.

Full documentation for the *Understanding Society* is available at <https://www.understandingsociety.ac.uk/>.

