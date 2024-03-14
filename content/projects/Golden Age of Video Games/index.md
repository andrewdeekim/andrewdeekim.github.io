---
title: Golden Age of Video Games Analysis Using SQL(BigQuery) and Tableau
date: 2023-11-10T18:08:42-04:00
draft: true
description: data analytics, Spreadsheets, SQL, BigQuery, Tableau
weight: 1
slug: movienow
categories:
  - Data Analytics
tags:
  - Data Analytics
  - Case Study
  - SQL
  - BigQuery
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
  alt: "MovieNow Rental Analysis Using SQL(BigQuery) and Tableau by Andrew Kim"
hideMeta: true
---

In this project, I analyze historical data from an online movie rental company in order to identify trends in customer preferences, engagement, and sales development. This highlights skills in SQL (PostgreSQL in BigQuery) like aggregate functions, joins, subqueries, OLAP, Common Table Expressions (CTE) and Window Functions. The main tools I use are SQL in Google BigQuery and Tableau. Here are the highlights:  


<!-- * [Tableau Dashboard: ???Bikeshare in Chicago](https://public.tableau.com/app/profile/andrewdeekim/viz/BikeshareinChicago/BikeshareinChicago"target="_blank") -->

* [GitHub: MovieNow Rental Analysis Repository](https://github.com/andrewdeekim/bike-share-in-chicago)

<!-- * [Slides: ???Navigating Speedy Success](https://docs.google.com/presentation/d/1eFg7z36HifSdGsDJuQC-b0w9eSGLmz2mKFWGHc-mPAM/edit?usp=share_link) -->


A more in-depth breakdown of the scenario is included below, followed 
by my full report.  
<br>


### Scenario
MovieNow wishes to make more informed decisions about which movies to add to their inventory by analyzing which actors are the most popular and which genres are the highest in demand. They also want to explore rental policies in order to maximize customer satisfication and sales revenue.

<br>

***

## ASK: Defining the Business Task  
Three questions will guide the future marketing program:

* How do annual members and casual riders use Cyclistic bikes differently?
* Why would casual riders buy Cyclistic annual memberships?
* How can Cyclistic use digital media to inuence casual riders to become members?

The dierctor of marketing and my manager has assigned me the first question to answer. Therefore the business task can be stated as follows:

> ### Analyze historical bike trip data to identify trends in how annual members and casual riders use Cyclistic bikes differently.


<br>

***

## PREPARE: Data sources

We used historical bike trip data from the last full year (12 months): January 2022 – December 2022. This helps us get a full picture of the annual calendar year and the potential effect from seasons factors. The data is from [Divvy Bike Trip Data](https://divvy-tripdata.s3.amazonaws.com/index.html) and has been made publicly available by Motivate International Inc. under [this license](https://www.divvybikes.com/data-license-agreement). We downloaed the following CSV files:


    1)  2022-01_divvy_trip-data.csv
    2)  2022-02_divvy_trip-data.csv  
    3)  2022-03_divvy_trip-data.csv  
    4)  2022-04_divvy_trip-data.csv  
    5)  2022-05_divvy_trip-data.csv  
    6)  2022-06_divvy_trip-data.csv  
    7)  2022-07_divvy_trip-data.csv  
    8)  2022-08_divvy_trip-data.csv  
    9)  2022-09_divvy_trip-data.csv //renamed to match other filenames.
    10) 2022-10_divvy_trip-data.csv  
    11) 2022-11_divvy_trip-data.csv  
    12) 2022-12_divvy_trip-data.csv  
  

The data is organized with each row (record) corresponding to a single trip identified by `ride_id` and incldues the following columns (fields):  

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

There are no issues with bias or credibility as all personal identifiable information (PII) has been removed. The data is also credible as it is primary data from the company itself. In other words, it ROCCC's:

* **Reliable and Original**: the data is both reliable and original as it is primary source data.
* **Comprehensive**: the data has all of the relevant fields necessary for our historical analysis.
* **Current**: the data is current as it is from the specific time frame we need 2022 and is updated monthly.
* **Cited**: the data is cited as it is verified as a primary souce.
I have saved a folder of the original data and made copies to manipulate for my analysis.


<br>


***

## PROCESS: Data Cleaning & Manipulation

### R (Programming Language): Initial Data Cleaning and Manipulation  
Due to the large size and number of files, I performed my processing and analysis using R and exported it for visualization using Tableau.

### Step 0: Initialize Workspace

{{< github-code-snippets 1f11ced948e3ab1a7e6bac0fdf1ed11f >}}

<br> 

### Step 1: Collect Data
I downloaded the files and renamed `2022-09_divvy_trip-data.csv` to match the other filenames.

{{< github-code-snippets 0bd187973084c32e6e3c584d15aae01e >}}

<br> 

### Step 2: Wrangle Data and Combine into a Single File
Note, we will need to compare the column names to see if an rbind is possible. We use the helpful `compare_df_cols_same` fcn from our janitor library. This saves space as the alternative would be to call the column name of each file and manually compare them (e.g. `colnames(jan22)` ).

