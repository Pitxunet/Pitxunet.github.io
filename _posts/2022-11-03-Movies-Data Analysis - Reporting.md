---
layout: post
title: "Trends in TMDb Movie sample"
subtitle: "I used Python pandas to gather, wrangle and explore the data set"
background: '\img\posts\Tmdb\themoviedb.jpg'
---
*Image from the Movie database*

# TMDb Movies Data Analysis
by Estitxu Larralde

      

## Table of Contents
<ul>
<li><a href="#intro">Introduction</a></li>
<li><a href="#wrangling">Data Wrangling</a></li>
<li><a href="#eda">Exploratory Data Analysis</a></li>
<li><a href="#conclusions">Conclusions</a></li>
</ul>

<a id='intro'></a>
## Introduction

### Dataset Description 


In this Notebook I'll be analyzing data regarding 10,000 movies. This data has been collected from The Movie Database.

The columns names of the dataset and their significance are the following:

id   : general id of the movie                  
imdb_id  : id assigned at imdb               
popularity : different from rating, it depends on votes and views of the day, release date, previous days score. It fluctuates frequently             
budget   : for producing the movie               
revenue: revenue of the movie                 
original_title: title of the movie          
cast :  main roles                  
homepage                
director                
tagline                 
keywords: several keywords related to the movie                
overview: sum-up of the movie's plot               
runtime : duration in minutes                 
genres                  
production_companies    
release_date: year, month and day of release            
vote_count: total vote count in TMDb            
vote_average: the mean of the votes received by the movie            
release_year: year of release          
budget_adj: updated budget in terms of 2010 dollars, accounting for inflation    
revenue_adj: updated in terms of 2010 dollars, accounting for inflation

### Questions for Analysis

I asked myself:

- Is there a correlation between good-ratings and other variables?
- Are some production companies better than others producing well rated movies?
- Does the size of the budget matter to get good ratings?
- Are sequels' ratings high as compared to the original movie?



<a id='wrangling'></a>
## Data Wrangling


### General Properties

In this section I loaded the dataset. I then explored the first few rows (df.head), the format of the variables (df.info), the shape of the dataframe (df.shape) and the descriptive statistics of the numerical variables (df.describe)


### Data Cleaning

In this part of the analysis, I continued to inspect the data, to find issues to be solved to make it more reliable and easier to analyse.


#### a) Dropping irrelevant columns

I decided that some of the columns were unnecessary to answer the questions of the investigation. Also, the release year  was already part of the 'release_date' column.

As for columns 'budget' and 'revenue', I decided to keep the updated variables 'budget_adj' and 'revenue_adj' to compare figures more accurately taking inflation into account.

I dropped all the irrelevant columns to simplify the dataset and the analysis.
 

#### b) Replacing special characters

Certain columns contained values separated by pipe characters. I replaced them by commas.


#### c) Figures in scientific notation

As it's not easy to interpret dollar figures in scientific notation, I converted "budget" and "revenue" columns into float formatting


#### d) Null values

I checked and found over 1000 null values in the "production_companies" column. Before dropping all the rows containing null values in that column, I compared the distribution of the dataset to the distribution of the rows containing null values on the "production_companies" column.

The distribution was very similar. The only difference was that in the null values dataset movies had in average less than 50 votes. I decided to go ahead and drop all rows containing null values.

  
#### e) Duplicated values

I found only one duplicated row in the dataset. I dropped the row. 


#### f) Data format

I changed "release_date" datatype to datetime as it had the wrong data format.


#### g) Incoherent values

After running the descriptive statistics on the dataframe, I realized that minimum value of 'budget_adj' column was very low. I sorted the values of the column to check if there are more outliers or values that seem too low for the production of a film.

The first 30 ascending values in column 'budget_adj' seemed too low for the production of a movie. Nevertheless, and as I didn't know which amount is the minimum reasonable for the production of a movie, I decided to keep in mind this limitation of the dataset when interpreting the results.



<a id='eda'></a>
## Exploratory Data Analysis

I proceeded to explore the data to find the answer to my questions at the beginning of the project

#### Are there some companies more successful than others producing well rated movies?

First, I tried to compare the number of production companies in the dataset to the number of companies of the movies with best ratings (75th - 100th percentile, i.e. over 6.7 points). This didn't shred any light onto the question as the number of unique companies in the best rated movies was roughly 30% of the total number of unique production companies of the dataset (729 vs 2905 companies).

Then I checked the distribution of the production companies within that group.


    
![png](\img\posts\Tmdb\output_21_0.png)
    


As per our dataset, it seems Paramount Pictures, Universal Pictures, Coumbia Pictures and Warner Bros produced some of the best rated (top 25%) movies. Nevertheless, this could be either because of their experience in the industry or because they produce many more movies than other Production companies.

