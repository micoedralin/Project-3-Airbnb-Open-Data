# Introduction
Provide valuable insights into airbnb prices from different neighborhoods in New York City. I analyzed for the most expensive and cheapest neighborhoods, how reviews affect prices, and other factors that may affect the prices.


# Background
Decided to create this project to showcase ability in data cleaning using power query in Excel, and to find useful insights using SQL in VS Code, and then creating a visualization using Power BI.

### Questions answered in this project
1. What neighborhoods have the highest nightly prices for private rooms in New York City?
2. Which boroughs have the highest nightly prices for listings in New York City?
3. Which neighborhoods in Manhattan is cheapests by nightly prices?
4. How does reviews affect single night prices of listings in a neighborhood like Brooklyn?
5. What are the most expensive neighborhoods in total price (service fee + price) for entire home/apt in New York City?

# Tools used to create this project
- **SQL:** SQL allowed me to query the database and find critical insights needed for the proejct
- **PostgreSQL:** The database management used for handling airbnb data
- **Visual Studio Code:** Used for database management and executing SQL queries
- **Microsoft Excel:** Used to clean data in power query
- **Power BI:** Used to create data visualization to turn insights into visualization
- **Git & Github:** Used for sharing my work including my SQL scripts, analysis, and all neccesary files to show process

# The Analysis
Each query made for this project is aimed to find insights into factors that affect airbnb pricing in different boroughs and neighborhoods across New York City. This particular query answers the first question of highest nightly prices for private rooms in New York City.


```sql
SELECT
    neighborhood,
    borough,
    room_type,
    COUNT(*) AS listing_count,
    ROUND(AVG(price), 2) AS avg_price_per_night,
    PERCENTILE_CONT(.5) WITHIN GROUP (ORDER BY price) AS median_price_per_night
FROM 
    airbnb_data
WHERE
    price IS NOT NULL
    AND price BETWEEN 50 AND 1500
    AND room_type = 'Private room'
    AND minimum_nights = 1
GROUP BY
    borough,
    neighborhood,
    room_type
HAVING
    COUNT(*) >= 50
ORDER BY
    median_price_per_night DESC;
```

Here are some insights I uncovered about this query

![Power BI Image 1](/Power%20BI%20Images/Image_1.PNG)

-  In this airbnb dataset the neighborhood of Ditmars Steinway had the highest median price per night set at $757.5 and also the highest average price per night at $717.52.

- There was not a single borough that we can point to as most expensive according to the results in this particular query, initially I expected Manhattan neighborhoods to dominate the top of the list based on previous knowledge about expensive boroughs of New York City.

- There were some huge outliers in the listings due to the price difference in median and average price per night on some neighborhoods.


# Visualiztion

![Power BI Image 2](/Power%20BI%20Images/Image_2.PNG)

- In this visualization, Bronx and Queens show higher median nightly prices than Manhattan. However, this may be influenced by listing composition (room types, luxury units, or sample size differences), with the Bronx having 705 listings compared to Queens and Manhattan having 3971 and 7980 listings respectively.


![Power BI Image 3](/Power%20BI%20Images/Image_3.PNG)


- This visualization focuses on nightly prices regardless of room type, we can see that Nolita is the cheapest followed by Murray Hill and Theater District. The substantial difference between Median and Average Price is most likely down to Nolita, Murray Hill and Theater District having more private rooms than entire home/apt listings.


![Power BI Image 4](/Power%20BI%20Images/Image_4.PNG)


- We can see that reviews that received a 1 is priced higher than reviews that recevied higher ratings in Brooklyn. From this visualization price appears to play a role in how customers view or rate listings with expensive listings that may not be worth the price receiving lower ratings and listings that are fairly priced or perceived as bargain by customers receving better ratings.


![Power BI Image 5](/Power%20BI%20Images/Image_5.PNG)

- Finally, this result shows the most expensive neighborhoods for entire home/apt in New York City when price and service fee are added together. The result was filtered out to at least 50 listings to have reliable data. Although the query was filtered to entire home/apt, the size of the home and apartment likely reflected the total price since the bigger space the higher the service fee will probably be and the price as well.
   

# What I Learned
- The dataset that I worked with was a fairly messy data in the fact that there were several missing values, incorrect formatting, and inconsistent data in which I had to filter out and piece together important data to get credible insights from. It was a good practice to be able to figure out important data that was needed for this project and eliminate data that was not needed.

- There were outside factors that played a part in the cost of nightly prices that I cannot account for such as some listings closeness to parks and expensive neighborhoods, listings may be located in an area not particularly expensive but very close or on the boundary of an expensive neighborhood such as the case with many East Harlem listings being close to Upper East Side and the Central Park.

- Based on the difference between median and average price on many listings we can assume that there were many luxury apartments or other factors that could've driven up the average cost that we cannot account for based on the data given.
 
# Data Challenges and Solutions
- There were columns such as minimum nights where values were over 365 days and although I fixed it in power query to ensure values weren't over 365 days, it was hard to determine which values were typos and which values were real. The prices were also questionable since there would be rows in which price would be $210 and minimum nights would be 150. To account for this issue, I made sure to filter out the query so that minimum nights were only for 1 night ensuring that the price reflected the 1 night value.

- Some data had small number of listings and I wanted the data to be accurate and reliable so I decided to filter out some queries to at least 50 listings so that the result is big enough to be reliable, this could also even out prices based on the size of the listings since even though we filter out some listings to private rooms and entire home/apt the price may be reflective of the size of the private room and the size of the entire home/apt.

# Conclusion
In creating this project there were many interesting findings that surprised me, including the prices for different neighborhoods. As stated earlier, there were many factors that I cannot account for so it was hard to come to a robust conclusion. Overall, some of the biggest factors that determined prices were listing availability, proximity to parks and transportation, types of rentals such as luxury apartments, the size of rooms or entire home/apt, and location.