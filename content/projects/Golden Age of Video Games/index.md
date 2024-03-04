---
title: The Golden Age of Video Games Using Spreadsheets, SQL and Tableau by Andrew Kim
date: 2023-11-10T18:08:42-04:00
draft: true
description: data analytics, Spreadsheets, SQL, Tableau
weight: 1
slug: cyclistic-case-study
categories:
  - Data Analytics
tags:
  - Data Analytics
  - Case Study
  - SQL
  - Spreadsheets
  - Tableau
  - GitHub
  - Data Visualization
  - Dashboard
  - Google Workspace
series:
  - Projects
  - Data Analytics
cover: 
  image: "banner.png"
  alt: "The Golden Age of Video Games Using Spreadsheets, SQL and Tableau by Andrew Kim"
hideMeta: true
---

In this case study, I analyze historical data from a Chicago based bike-share 
company in order to identify trends in how their customers use bikes differently. 
The main tools I use are spreadsheets, SQL and Tableau. Here are the highlights:  


* [Tableau Dashboard: Bikeshare in Chicago](https://public.tableau.com/app/profile/andrewdeekim/viz/BikeshareinChicago/BikeshareinChicago"target="_blank")

* [GitHub: Bike Share in Chicago Repository](https://github.com/andrewdeekim/bike-share-in-chicago)

* [Slides: Navigating Speedy Success](https://docs.google.com/presentation/d/1eFg7z36HifSdGsDJuQC-b0w9eSGLmz2mKFWGHc-mPAM/edit?usp=share_link)


A more in-depth breakdown of the case study scenario is included below, followed 
by my full report.  
<br>


### Scenario
Cyclistic is a bike-share company based in Chicago with two types of customers. 
Customers who purchase single-ride or full-day passes are known as **casual riders**, 
while those who purchase annual memberships are known as **members**. Cyclistic’s 
financial analysts have concluded that annual members are much more profitable 
than casual riders. The director of marketing believes the company’s future 
success depends on maximizing the number of annual memberships.  

The marketing analytics team wants to understand how casual riders and annual 
members use Cyclistic bikes differently. From these insights, the team will 
design a new marketing strategy to convert casual riders into annual members. 
The primary stakeholders for this project include Cyclistic's director of 
marketing and the Cyclistic executive team. The Cyclistic marketing analytics 
team are secondary stakeholders. 

<br>

***

## Defining the problem  
The main problem for the director of marketing and marketing analytics team is 
this: Design marketing strategies aimed at converting Cyclistic’s casual riders 
into annual members. There are three questions that will guide this future 
marketing program. For my scope on this project, I will anlyze the first 
question:  

*1) How do annual members and casual riders use Cyclistic bikes differently?*  
2) Why would casual riders buy Cyclistic annual memberships?  
3) How can Cyclistic use digital media to influence casual riders to become 
members?  

By looking at the data, we will be able to first get a broad sense of certain 
patterns that are occurring in the two different groups. Understanding the 
differences will provide more accurate customer profiles for each group. These 
insights will help the marketing analytics team design high quality targeted 
marketing for converting casual riders into members. For the Cyclistic executive 
team, these insights will help Cyclistic maximize the number of annual members 
and will fuel future growth for the company.  

## Business task  
> ### Analyze historical bike trip data to identify trends in how annual members and casual riders use Cyclistic bikes differently. 


<br>


***

## Data sources

We’ll be using Cyclistic’s historical bike trip data from the last 12 months, 
which is publicly available [here](https://divvy-tripdata.s3.amazonaws.com/index.html). 
The data is made available by Motivate International Inc. under this [license](https://www.divvybikes.com/data-license-agreement). The data is stored 
in spreadsheets. There are 12 .CSV files total:  

    01) 2021-02_divvy_trip-data.csv  
    02) 2021-03_divvy_trip-data.csv  
    03) 2021-04_divvy_trip-data.csv  
    04) 2021-05_divvy_trip-data.csv  
    05) 2021-06_divvy_trip-data.csv  
    06) 2021-07_divvy_trip-data.csv  
    07) 2021-08_divvy_trip-data.csv  
    08) 2021-09_divvy_trip-data.csv  
    09) 2021-10_divvy_trip-data.csv  
    10) 2021-11_divvy_trip-data.csv  
    11) 2021-12_divvy_trip-data.csv  
    12) 2022-01_divvy_trip-data.csv  