Then we inspect the dataframes to look for incongruencies.

Lastly, we combine the files into a single dataframe.

{{< github-code-snippets e458dc55d1068070cb07c0b194b87b19 >}}

<br> 

### Step 3: Clean Up and Add Data
First, we inspect the new dataframe that has been created:

{{< github-code-snippets 9bc84dca2a86139a58eda98983470c36 >}}

<br> 

We note that there are a few problems we need to fix:

* The data can only be aggregated at the ride-level, which is too granular. We will want to add some additional columns of data (e.g. day, month, year) that provide additional opportunities to aggregate the data.
* We will want to add a calculated field for length of ride as `ride_length` for helpful analysis and inspect it.

Note, we need to convert `ride_length` to numeric and convert it to minutes as it's easier to understand:

{{< github-code-snippets 64832671d322ffde2623d57e62ffb6a3 >}}

<br> 

Before we continue, we decide to remove "bad" data that includes missing `start/end station id` and/or names or has negative `ride_length` values. Then, we will create a `trip_type` variable to inspect if there are any insights in how casual riders and annual members use the service differently based on this:

{{< github-code-snippets bf9f62ec8163d72e45adc5005295b50d >}}


## ANALYZE: Summary of Calculations, Trends, and Relationships
### Descriptive Analysis

We will begin with a descriptive analysis on our cleaned up data set `all_trips_v2`. We will compare the mean, median, max, and min between casual riders and annual members.

{{< github-code-snippets 08106ff7d9c1c52afe0d0b4ab0cd256d >}}
![Ride Length Summary](img/ride_length.png)

We also analyze by DAY, MONTH, and RIDE_TYPE by WEEKDAY
{{< github-code-snippets be9babf03c2cec12ea6188b48eb24f19 >}}


There are a few things to note:

* At first glance, casual riders' trips are almost 2x as long as annual members' on average.
* However, we note that the mean for the total trips is 10.60 min while the mean is 17.10 min, indicating a presence of outliers skewing the average.
* This is confirmed as the max for a casual rider is 34,354 minutes, which would indicate a ride length of about 23 days! * Furthermore, the max ride for an annual member is 1,493 minutes(or 24 hours!)
* Upon glancing at the data, we recongize using the median and also the total amount of rides (versus length) may be more helfpul.


### Visual Analysis

Next, we will conduct a visual analysis. First, we will note how **annual members** and **casual riders** differ on the total amount of rides given the day of the week.
{{< github-code-snippets 89326cb08e6da10f71744efef7004488 >}}

Here is the resulting visualization: 
![Day of Week vs. Ride_Type](img/dow_ride-type.png)

We will break down this visualization further: 
{{< github-code-snippets 38df4a9c98dd6178330cf5aedaf5ff62 >}}

Here is the resulting visualization: 
![Rider Type](img/rider_type.png)


> ### INSIGHT: Annual members prefer the weekdays whereas casual riders prefer the weekends.


<br> 


Next, we will note how **annual members** and **casual riders** differ on the average duration of rides given the day of the week.

{{< github-code-snippets 23995fd8b3238579dab8e01211fd2908 >}}

Here is the resulting visualization: 
![Average Duration](img/avg_dur.png)

<br> 

Last, we will note how **annual members** and **casual riders** differ on the average median of rides given the day of the week (due to the skewing mentioned earlier).

{{< github-code-snippets 7b8edddd61e0c60388f490c16fa87491 >}}

Here is the resulting visualization: 
![Median Duration](img/med_dur.png)


<br> 

As we see, these totals are closer together, and the maximum values are around the 1000 minutes range compared to the 1750 minutes range from the mean graph above. Also, we recognize that in both the mean and median, **casual riders** have longer ride lengths than **annual members**

> ### INSIGHT: Casual riders have longer ride lengths than annual members

<br>

***

## SHARE: Supporting Visualizations and Key Findings
Lastly, we export the aggregate data for visualizations in Tableau:

{{< github-code-snippets 5c646180eab47e401fb4f50717147a03 >}}


***

## Conclusion

### Stakeholder presentation and dashboard

I’ve provided links below for my dashboard and shareholder presentation, which 
includes the following:

* A summary of my analysis
* Supporting visualizations and key findings
* Three recommendations based on my analysis

<!-- [Tableau Dashboard: Bikeshare in Chicago](https://public.tableau.com/app/profile/andrewdeekim/viz/BikeshareinChicago/BikeshareinChicago"target="_blank") -->

[GitHub: MovieNow Rental Analysis Repository](https://github.com/andrewdeekim/bike-share-in-chicago)

<!-- [Slides: Navigating Speedy Success](https://docs.google.com/presentation/d/1eFg7z36HifSdGsDJuQC-b0w9eSGLmz2mKFWGHc-mPAM/edit?usp=share_link) -->
