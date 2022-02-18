# Case Study: How Does a Bike-Share Navigate Speedy Success?

## Scenario

I am assigned as a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director of marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore, my team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, your team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve your recommendations, so they must be backed up with compelling data insights and professional data visualizations.

## Background:

Cyclistic’s finance analysts have concluded that annual members are much more profitable than casual riders. My manager, Lily Moreno, believes that maximizing the number of annual members will be key to future growth. Rather than creating a marketing campaign that targets all-new customers, Moreno believes there is a very good chance to convert casual riders into members.

## Stakeholders:

Primary stakeholder: Cyclistic executive team: The notoriously detail-oriented executive team will decide whether to approve the recommended marketing program.

### Secondary stakeholder:

Lily Moreno: The director of marketing and your manager. Moreno is responsible for the development of campaigns and initiatives to promote the bike-share program. These may include email, social media, and other channels

Cyclistic marketing analytics team: I joined this team of data analysts six months ago. We are responsible for collecting, analyzing, and reporting data that helps guide Cyclistic marketing strategy and help Cyclistic achieve them.

## Objective:

Final goal for the marketing analytics team: Design marketing strategies aimed at converting casual riders into annual members.

Business task assigned to me: Figure out how annual members and casual riders use Cyclistic bikes differently.

In doing so, I hope to help stakeholders better understands:

- How annual members and casual riders differ.
- Why casual riders would buy a membership.
- How digital media could affect their marketing tactics.
- If there are sufficient data to answer those questions.

   To achieve that, I have defined four matrices to evaluate the differences between annual members and causal rider's rental pattern:

      - Frequency of rental
      - Average duration of rentals
      - Locations with the most rentals
      - Most popular bike types

## Data Preparation:

I will be using Cyclistic’s past 12 month's (January 2021 - December 2021) trip data to analyze and identify trends (Note: The datasets have a different name because Cyclistic is a fictional company. For the purposes of this case study, the datasets are appropriate and will enable you to answer the business questions. The data has been made available by Motivate International Inc. under this [license](https://ride.divvybikes.com/data-license-agreement)

The data are collected internally using Cyclistic's own system. Then, the data are uploaded to a secured server for users to download. The data was downloaded and stored on my secured hard rive to insure privacy and security.

We can safely assume that the data is non bias, reliable, original, current, and cited as it is collected internally by Cyclistic. It is also comprehensive, since the data are formatted in .csv files.

## Data Processing:

The data are duplicated, and the raw data set is stored in a separate folder. The duplicated set is then reviewed in Microsoft Excel: 1) Reviewed the spreadsheets to get a good idea on what I have to work with. 2) Sorting and filtering: Removing numbers that are too big, too small, or values that does not belong in a column. Then, reveal “null”, or NA values in the columns using filters. By removing those rows, it would allow for smoother cleaning later. 3) Performed the duplicate removal function on all data sets. Afterward, I added three new columns:

- ride_length: showing the duration of each rental "=TEXT(D2-C2,"HH:MM:SS")"
- day_of_week: showing rental per day of the week "=WEEKDAY(C2,1)"
- day_of_week_w: since =WEEKDAY() returns a number, I use this column to translate the number into a word "=IF(O2=1, "Sunday",IF(O2=2, "Monday",IF(O2=3, "Tuesday",IF(O2=4, "Wednesday", IF(O2=5, "Thursday", IF(O2=6, "Friday", IF(O2=7, "Saturday", "Null")))))))"

I will be importing data into RStudio for the remaining portion of data cleaning. I will also employ different Data Analysis Packages offered by the R community, such as tidyverse, janitor, and lubridate, to allow for more efficient and effective cleaning.

tidyverse is used for data analysis and obtaining quick visualization to gain insight easier janitor is used for cleaning rows and columns of empty values lubridate is used for easy date/time application

I will use readxl library to read excel files of te cleaned data.

```{r}
install.packages("tidyverse")
install.packages("janitor")
install.packages("lubridate")
library(tidyverse)
library(janitor)
library(lubridate)
library(readxl)
```
Then, I inputted the 12 data sets from my hard drive into R.

```{r}
df1 <- read_excel("E:\\Data Analytics\\Data Set\\Cleaned Data\\202101-divvy-tripdata.xlsx")
df2 <- read_excel("E:\\Data Analytics\\Data Set\\Cleaned Data\\202102-divvy-tripdata.xlsx")
df3 <- read_excel("E:\\Data Analytics\\Data Set\\Cleaned Data\\202103-divvy-tripdata.xlsx")
df4 <- read_excel("E:\\Data Analytics\\Data Set\\Cleaned Data\\202104-divvy-tripdata.xlsx")
df5 <- read_excel("E:\\Data Analytics\\Data Set\\Cleaned Data\\202105-divvy-tripdata.xlsx")
df6 <- read_excel("E:\\Data Analytics\\Data Set\\Cleaned Data\\202106-divvy-tripdata.xlsx")
df7 <- read_excel("E:\\Data Analytics\\Data Set\\Cleaned Data\\202107-divvy-tripdata.xlsx")
df8 <- read_excel("E:\\Data Analytics\\Data Set\\Cleaned Data\\202108-divvy-tripdata.xlsx")
df9 <- read_excel("E:\\Data Analytics\\Data Set\\Cleaned Data\\202109-divvy-tripdata.xlsx")
df10 <- read_excel("E:\\Data Analytics\\Data Set\\Cleaned Data\\202110-divvy-tripdata.xlsx")
df11 <- read_excel("E:\\Data Analytics\\Data Set\\Cleaned Data\\202111-divvy-tripdata.xlsx")
df12 <- read_excel("E:\\Data Analytics\\Data Set\\Cleaned Data\\202112-divvy-tripdata.xlsx")
```