It is structured data, organized in rows (records) and columns (fields). Each 
record represents one trip, and each trip has a unique field that identifies it: 
`ride_id`. Each trip is anonymized and includes the following fields:  

    * ride_id               #Ride id - unique
    * rideable_type         #Bike type - Classic, Docked, Electric
    * started_at            #Trip start day and time
    * ended_at              #Trip end day and time
    * start_station_name    #Trip start station
    * start_station_id      #Trip start station id
    * end_station_name      #Trip end station
    * end_station_id        #Trip end station id
    * start_lat             #Trip start latitude  
    * start_lng             #Trip start longitute   
    * end_lat               #Trip end latitude  
    * end_lat               #Trip end longitute   
    * member_casual         #Rider type - Member or Casual  

Bike station data that is made publicly available by the <a href="https://www.chicago.gov/city/en/narr/foia/data_disclaimer.html" target="_blank">city of Chicago</a> 
will also be used. It can be downloaded <a href="https://data.cityofchicago.org/Transportation/Divvy-Bicycle-Stations/bbyy-e7gq/data" target="_blank">here</a>. In terms of bias and credibility, both data sources we are using ROCCC:

* **Reliable and original:** this is public data that contains accurate, 
complete and unbiased info on Cyclistic’s historical bike trips. It can be used 
to explore how different customer types are using Cyclistic bikes.  

* **Comprehensive and current:** these sources contain all the data needed to 
understand the different ways members and casual riders use Cyclistic bikes. The 
data is from the past 12 months. It is current and relevant to the task at hand. 
This is important because the usefulness of data decreases as time passes.  

* **Cited:**these sources are publicly available data provided by Cyclistic and 
the City of Chicago. Governmental agency data and vetted public data are 
typically good sources of data.  

<br>


***

## Data cleaning and manipulation    

### Microsoft Excel: initial data cleaning and manipulation  
Our next step is making sure the data is stored appropriately and prepared for 
analysis. After downloading all 12 zip files and unzipping them, I housed the 
files in a temporary folder on my desktop. I also created subfolders for the 
.CSV files and the .XLS files so that I have a copy of the original data. 
Then, I launched Excel, opened each file, and chose to Save As an Excel Workbook 
file. For each .XLS file, I did the following:  

* Changed format of `started_at` and `ended_at` columns  
  + Formatted as custom `DATETIME`  
  + Format > Cells > Custom > yyyy-mm-dd h:mm:ss  
  
* Created a column called `ride_length`  
  + Calculated the length of each ride by subtracting the column `started_at` 
  from the column `ended_at` (example: `=D2-C2`)  
  + Formatted as `TIME`  
  + Format > Cells > Time > HH:MM:SS (37:30:55)  
  
* Created a column called `ride_date`  
  + Calculated the date of each ride started using the `DATE` command 
  (example: `=DATE(YEAR(C2),MONTH(C2),DAY(C2))`)  
  + Format > Cells > Date > YYYY-MM-DD  
  
* Created a column called `ride_month`  
  + Entered the month of each ride and formatted as number (example: January: `=1`)  
  + Format > Cells > Number  
  
* Created a column called `ride_year`  
  + Entered the year of each ride and formatted as general  
  + Format > Cells > General > YYYY  
  
* Created a column called `start_time`  
  + Calculated the start time of each ride using the `started_at` column
  + Formatted as `TIME`  
  + Format > Cells > Time > HH:MM:SS (37:30:55)  
  
* Created a column called `end_time`  
  + Calculated the end time of each ride using the `ended_at` column  
  + Formatted as `TIME`  
  + Format > Cells > Time > HH:MM:SS (37:30:55)  
  
