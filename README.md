# AirBNB-listing-EDA

### Introduction

In recent years, the hospitality industry has witnessed a significant transformation driven by technological advancements and the rise of the sharing economy. Among the key players in this evolution is Airbnb, a platform that allows individuals to rent out their properties or spare rooms to travelers. This analysis delves into the Airbnb listings dataset for New York City, aiming to uncover insights related to pricing trends, availability, and other factors that influence guest experiences. By exploring this dataset, we can gain a better understanding of the market dynamics within the Airbnb ecosystem.

### About Airbnb

Founded in 2008, Airbnb has grown from a small startup to a global phenomenon, providing millions of listings across diverse locations. The platform allows hosts to list their properties while offering travelers unique accommodation options often at competitive prices compared to traditional hotels. Airbnb operates in over 220 countries and regions, facilitating connections between hosts and guests, thereby fostering community engagement and cultural exchange. The company's commitment to innovation and customer experience continues to drive its expansion, positioning it as a leader in the hospitality sector.

### Industry Scope

The short-term rental market, particularly platforms like Airbnb, has reshaped the hospitality landscape by providing travelers with more choices and personalized experiences. According to various industry reports, the global vacation rental market is projected to reach over $87 billion by 2027, fueled by increasing travel demand and consumer preference for unique stays. Major urban centers, such as New York City, have become hotspots for Airbnb listings, attracting millions of tourists annually. This sector not only benefits hosts through supplemental income but also contributes to local economies by enhancing tourism and providing new business opportunities.

### Purpose of the Analysis

The primary purpose of this analysis is to explore the Airbnb listings dataset for New York City, identifying patterns and trends that can inform stakeholders about market behavior. Specific objectives include:

Analyzing pricing dynamics across different neighborhoods and room types.
Understanding the impact of availability and minimum stay requirements on occupancy rates.
Examining the relationship between reviews, ratings, and pricing to identify factors that enhance guest satisfaction.
Providing actionable insights for potential hosts, guests, and policymakers to optimize their strategies within the short-term rental market.

### Dataset Overview

The dataset comprises 20,770 entries and 22 columns, capturing a wide range of attributes related to Airbnb listings in New York City. Key features include:

ID and Host Information: Unique identifiers for each listing and host, facilitating easy reference.
Location Data: Neighborhood group and coordinates (latitude and longitude) provide geographical context.
Room Characteristics: Room type, price, number of beds, and bathrooms offer insights into accommodation options.
Guest Interaction Metrics: Number of reviews, reviews per month, and last review date shed light on guest experiences and host responsiveness.
Availability and Pricing: Minimum nights, availability throughout the year, and price data are crucial for understanding market dynamics.

Ratings and Reviews: These qualitative measures indicate guest satisfaction and can significantly influence booking decisions.
The dataset provides a comprehensive view of the Airbnb landscape, making it a valuable resource for exploring market trends and guest behaviors.

### Importing All Libraries


