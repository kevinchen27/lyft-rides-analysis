# Lyft Rides Data Analysis (Not For Monetization - Creative, not-for-profit purposes only)

Analyzing Lyft data to draw potential insights about driver and rides patterns. Insights include questions that could be of use to Lyft to improving its rides experience.

## Introduction

There are a couple of reasons I am fond of Lyft. Most recently, the biggest one is its scooter pricing. At UCLA, Lyft scooters are convenient to move around with. Cheaper than Birds and more abundant than Limes and Ubers, I'm always on the prowl for Lyft scooters if I'm late to class to returning from a friend's place late at night. The second reason is that I have noticed that its shared rides are usually more inexpensive than Ubers, and if one adds up their rides, they could actually find themselves saving a decent amount on ride-sharing.

As both a rider and from a business perspective, I want to analyze this data set to draw insights about the Lyft experience for riders and drivers, and potential business opportunities for Lyft.

## Data Preprocessing

I obtained the data from Kaggle and is known as the Lyft Data Challenge. Before proceeding with analysis, I first checked to see if there were any extraordinary observations in my data. My data consisted of three separate files, which I joined toegther using inner joins on the a common key:
- driver_ids : contains the driver id's and onboarding dates
- ride_ids : contains information about rides including irde distance and duration, ride id, the prime time charge, and the associated driver id
- ride_timestamps : contains the time of event, ride id and event(requested, accepted, etc.)
<br>
On looking at the data, there was only one NA value in the timestamp file, and no odd dates, times, or events. The starting date of this data was March 27 and the end date was June 27. However, there were ride distances of less than 0 and as high as 700,000 units. Knowing Lyft rides cannot exceed 100 miles, I assumed that this distance was in feet. Thus, I eliminated entries with more than 100 miles or 528000 feet.

## What's important, based on the data?

I thought the following insights could be important given the data:
- % of successful pick-ups after acceptance
- Mean difference between request and accept times per group of prime time
- Drivers with least number of completed rides
- Are number of rides increasing/decreasing/stagnant over this duration?
- Mean number of rides per driver on given day of the week
- How many drivers with at least one completed ride in April also completed a ride in June?
- Frequency of Lyft rides according to hour and day

## Why could these insights by important?
- Percentage of successful pick ups could be an important indicator for rider retention. Low or drop in percentage of successful pick ups after accepting a request could turn riders away from Lyft and towards competitors
- Prime time is when Lyft prices are higher than normal. One might expect the mean request and accept times during prime-time to be lower than during non-prime-time since drivers might be more eager to accept a costlier ride. On the other hand, this metric could also be misleading since there is more demand during prime-time, leading to increased pressure on the driver supply which could be outweighed by demand, leading to longer average accept times. However, in general this would probably be something Lyft would be looking to decrease all the time (faster acceptance, greater rider satisfaction)
- Since this dataset gives us a lot of information about drivers, driver data could be used to see whether drivers are enjoying being Lyft drivers compared to competitors. Ride-hailing services are a two-way street: if demand outweighs supply, than riders might turn elsewhere for transportation. If supply is an issue, then Lyft might be interested in isolating this as a problem.

## Analysis

### Percentage of successful pick ups
Firstly, I wanted to measure the **percentage of successful** pickups. I don't really know the standard of "good" and "bad" in the industry, and so was curious to find out. However, I found that the **successful pick-up rate was 100%**; from just my personal experience, I know that some riders do not get picked up for a variety of reasons, and this led me to believe that this dataset consisted only of successful, complete rides.
<br>
I then wanted to find the mean difference between accepted and requested time across prime-time groups. As mentioned earlier, higher prime-time rate could be associated with higher average time between requests and acceptances, but the reverse could also happen depending on the situation. The truth seemed to lie somewhere in between the two situations.  

<p align = "center">
<img src="https://github.com/kevinchen27/lyft-rides-analysis/blob/master/Pictures/prime%20time%20accept%20request.png" width="500" align = "middle"/>
</p>