* Created a column called `day_of_week`  
  + Calculated the day of the week that each ride started using the `WEEKDAY` 
  command (example: `=WEEKDAY(C2,1)`)  
  + Formatted as a `NUMBER` with no decimals  
  + Format > Cells > Number (no decimals) > 1,2,3,4,5,6,7  
  + Note: 1 = Sunday and 7 = Saturday  


After making these updates, I saved each .XLS file as a new .CSV file.  

### BigQuery: further data cleaning and manipulation via SQL

Since these datasets are so large, it makes sense to move our analysis to a tool 
that is better suited for handling large datasets. I chose to use SQL via [BigQuery](https://cloud.google.com/bigquery).  

In order to continue processing the data in BigQuery, I created a bucket in 
Google Cloud Storage to upload all 12 files. I then created a project in 
BigQuery and uploaded these files as datasets. I've provided my initial cleaning 
and transformation SQL queries here for reference: <a href="https://github.com/joeypetosa/cyclistic_cs/blob/3331e54abf16dbdb379bd71366bc27185ecd0006/initial_setup_query.sql" target="_blank">initial_setup_query.sql</a>  

The results from the `COUNT DISTINCT` query for each table are very interesting. 
We can see that the three summer months have the highest trip counts, followed 
by alternating spring and fall months before ending with winter months: 

<img align=center height=400px src="https://s3.us-west-1.amazonaws.com/joeypetosa.com/portfolio/cyclistic-cs/ccs-monthly-trip-counts-rank.png" alt="monthly trip totals and rank">  

#### Create quarterly tables  
In order to perform analysis by season, let's combine these tables. We'll 
create Q1, Q2, Q3 and Q4 tables for analysis. We'll have two Q1 tables-- one 
for 20221 and one for 2022 -- since we have FEB/MAR data from 2021 and JAN data 
from 2022:  

* Table 1) 2021_Q1 -> FEB(02), MAR(03)
* Table 2) 2021_Q2 -> APR(04), MAY(05), JUN(06)
* Table 3) 2021_Q3 -> JUL(07), AUG(08), SEP(09)
* Table 4) 2021_Q4 -> OCT(10), NOV(11), DEC(12)
* Table 5) 2022_Q1 -> JAN(01)

We'll first create 2021_Q2 and then repeat for the remaining four tables:  

<script src="https://gist.github.com/joeypetosa/2c7c2e6ae2ceb635a6d6360f24812ef2.js"></script>
<br> 