Methods used to begin the analysis:

- Consolidate the data sets into one so it is easier to work with
- Clean the data set using janitor to make sure there are no blank values
- Ensure columns are in correct data formats
- Keep only the columns I need for my analysis

With those in mind, I proceeded with the cleaning
```{r}
bike_rides <- rbind(df1,df2,df3,df4,df5,df6,df7,df8,df9,df10,df11,df12)
bike_rides <- janitor::remove_empty(bike_rides, which = c("cols"))
bike_rides <- janitor::remove_empty(bike_rides, which = c("rows"))
```
Next, I used a few summary functions to see if the data are ready to be used.
```{r}
str(bike_rides)
summary(bike_rides)
```
![summary](https://user-images.githubusercontent.com/58610546/151686351-cbe09e50-4f3b-4426-8bd8-e6c6d6cd7bcd.PNG)

![str](https://user-images.githubusercontent.com/58610546/151686365-434844f2-e6d9-4daa-9585-2e1fb93a6216.PNG)

All the column names are here, but I noticed in the summary() command that the started_by and ended_by columns are characters, instead of dttm format. So I used the following code to convert it:

```{r}
bike_rides$started_at <- as.POSIXct(bike_rides$started_at, format="%m/%d/%Y %H: %M")
bike_rides$ended_at <- as.POSIXct(bike_rides$ended_at, format="%m/%d/%Y %H: %M")
```
Now, I can perform the difftime() function to get the ride duration of each rental. I also used the as.numberic() function to ensure we can perform statistical calculation on the column later.

```{r}
bike_rides$ride_length <- difftime(bike_rides$ended_at, bike_rides$started_at, units="mins")
bike_rides$ride_length <- as.numeric(bike_rides$ride_length)
```
Now that we have ride_length in place, I want to expand the analysis beyond only the day of the week. Keeping in mind that our primary stakeholders are detail-oriented, I will break down the data into days and month. Since we are focusing on the pass 12 months of data, the calender year might not be as important. However, in case the stakeholders ask, I can add in another line of code for creating a year column.

```{r}
bike_rides$day <- weekdays(bike_rides$started_at)
bike_rides$month <- months(bike_rides$started_at)
bike_rides$year <- year(bike_rides$started_at)
```
Once those columns are created, I stopped and reviewed what I have.

```{r}
View(bike_rides)
```
![view](https://user-images.githubusercontent.com/58610546/151687937-9f9786fe-1a9f-4224-8e06-7a2bb07f1ec0.PNG)

Upon checking, I noticed from the filter function that the values range from -2000 to 56000. This could be due to technical difficulties, such as stations not capturing the returns correctly. Or the company took in bikes from different stations for repairing, and deployed them to the wrong station after. This would be a question to bring up during the next team meeting. For now, those negative values cleaned out.

```{r}
bike_rides <- filter(bike_rides, bike_rides$ride_length > 0)
```

I have also noticed that the start_lat, start_lng, end_lat, and end_lng are not necessary because one station could have multiple latitude and longitude. To double check if that is the case before I remove the columns, I used:

```{r}
name_lat_lng <- bike_rides %>%
  select(start_station_name, start_lat, start_lng)
View(name_lat_lng) 
```

After confirming that is the case, I removed the columns. Afterward, a quick check with the str() function.

```{r}
bike_rides <- bike_rides %>%
  select(-c(start_lat, start_lng, end_lat, end_lng))
str(bike_rides)
```

![strr](https://user-images.githubusercontent.com/58610546/151739185-faa4ecc5-61c8-41ab-9f78-d84b16b72cfe.PNG)

Excellent, the four columns are removed

So far, the data looks to be ready for further analysis. We can obtain a few important stats by using aggregate():

```{r}
mean_member_casual <- aggregate(ride_length~member_casual, bike_rides, mean)
max_member_casual <- aggregate(ride_length~member_casual, bike_rides, max)
min_member_casual <- aggregate(ride_length~member_casual, bike_rides, min)
mean_member_casual
max_member_casual
min_member_casual
```

![aggregate](https://user-images.githubusercontent.com/58610546/151739299-79a48c71-7166-41ad-871f-124f5c1f2005.PNG)

Obviously, that will not satisfy our need. Those stats will at most give us a high level review of the data.

## Data Visualization

We could use the ggplot() function in R to create data viz by using ggplot(). However, due to the large amount of comparison in different matrices. I will mainly be using Tableau Public to create data viz since it is easier and more efficient to use.

```{r}
ggplot(data=bike_rides)+
  geom_bar(mapping=aes(x=day_of_week_w, fill=member_casual))+
  labs(
    title="Number of Rental per Day of the Week",
    x="Day of the Week",
    y="Number of Rentals",
    )
```

![ggplot](https://user-images.githubusercontent.com/58610546/151739385-4cd49164-23b2-4597-8739-b68fa749c8bd.PNG)

I will export the cleaned data set so we can upload it to Tableau Public.

```{r}
write.csv(bike_rides, "bike_rides_cleaned_case_1_data.csv")
```

## **Using [Tableau Public](https://public.tableau.com/s/)**

By using Tableau, a public data visualization tool, I am able to create data viz with dragging and dropping different fields. This greatly improved efficiency.

Let us recall the four matrices we will be evaluating the differences of member and causal riders with:

- Frequency of rental
- Average duration of rentals
- Locations with the most rentals
- Most popular bike types

### Frequency

The first difference we can observe is that casual riders have significantly more rental during Saturday and Sunday. Member riders have a more consistent level of bike usage throughout the week, with the lowest use on Sundays.