A graph of mean difference in times across prime time groups revealed an almost normal distribution, with mean time difference increasing by prime time group until 150 before falling. Interestingly, there were two outliers in the data, with mean time difference for 300 and 400 prime-time groups showing a very high interval. However, on looking at the data, this anomaly was caused primarily by one outlier in each of these groups. For the 350 Prime-Time group, one of observation had a wait time of **1216 seconds**, while the 450 Prime-Time group was skewed by a ride with **422 seconds** between request and acceptance 

### Drivers with least number of completed rides
I made a table of drivers with least number of completed rides in the timespan of this dataset. The mean number of rides completed by drivers during this time was 219. On the other hand, the top 10 drivers in terms of least rides completed had less than 25. Identifying these drivers could be useful for Lyft to zero in on why they might have driven so infrequently and whether or not they left Lyft entirely. This could in turn be used to collect survey data to improve driver experience or initiatives to lure them back to Lyft.

<p align = "center">
<img src="https://github.com/kevinchen27/lyft-rides-analysis/blob/master/Pictures/top%2010.png" width="500" align = "middle"/>
</p>

### Driver behavior
Continuing on the topic of Lyft drivers, I did further analysis to uncover more about Lyft driver behavior during this time frame. 
<br>
<br>
First, I wanted to see if the number of rides were increasing or decreasing with time. I eliminated March since observations start only on March 27. The results did show a favorable trend for Lyft, with a huge spike in rides from April to May. There is a slight decrease from May to June, but this could be attributed to the the dataset ending its time period on June 27. Correlation is not causation, however, and a number of factors could have contributed to this spike (seasonal, events, transport regulations), and the 10,000 ride deficit from May to June is also unlikely to be coverd in the three remaining days in June

<p align = "center">
<img src="https://github.com/kevinchen27/lyft-rides-analysis/blob/master/Pictures/rides%20by%20month.png" width="500" align = "middle"/>
</p>


<br>
A healthier measure of drivers' affinity and happiness with Lyft would be **retention**. Monthly riders is Lyft's North Star metric, and on the driver side, I presume Lyft considers this metric crucial for gauging driver satisfaction. The retention rate from April to June for drivers was **78.06%**, meaning 78% of drivers who completed at least one ride in April completed one ride in June. This amounts to a **churn rate of about 22%**, which does seem quite high to me. 

<p align = "center">
<img src="https://github.com/kevinchen27/lyft-rides-analysis/blob/master/Pictures/drivers%20by%20month.png" width="500" align = "middle"/>
</p>


Indeed, the number of unique drivers spikes to about 800 in May, but dips to below 800 for the month of June. However, as mentioned, although the number of returning drivers decreased, the number of rides increased. One positive is that those current drivers have a larger share of the rides, but fewer drivers could result in deficit of drivers during peak hours. 

## Peak times

Peak ride times could be measured in number of rides per day. To gain a better sense of ride-hailing trends, I calculated the frequency of Lyft rides *by day and per hour of day*. This analysis returned the resulting graph:

<p align = "center">
<img src="https://github.com/kevinchen27/lyft-rides-analysis/blob/master/Pictures/heatmap.png" width="500" align = "middle"/>
</p>


The resulting graph was almost as expected, with peak ride-hailing frequencies for Lyft occurring late at night on weekends and the beginning and end of working hours on weekdays. However, the heat map does suggest that there are slightly more Lyft rides occurring at the end of work than at the beginning of the day. This information could be useful for Lyft for crafting plans to meet demand during peak hours and also possibly to raise demand and supply of rides during other times such as the beginning of a working day when people are going to work (a daily carpool feature, perhaps?).

## Opportunities for further research
While I enjoyed writing this article and conducting analysis, there are still ways to capitalize on this data and do even more with more information. For example:
- Mean distance per prime-time group. This could help pricing strategies for Lyft
- Predicting destination:
  - If location data were available, data mining methods could help predict which destinations are most common according to pick-up location and time of day/day of week. This could help in devising strategies for pricing and also to make sure demand for drivers is met accordingly. 