#### Clean and transform day of week  
Some additional data cleaning is needed on the new table. First, we'll update 
the format for `day_of_week` from `FLOAT` to `STRING`. Then, we'll change the 
values from numbers to their corresponding day names (i.e. 1 = Sunday, 7 = 
Saturday. We'll start with 2021_Q1 and repeat for the remaining four tables:  

<script src="https://gist.github.com/joeypetosa/1938c8e6ef62a94c439384f33a3fd000.js"></script>
<br>

#### Delete old tables  
Now that we have our tables organized into quarters, we can delete the original 
monthly tables from BigQuery. We no longer need the monthly tables since the 
data is available in the quarter tables. Also, it costs money to store these 
datasets in BigQuery.  

<br>


***

## Analysis #1: Exploratory  
### 2021_Q1 - quarterly data exploration  
We'll select a few columns from 2021_Q1 to preview in a temporary table. This 
will help give us an idea of potential trends and relationships to explore 
further:  

<script src="https://gist.github.com/joeypetosa/a5d6d670be6d09f5c71328571b1bbfdf.js"></script>  

<img align=center src="https://s3.us-west-1.amazonaws.com/joeypetosa.com/portfolio/cyclistic-cs/2021-q1-preview.png" alt="2021_Q1 data preview">  

The above query returned 278,119 rows. That is the number of recorded trips we 
have data for in this quarter. Let's dive deeper into those trip totals.  

<br>

#### Total trips  
We'll create total columns for overall, annual members and casual riders. We'll 
also calculate percentages of overall total for both types:

<script src="https://gist.github.com/joeypetosa/dff34920d6ea0ed9de2cee7a12588278.js"></script>  

<img align=center width=500px src="https://s3.us-west-1.amazonaws.com/joeypetosa.com/portfolio/cyclistic-cs/2021-q1-trip-totals.png" alt="2021_Q1 trip totals">  

Of the 278,118 total trips in 2021_Q1, 66% were from annual members while 34% were from casual riders.  

<br>

#### Average ride lengths
How does average `ride_length` differ for these groups?  

<script src="https://gist.github.com/joeypetosa/faa11589551287ca2e0276fccc7ebc5b.js"></script>  

<img align=center width=500px src="https://s3.us-west-1.amazonaws.com/joeypetosa.com/portfolio/cyclistic-cs/2021-q1-avg-rl.png" alt="2021_Q1 AVG ride lengths">  

We can see that casual riders average about 23 more minutes per ride. That seems 
like a pretty big difference. What influence are outliers having on these 
averages? Let's investigate.  

<br>

#### Max ride lengths
We'll look at the maximum values for `ride_length` to see if anything extreme is 
influencing the casual rider average:  

<script src="https://gist.github.com/joeypetosa/69af8c9f44e83709643bfb1ccc7aac2f.js"></script>

<img align="center" width = 250px src="https://s3.us-west-1.amazonaws.com/joeypetosa.com/portfolio/cyclistic-cs/2021-q1-max-rl.png" alt="2021_Q1 MAX ride lengths">  

As we suspected, the casual riders average `ride_length` was significantly 
impacted by at least one outlier. The longest trip duration for casual riders 
was 528 hours, or 22 days. Meanwhile, the longest for annual was about 26 hours.  

Let's take a look at the top 100 highest `ride_length` values for casual riders 
to confirm there is more than one outlier affecting the average: 

<script src="https://gist.github.com/joeypetosa/ae2b1fc8b00efdf24ab644bb94af40f6.js"></script> 

<img align=center width=500px src="https://s3.us-west-1.amazonaws.com/joeypetosa.com/portfolio/cyclistic-cs/2021-q1-max-rl-casual-100.gif" alt="2021_Q1 MAX ride lengths, casual riders top 100">  

<br>

#### Median ride lengths  
Since there are more than a few outliers impacting the average, we're going to 
use median instead of average. Median will be more accurate for our analysis:  

<script src="https://gist.github.com/joeypetosa/dff8d1c0ea3ef8ec6a78c77a879d0b7c.js"></script>  

<img align=center width=300px src="https://s3.us-west-1.amazonaws.com/joeypetosa.com/portfolio/cyclistic-cs/2021-q1-median-rl.png" alt="2021_Q1 median ride lengths">  

Now we see a much closer number, with 18 minutes for casual riders and 10 minutes for annual members.  

<br>

#### Busiest day for rides 
Let's see which day has the most rides for annual members and casual riders:    

<script src="https://gist.github.com/joeypetosa/e2d6dc494dea55d518922fe60fef7df8.js"></script>

<img align=center width=300px src="https://s3.us-west-1.amazonaws.com/joeypetosa.com/portfolio/cyclistic-cs/2021-q1-mode-day-of-week.png" alt="2021_Q1 mode day of week">  

Unsurprisingly, Saturday is the most popular day for both annual members and casual riders.  

<br>

#### Median ride length per day    
Let's look at the median ride lengths per day for both annual members and casual 
riders. Since Saturday is the most popular overall, do we think it will also 
have the highest median ride length?  

<script src="https://gist.github.com/joeypetosa/02002d2ac07a2b20aa654de9a1158a85.js"></script>

<img align=center src="https://s3.us-west-1.amazonaws.com/joeypetosa.com/portfolio/cyclistic-cs/2021-q1-median-day-of-week.png" alt="2021_Q1 median ride length, day of week, casual and member"> 

Very interesting! The median ride length for casual riders on the top five days 
(SUN, SAT, MON, TUE, WED) is nearly double the amount for annual members on 
their top five days (SAT, SUN, MON, TUE, WED).  

<br>

#### Total rides per day  
Let's look at total rides per day. We'll create columns for overall total, 
annual members and casual riders:  

<script src="https://gist.github.com/joeypetosa/7467836562044c37e5247280ff77e60c.js"></script>  

<img align=center width=350px src="https://s3.us-west-1.amazonaws.com/joeypetosa.com/portfolio/cyclistic-cs/2021-q1-trips-per-day.png" alt="2021_Q1 number of trips per day">  

<br>

#### Start stations  
Next, we'll look at the most popular start stations for trips. We'll again 
include columns for overall, annual member and casual rider totals per start 
station:  

<script src="https://gist.github.com/joeypetosa/cd9d64989e2a8ec76e518907f97e461d.js"></script>

<img align=center width=350px src="https://s3.us-west-1.amazonaws.com/joeypetosa.com/portfolio/cyclistic-cs/2021-q1-start-stations.png" alt="2021_Q1 start stations">  

We can begin to see some interesting patterns in the start station data. It 
looks like casual riders and annual members tend to favor different regions 
for beginning their trips. By updating the `ORDER BY` function to sort by
`casual DESC` and `member DESC` in two separate queries, we can compare the 
top ten start stations for both: 

<img align=center src="https://s3.us-west-1.amazonaws.com/joeypetosa.com/portfolio/cyclistic-cs/2021-q1-start-station-comparison.png" alt="2021_Q1 start stations">  

Wow! There is only one start station that cracks the top ten for both lists.  
The [Clark St & Elm St](https://www.google.com/maps/place/W+Elm+St+%26+N+Clark+St,+Chicago,+IL+60610/data=!4m2!3m1!1s0x880fd34f04679fd7:0xfcb6443483c236c3?sa=X&ved=2ahUKEwiS7_n4zIr2AhWCJzQIHQS-C9gQ8gF6BAgIEAE) 
start station is ranked #1 for annual members and #10 for casual riders. The 
casual riders seem to favor stations near the water like [Lake Shore Dr & Monroe St](https://www.google.com/maps/place/S+Lake+Shore+Dr+%26+E+Monroe+St,+Chicago,+IL+60601/@41.8809363,-87.61969,17z/data=!3m1!4b1!4m5!3m4!1s0x880e2b58c766aa85:0x6a3ce76eb41e58c6!8m2!3d41.8809363!4d-87.617496) and [Streeter Dr & Grand Ave](https://www.google.com/maps/place/E+Grand+Ave+%26+N+Streeter+Dr,+Chicago,+IL+60611/@41.8921227,-87.6122317,17z/data=!3m1!4b1!4m5!3m4!1s0x880e2b525e10ee6d:0x1c44bc2bc0376a7!8m2!3d41.8921227!4d-87.610043), while 
annual members frequent start stations in the River North neighborhood like 
[Dearborn St & Erie St](https://www.google.com/maps/place/W+Erie+St+%26+N+Dearborn+St,+Chicago,+IL+60654/@41.8940553,-87.6319277,17z/data=!3m1!4b1!4m5!3m4!1s0x880e2cb2bff8e189:0x102f5d20351b9226!8m2!3d41.8940553!4d-87.629739) 
and [Kingsbury St & Kinzie St](https://www.google.com/maps/place/N+Kingsbury+St+%26+W+Kinzie+St,+Chicago,+IL+60654/@41.8908528,-87.6443857,15z/data=!4m5!3m4!1s0x880e2cb62185f3eb:0x9896f5bb29f51f13!8m2!3d41.8891037!4d-87.6380879).  

An initial hypothesis for casual riders could be that they tend to favor start 
stations near the water and close to tourist attractions because they use bikes 
for weekend entertainment. An initial hypothesis for annual members could be 
that they tend to favor start stations in downtown, retail areas because they 
are using bikes for their work commutes and shopping trips.  

#### Quarterly data exploration (cont.) 
Instead of walking through each quarter like we've done for 2021_Q1, I will 
instead provide links to the full SQL files. The queries used are similar to 
the ones above:  

* <a href="https://github.com/joeypetosa/cyclistic_cs/blob/main/analysis_2021_Q1.sql" target="_blank">analysis_2021_Q1.sql</a>  
* <a href="https://github.com/joeypetosa/cyclistic_cs/blob/main/analysis_2021_Q2.sql" target="_blank">analysis_2021_Q2.sql</a>
* <a href="https://github.com/joeypetosa/cyclistic_cs/blob/main/analysis_2021_Q3.sql" target="_blank">analysis_2021_Q3.sql</a>  
* <a href="https://github.com/joeypetosa/cyclistic_cs/blob/main/analysis_2021_Q4.sql" target="_blank">analysis_2021_Q4.sql</a>  
* <a href="https://github.com/joeypetosa/cyclistic_cs/blob/main/analysis_2022_Q1.sql" target="_blank">analysis_2022_Q1.sql</a>  

I'll included some high-level quarterly analysis notes in the next section.  


## Analysis #2: Summary  
### full_year - trends, relationships and insights

In order to analyze all twelve months together, we'll combine the five 
quarterly tables into one table. The queries used to accomplish this 
are included <a href="https://github.com/joeypetosa/cyclistic_cs/blob/main/setup_full_year.sql" target="_blank">here</a> 
for reference. I've also provided the SQL file used for full year analysis: <a href="https://github.com/joeypetosa/cyclistic_cs/blob/main/analysis_full_year.sql" target="_blank">analysis_full_year.sql</a>.  

For a summary and overall visualization of my full year analysis, please visit the 
Tableau Public dashboard I created here: <a href="https://public.tableau.com/views/CyclisticBikeshareinChicago/CyclisticBikeshareinChicago?:language=en-US&:display_count=n&:origin=viz_share_link" target="_blank">Tableau Dashboard: Cyclistic Bikeshare in Chicago</a>.  
I will also highlight some of the interesting trends and relationships I discovered
below.  

### Annual Members vs Casual Riders  
<img align=center src="https://s3.us-west-1.amazonaws.com/joeypetosa.com/portfolio/cyclistic-cs/ccs-summary-area-chart.png" alt="member vs casual">  
During the past 12 months, annual members accounted for 55% of Cyclistic's total 
trips while casual riders accounted for 45% of total trips. As we can see in the 
above area chart, this percentage fluctuates throughout the year.  

### Seasonal trends  

<img align=center src="https://s3.us-west-1.amazonaws.com/joeypetosa.com/portfolio/cyclistic-cs/ccs-quarterly-total-trips.svg" alt="quarterly trip totals">  

#### *Summer vs Winter*  
The busiest time of year for overall bike trips is Q3-- July, August and 
September. This makes sense because these months are mainly summer time. Bike 
riding is better suited for warmer weather, which is also why we see a major 
drop-off in total rides during the winter months of Q1-- January, February and 
March. 

Annual members outnumbered casual riders in every quarter except Q3. 
Interestingly, the annual members nearly doubled the casual ridership in Q1 and 
Q4 while only slightly edging them out in Q2. 

#### *Median ride length*  
We learned in the earlier quarterly analysis that the average `ride_length` for 
casual riders was significantly impacted by outliers, so median is a more 
accurate measurement for our analysis:  
<img align=center src="https://s3.us-west-1.amazonaws.com/joeypetosa.com/portfolio/cyclistic-cs/ccs-summary-median-rl.svg" alt="median ride lengths">  

We can see that casual riders consistently have longer rides than annual members. 

#### *Day of week*  
Which days of the week have the highest number of rides for casual riders vs 
annual members? Let's look at the mode for each quarter and for the full year:  

<img align=center width=500px src="https://s3.us-west-1.amazonaws.com/joeypetosa.com/portfolio/cyclistic-cs/ccs-summary-mode-day.png" alt="most popular days of week for rides"> 

Casual riders were extremely consistent, with **Saturday** revealing itself as 
their preferred day of week for each quarter and across the full year. Meanwhile, 
the annual members looked to favor the middle of the week for their bike use. The 
most popular day for them acrosss the full year was **Wednesday**. Let's see how 
the total rides for each day stack up for both groups:  

<img align=center src="https://s3.us-west-1.amazonaws.com/joeypetosa.com/portfolio/cyclistic-cs/ccs-summary-trips-per-day.svg" alt="total rides per day of week over full year">  

How about median ride length per day of week for both groups?  

<img align=center src="https://s3.us-west-1.amazonaws.com/joeypetosa.com/portfolio/cyclistic-cs/ccs-summary-median-rl-per-day.svg" alt="median ride lengths per day of week">  

A few fascinating insights from the above chart:  

* **U-shape pattern**  
  Sunday and Saturday are favored by both groups for longer
  rides, while ride duration decreases towards the middle of the week before 
  increasing again on Friday. This results in a u-shape trend for both groups in 
  the above chart, although it is much more dramatic for casual riders.  

* **Range differences**  
  For annual members, difference between their longest day 
  and their shortest day is 1 minute and 44 seconds. For casual riders, 
  difference is 4 minute and 57 seconds. That is a 185.58% increase in 
  difference for casual riders.  

* **Annual members: day-to-day consistency**  
  The annual members may have shorter ride lengths when compared to casual 
  riders, but they are extremely consistent with their bike use day-over-day.  

* **Casual riders: weekend warriors**  
  The daily median ride length for casual riders is consistently higher than 
  that of annual members. The range of their ride length duration varies at a 
  greater amount than that of annual members. Sundays and Saturdays stand out 
  as their longest ride days.  

#### Bike type    
Do members and casual riders have different preferences for bike type? Are 
classic bikes more popular than electric bikes?  

<img align=center src="https://s3.us-west-1.amazonaws.com/joeypetosa.com/portfolio/cyclistic-cs/ccs-summary-rides-by-type.svg" alt="total rides by bike type over full year">  

We can see that classic bikes are favored by both groups. Let's look at the 
percentages of bike type use within each group:  

<img align=center src="https://s3.us-west-1.amazonaws.com/joeypetosa.com/portfolio/cyclistic-cs/ccs-summary-bike-type-group-percentage.svg" alt="group percentages of rides by bike type over full year">  

Looking at the above, we might ask what exactly is a docked bike and why are only 
casual riders using them?  

<img align=center src="https://s3.us-west-1.amazonaws.com/joeypetosa.com/portfolio/cyclistic-cs/ccs-summary-bike-type-avg-and-max-rl.png" alt="bike type average and max ride lengths">  

We can now see from the above charts that docked bikes are the culprit for the
outliers affecting our ride length averages from earlier in our analysis. This is 
something we should discuss with our team further and address.  

#### Start and end station use  
In the Tableau Dashboard I created, which is again available <a href="https://public.tableau.com/views/CyclisticBikeshareinChicago/CyclisticBikeshareinChicago?:language=en-US&:display_count=n&:origin=viz_share_link" target="_blank">here</a>, there is a worksheet that
allows the exploration of start and end station use by members, casual riders 
and combined overall rides. The snapshot below is from the overall view. 
While interacting with the dashboard, we can see that  casual riders have 
a higher max than annual members. Annual members have a lower max, but we 
can see more colors represented across the member map versus the consistent 
coloring across the casual map. This tells us that rides by members are more 
distributed across stations while rides by casual riders are more top heavy in 
that a huge chunk are happening at the same few stations.  

<img align=center src="https://s3.us-west-1.amazonaws.com/joeypetosa.com/portfolio/cyclistic-cs/ccs-tableau-station-use.png" alt="cyclistic start and end station use, tableau dashboard screenshot">  
<div style="align: center; margin-left: 0px;"> 


***

## Conclusion

### Stakeholder presentation and dashboard

I’ve provided links below for my dashboard and shareholder presentation, which 
includes the following:

* A summary of my analysis
* Supporting visualizations and key findings
* Three recommendations based on my analysis


* [Tableau Dashboard: Bikeshare in Chicago](https://public.tableau.com/app/profile/andrewdeekim/viz/BikeshareinChicago/BikeshareinChicago"target="_blank")


* [Slides: Navigating Speedy Success](https://docs.google.com/presentation/d/1eFg7z36HifSdGsDJuQC-b0w9eSGLmz2mKFWGHc-mPAM/edit?usp=share_link)