We can see also that Walt Disney is not in the top three but it has coproduced some of the films and produces them under different names: Walt Disney Pictures, Walt Disney Productions, Walt Disney Feature Animation.

Production Companies producing more well rated movies by TMDb users are also producing many more movies than other Production Companies (in average each company - or compound of companies- produced 1.27 movies) 

In the pie chart below we can see the ten production companies that produced more movies each. These are all big names of the cinema industry.


    
![png](\img\posts\Tmdb\output_25_0.png)
    


#### Are the most expensive movies also the most popular/best rated movies?

Now I would like to see if there is any correlation between the rating of a movie ('vote_average') and the budget used for producing it:


    
![png](\img\posts\Tmdb\output_28_0.png)
    


There is some positive correlation, but not very strong. There are a lot of movies with a small budget that got a good 'vote_average' in TMDb.

#### Is the rating of sequels better than the average rating? What's the distribution of the popularity of movies considered as sequels ?

I personally find most sequels worse than the original title so I would like to see how many of the movies considered as sequels (53) are rated better than the mean value of 'vote_average' (6.182) and how many worse:


    
![png](\img\posts\Tmdb\output_32_0.png)
    


#### Is there a correlation between budget and profit? 

We've seen that there is no a strong correlation between 'budget' and 'vote_average'. Nevertheless, I'd like to see whether films that were more expensive to produce were also more profitable. 

    
![png](\img\posts\Tmdb\output_35_0.png)
    

There is a positive correlation which means that as the budget increases the profit sometimes does too. Nevertheless, as per the scatter plot we can see it's not very strong.


#### Is there a correlation between profit and good ratings in TMDb?

What about the correlation between 'profit' and 'vote_average'? Let's see if profitable films got a higher 'vote_average' than those that got a smaller profit.

    
![png](\img\posts\Tmdb\output_39_0.png)
    

The correlation is clearly positive between both variables. It means that profitable films got overall better ratings than films that were less profitable.



<a id='conclusions'></a>
## Conclusions


I focused in the ratings of the films of the dataset and their relationship with other variables. 

First, I tried to check if some production companies were better at producing well rated movies. There seem to be some companies that got many more well rated movies than the average of the other production companies. Nevertheless, the analysis has some limitations. Same companies that have produced more well rated movies have also produced many more films, regardless the rating, than the rest of the production companies. This limitation could be overcome by working with proportions. The second limitation is that many companies produce movies together with other production companies. Therefore the number of movies each company has produced is more dificult to determine. 

Second, I wanted to find out if there was some correlation between the most expensive movies and good ratings, i.e.: whether most expensive movies were getting also the highest ratings. Looking to the scatter plot of the two variables, there seems to be some positive correlation but it isn't very strong. There are slightly more films with a big budget that got also better ratings. Nevertheless, there are many films with a budget on the small side that got rated really well. 

Third, I was curious about the rating ('vote_average') of movies classed as sequels. I wanted to know it their rating was over or below the mean of 'vote_average'. I found 53 sequel movies as per the variable 'keyword'. The majority of them (33 out of 53) got a rating below the mean of 'vote_average'. As per this dataset, sequel movies tend to be worse rated overall than movies that aren't a sequel.

Fourth, I calculated the profit of each movie by substracting 'budget_adj' from 'revenue_adj'. I wanted to see whether movies with an important budget are more profitable than those with a small budget. There is no clear correlation between both variables according to the scatter plot I produced: small budget films are profitable more or less as often as big budget films of this dataset. It would be interesting to find out whether there are additional revenues and profit (not only those related to selling cinema tickets) that expensive movies have access to and other movies don't.

Finally, I tried to see if there is any correlation between profitable films and good ratings. As per the scatter plot on variables 'profit' and 'vote_average' it seems to be a positive correlation, i.e. films that are more profitable seem to be also better rated than the average.

An important limitation of the dataset is the number of missing values. There are more than 1000 missing values in the column 'production_companies'. It should be possible to look up the movies concerned by the missing values and find out the name of the production company. Nevertheless, due to the limited time to produce the report I abandoned the idea. I dropped all the null values instead. As a result the dataset was much smaller than the original one.

Another limitation is the one noticed in the column 'budget_adj'. Even after dropping all values equal to zero, there are some very low values in the column. Double checking budget_adj values or dropping rows that don't meet a minimum value could be solutions to improve the quality of the dataset.


Ps: to see the code I used, check my Jupyter Notebook <a href="https://github.com/Pitxunet/TMDb-movie-data/blob/main/TMDb%20Movies%20Data%20Analysis.ipynb">TMDb Movies Data Analysis</a> in project's GitHub repository