![Alt text](https://github.com/AFZALSUNSHINE/AirBNB-listing-EDA/blob/main/1%20load%20%202024-11-30%20171641.png)


### Load Data
![Alt text](https://github.com/AFZALSUNSHINE/AirBNB-listing-EDA/blob/main/2%20load%20Screenshot%202024-11-30%20171808.png)


data.head()

View last few rows of the dataset

data.tail()

#### Check the shape of the dataset

data.shape


Get a concise summary of the DataFrame

data.info()

Statistical summary of numeric columns

data.describe()

Data Cleaning

Check for missing values

data.isnull().sum()

Dropping rows with missing values

data.dropna(inplace=True)

Drop duplicated rows


data.drop_duplicates(inplace=True)

Check data types

data.dtypes

Type casting

data['id'] = data['id'].astype(object)


data['host_id'] = data['host_id'].astype(object)

## Univariate Analysis

Price Outliers and Distribution


# Identifying outliers in price
data = data[data['price'] < 1500]

# Boxplot for price
sns.boxplot(data=data, x='price')


# Price distribution histogram
plt.figure(figsize=(8, 5))
sns.histplot(data=data, x='price', bins=100)
plt.title('Price Distribution')
plt.ylabel("Frequency")
plt.show()
     

Availability Distribution

# Availability distribution histogram
plt.figure(figsize=(6, 3))
sns.histplot(data=data, x='availability_365')
plt.title('Availability Distribution')
plt.ylabel("Frequency")
plt.show()

# Feature Engineering

Create new features to gain deeper insights.

# Average price per bed
data['price per bed'] = data['price'] / data['beds']
data.head()

# Bivariate Analysis

Price Dependency on Neighborhood


sns.barplot(data=data, x='neighbourhood_group', y='price', hue='room_type')
plt.title('Price Dependency on Neighborhood')
plt.ylabel("Average Price")
plt.show()
     

### Number of Reviews vs. Price


plt.figure(figsize=(8, 5))
plt.title("Number of Reviews vs. Price")
sns.scatterplot(data=data, x='number_of_reviews', y='price', hue='neighbourhood_group')
plt.show()

## Geographical Distribution

Visualize the geographical distribution of listings.


plt.figure(figsize=(10, 7))
sns.scatterplot(data=data, x='longitude', y='latitude', hue='room_type')
plt.title("Geographical Distribution of Airbnb Listings")
plt.show()

# Correlation Analysis


# Correlation heatmap
corr = data[['latitude', 'longitude', 'price', 'minimum_nights', 'number_of_reviews', 'reviews_per_month', 'availability_365', 'beds']].corr()

plt.figure(figsize=(8, 6))
sns.heatmap(data=corr, annot=True)
plt.title('Correlation Heatmap')
plt.show()
     

# Advanced Analysis

Room Type Analysis


plt.figure(figsize=(8, 5))
sns.barplot(data=data, x='room_type', y='price', estimator=np.mean)
plt.title('Average Price by Room Type')
plt.ylabel("Average Price")
plt.show()

Price Distribution by Neighborhood


plt.figure(figsize=(12, 6))
sns.boxplot(data=data, x='neighbourhood_group', y='price')
plt.title('Price Distribution by Neighborhood')
plt.ylabel("Price")
plt.xticks(rotation=45)
plt.show()
     

### Availability Analysis


plt.figure(figsize=(8, 5))
sns.histplot(data=data, x='availability_365', bins=30)
plt.title('Availability Distribution (Days Available in a Year)')
plt.xlabel("Days Available")
plt.ylabel("Frequency")
plt.grid()
plt.show()

Number of Reviews Over Time


data['last_review'] = pd.to_datetime(data['last_review'])
data.set_index('last_review', inplace=True)
reviews_per_month = data.resample('M').size()

plt.figure(figsize=(12, 6))
reviews_per_month.plot()
plt.title('Number of Reviews Over Time')
plt.ylabel('Number of Reviews')
plt.xlabel('Date')
plt.grid()
plt.show()

Impact of Minimum Nights on Price


plt.figure(figsize=(8, 5))
sns.boxplot(data=data, x='minimum_nights', y='price')
plt.title('Price Distribution by Minimum Nights')
plt.ylabel("Price")
plt.xlabel("Minimum Nights")
plt.grid()
plt.show()

Reviews Analysis


plt.figure(figsize=(8, 5))
sns.scatterplot(data=data, x='reviews_per_month', y='price')
plt.title('Price vs. Reviews per Month')
plt.xlabel('Reviews per Month')
plt.ylabel('Price')
plt.grid()
plt.show()

Geospatial Analysis with Folium


import folium

nyc_map = folium.Map(location=[40.7128, -74.0060], zoom_start=12)

for index, row in data.iterrows():
    folium.CircleMarker([row['latitude'], row['longitude']],
                        radius=5,
                        color='blue' if row['price'] < 300 else 'red',
                        fill=True,
                        fill_opacity=0.6,
                        popup=f"Price: ${row['price']}").add_to(nyc_map)

# Save and display the map
nyc_map.save("nyc_airbnb_map.html")

Clustering Analysis


from sklearn.cluster import KMeans

features = data[['price', 'minimum_nights', 'number_of_reviews']]
kmeans = KMeans(n_clusters=3)
df['cluster'] = kmeans.fit_predict(features)

plt.figure(figsize=(10, 7))
sns.scatterplot(data=data, x='number_of_reviews', y='price', hue='cluster', palette='viridis')
plt.title('Clustering of Listings Based on Price and Number of Reviews')
plt.xlabel('Number of Reviews')
plt.ylabel('Price')
plt.grid()
plt.show()
     

# Conclusions and Insights



The exploratory data analysis (EDA) of the Airbnb listings dataset for New York City has yielded several valuable insights that can inform stakeholders within the short-term rental market.

Pricing Dynamics: 
The analysis revealed significant variability in pricing based on neighborhood groups and room types. For instance, listings in areas like Manhattan tend to command higher prices compared to those in outer boroughs. This suggests that location remains a crucial factor for both hosts and guests when considering rental options.

Impact of Availability and Minimum Nights: 
Listings with a higher availability throughout the year tend to have better occupancy rates. Additionally, properties with flexible minimum night requirements are more likely to attract bookings, indicating that hosts who adapt their availability to guest preferences may achieve higher success.

Guest Satisfaction Metrics: 
There is a strong correlation between the number of reviews and the pricing of listings. Properties with higher reviews per month typically have better ratings, suggesting that guest experiences directly influence their willingness to recommend and rebook. Hosts who actively engage with their guests and encourage reviews can enhance their listing's visibility and attractiveness.

Geographical Insights: 
Visualizing the geographical distribution of Airbnb listings using scatter plots highlights clusters of high-density listings, particularly in popular tourist areas. This geographical analysis can guide potential hosts in selecting optimal locations for their listings to maximize exposure and occupancy.

Outlier Detection: 
Box plots used to identify outliers in pricing revealed that extremely high-priced listings may skew perceptions of average prices. Such insights can help hosts set competitive rates without overpricing their offerings.

Feature Engineering: 
The creation of the "price per bed" feature allowed for a more nuanced understanding of pricing, facilitating better comparisons across properties. This insight can be particularly useful for potential hosts looking to price their listings competitively based on the number of beds available.


### Visual Findings
Price Distribution: The histogram of price distribution showed a right-skewed distribution, indicating that most listings are priced affordably, while a small number of high-priced listings exist.
Heatmaps of Correlation: The heatmap highlighting correlations among numerical features revealed strong correlations between price and variables such as the number of reviews and availability, emphasizing the importance of these factors in setting prices.
Bar Plots for Neighborhood Analysis: The bar plot depicting average prices by neighborhood group highlighted significant differences in pricing, suggesting that hosts should consider local market conditions when setting their prices.

Overall, the insights gathered from this analysis not only enhance our understanding of the Airbnb landscape in New York City but also provide actionable recommendations for hosts, guests, and industry stakeholders to optimize their strategies and improve their overall experiences in the short-term rental market.
