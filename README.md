## Case Study: How Does a Bike-Share Navigate Speedy Success?

### Scenario

I am assigned as a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director of marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore, my team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, your team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve your recommendations, so they must be backed up with compelling data insights and professional data visualizations.

### Background:

Cyclistic’s finance analysts have concluded that annual members are much more profitable than casual riders. My manager, Lily Moreno, believes that maximizing the number of annual members will be key to future growth. Rather than creating a marketing campaign that targets all-new customers, Moreno believes there is a very good chance to convert casual riders into members.

### Stakeholders:

Primary stakeholder: Cyclistic executive team: The notoriously detail-oriented executive team will decide whether to approve the recommended marketing program.

#### Secondary stakeholder:

Lily Moreno: The director of marketing and your manager. Moreno is responsible for the development of campaigns and initiatives to promote the bike-share program. These may include email, social media, and other channels

Cyclistic marketing analytics team: I joined this team of data analysts six months ago. We are responsible for collecting, analyzing, and reporting data that helps guide Cyclistic marketing strategy and help Cyclistic achieve them.

### Objective:

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

### Data Preparation:

I will be using Cyclistic’s past 12 month's (January 2021 - December 2021) trip data to analyze and identify trends (Note: The datasets have a different name because Cyclistic is a fictional company. For the purposes of this case study, the datasets are appropriate and will enable you to answer the business questions. The data has been made available by Motivate International Inc. under this [license](https://ride.divvybikes.com/data-license-agreement)

The data are collected internally using Cyclistic's own system. Then, the data are uploaded to a secured server for users to download. The data was downloaded and stored on my secured hard rive to insure privacy and security.

We can safely assume that the data is non bias, reliable, original, current, and cited as it is collected internally by Cyclistic. It is also comprehensive, since the data are formatted in .csv files.

### Data Processing:

The data are duplicated, and the raw data set is stored in a separate folder. The duplicated set is then reviewed in Microsoft Excel: 1) Reviewed the spreadsheets to get a good idea on what I have to work with. 2) Sorting and filtering: Removing numbers that are too big, too small, or values that does not belong in a column. Then, reveal “null”, or NA values in the columns using filters. By removing those rows, it would allow for smoother cleaning later. 3) Performed the duplicate removal function on all data sets. Afterward, I added three new columns:

- ride_length: showing the duration of each rental "=TEXT(D2-C2,"HH:MM:SS")"
- day_of_week: showing rental per day of the week "=WEEKDAY(C2,1)"
- day_of_week_w: since =WEEKDAY() returns a number, I use this column to translate the number into a word "=IF(O2=1, "Sunday",IF(O2=2, "Monday",IF(O2=3, "Tuesday",IF(O2=4, "Wednesday", IF(O2=5, "Thursday", IF(O2=6, "Friday", IF(O2=7, "Saturday", "Null")))))))"

I will be importing data into RStudio for the remaining portion of data cleaning. I will also employ different Data Analysis Packages offered by the R community, such as tidyverse, janitor, and lubridate, to allow for more efficient and effective cleaning.

tidyverse is used for data analysis and obtaining quick visualization to gain insight easier janitor is used for cleaning rows and columns of empty values lubridate is used for easy date/time application

```{r}
install.packages("tidyverse")
install.packages("janitor")
install.packages("lubridate")
library(tidyverse)
library(janitor)
library(lubridate)
```
Then, I inputted the 12 data sets from my hard drive into R.

```{r}
df1 <- read.xlsx("E:\Data Analytics\Data Set\Cleaned Data/202101-divvy-tripdata.xlsx")
df2 <- read.xlsx("E:\Data Analytics\Data Set\Cleaned Data/202102-divvy-tripdata.xlsx")
df3 <- read.xlsx("E:\Data Analytics\Data Set\Cleaned Data/202103-divvy-tripdata.xlsx")
df4 <- read.xlsx("E:\Data Analytics\Data Set\Cleaned Data/202104-divvy-tripdata.xlsx")
df5 <- read.xlsx("E:\Data Analytics\Data Set\Cleaned Data/202105-divvy-tripdata.xlsx")
df6 <- read.xlsx("E:\Data Analytics\Data Set\Cleaned Data/202106-divvy-tripdata.xlsx")
df7 <- read.xlsx("E:\Data Analytics\Data Set\Cleaned Data/202107-divvy-tripdata.xlsx")
df8 <- read.xlsx("E:\Data Analytics\Data Set\Cleaned Data/202108-divvy-tripdata.xlsx")
df9 <- read.xlsx("E:\Data Analytics\Data Set\Cleaned Data/202109-divvy-tripdata.xlsx")
df10 <- read.xlsx("E:\Data Analytics\Data Set\Cleaned Data/202110-divvy-tripdata.xlsx")
df11 <- read.xlsx("E:\Data Analytics\Data Set\Cleaned Data/202111-divvy-tripdata.xlsx")
df12 <- read.xlsx("E:\Data Analytics\Data Set\Cleaned Data/202112-divvy-tripdata.xlsx")
```

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/AsmaaKhattab45/Data-Analytics-Case-Study/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
