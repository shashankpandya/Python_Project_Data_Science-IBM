# Course 10: Applied Data Science Capstone - Complete Study Guide
IBM Data Science Professional Certificate - Course 10 of 12
Learner: Pandya Shashank | Status: In Progress

---

## Course Overview

The Applied Data Science Capstone is the culminating project of the IBM Data Science Professional Certificate.
You apply all skills from Courses 1-9 to solve a real-world business problem:
"Battle of the Neighborhoods" - using location data to recommend where to open a new business venue.

Key technologies: Foursquare Places API, Folium maps, K-Means clustering, Pandas, Plotly

---

## Module 1: Capstone Project Overview and Introduction to Location Data

### The Business Problem
A stakeholder (investor, city planner, entrepreneur) wants to:
- Find the best neighborhood to open a new restaurant, gym, coffee shop, etc.
- Compare neighborhoods across a city based on venue category density
- Cluster similar neighborhoods to find unexplored market opportunities

### Problem Statement Template
"In [CITY], which neighborhoods have the highest concentration of [VENUE TYPE],
and which neighborhoods are underserved, representing the best opportunity
to open a new [BUSINESS TYPE]?"

Example: "In New York City, which borough neighborhoods are ideal for opening
a new Italian restaurant, based on existing venue competition and neighborhood demographics?"

### Location Data Sources
| Source | Type | Notes |
|:---|:---|:---|
| Foursquare Places API | Venue categories, coordinates, ratings | Primary source - free tier available |
| OpenStreetMap | Road networks, boundaries, POIs | Open-source via osmnx library |
| Google Maps API | Geocoding, places, distances | Requires paid API key |
| Wikipedia + BeautifulSoup | Neighborhood lists and postcodes | Scraped for city neighborhood data |
| Canada Post / US Census | ZIP codes, borough data | Administrative boundary data |

### Geopy - Geocoding Library
`python
# pip install geopy
from geopy.geocoders import Nominatim

# Convert address to lat/lon (geocoding)
geolocator = Nominatim(user_agent="ds_capstone_app")
location = geolocator.geocode("Manhattan, New York City, NY")
print(location.address)
print(location.latitude, location.longitude)  # 40.7831, -73.9712

# Convert lat/lon back to address (reverse geocoding)
rev = geolocator.reverse("40.7831, -73.9712")
print(rev.address)
`

---

## Module 2: Foursquare API - Location Data Extraction

### Foursquare Places API Setup
`python
# Sign up at https://developer.foursquare.com/
# Create a project to get CLIENT_ID and CLIENT_SECRET

CLIENT_ID     = 'YOUR_FOURSQUARE_CLIENT_ID'
CLIENT_SECRET = 'YOUR_FOURSQUARE_CLIENT_SECRET'
VERSION       = '20230101'   # YYYYMMDD format - API version date
LIMIT         = 100          # Max venues per request
`

### Search Venues Near a Location
`python
import requests
import pandas as pd
import json

def get_venues_near(lat, lon, radius=500, limit=100):
    """
    Search for venues within radius meters of (lat, lon).
    Returns a Pandas DataFrame with venue details.
    
    Args:
        lat    (float): Latitude of center point
        lon    (float): Longitude of center point
        radius (int):   Search radius in meters (max 100000)
        limit  (int):   Max number of venues to return (max 50 for free tier)
    """
    url = f"https://api.foursquare.com/v2/venues/explore"
    params = {
        'client_id':     CLIENT_ID,
        'client_secret': CLIENT_SECRET,
        'v':             VERSION,
        'll':            f"{lat},{lon}",
        'radius':        radius,
        'limit':         limit
    }
    results = requests.get(url=url, params=params).json()
    
    # Extract venue items from nested JSON response
    venues = results['response']['groups'][0]['items']
    
    # Flatten nested JSON into a DataFrame
    return pd.json_normalize(venues, sep='_')

# Example usage
df_venues = get_venues_near(lat=40.7831, lon=-73.9712, radius=500)
print(df_venues.shape)
print(df_venues.columns.tolist())
`

### Clean and Extract Key Venue Fields
`python
def get_venues_cleaned(neighborhoods_df, radius=500, limit=100):
    """
    For each neighborhood row in neighborhoods_df,
    retrieve nearby venues and combine into one DataFrame.
    
    Expected columns in neighborhoods_df:
        'Neighborhood', 'Borough', 'Latitude', 'Longitude'
    """
    venues_list = []
    
    for idx, row in neighborhoods_df.iterrows():
        print(f"Fetching venues for: {row['Neighborhood']}")
        
        url = "https://api.foursquare.com/v2/venues/explore"
        params = {
            'client_id':     CLIENT_ID,
            'client_secret': CLIENT_SECRET,
            'v':             VERSION,
            'll':            f"{row['Latitude']},{row['Longitude']}",
            'radius':        radius,
            'limit':         limit
        }
        results = requests.get(url=url, params=params).json()
        
        try:
            venues = results['response']['groups'][0]['items']
            for v in venues:
                venues_list.append([
                    row['Neighborhood'],
                    row['Borough'],
                    row['Latitude'],
                    row['Longitude'],
                    v['venue']['name'],
                    v['venue']['location']['lat'],
                    v['venue']['location']['lng'],
                    v['venue']['categories'][0]['name']
                ])
        except (KeyError, IndexError):
            pass  # Skip neighborhoods with no venue data
    
    return pd.DataFrame(venues_list, columns=[
        'Neighborhood', 'Borough',
        'Neighborhood Latitude', 'Neighborhood Longitude',
        'Venue', 'Venue Latitude', 'Venue Longitude',
        'Venue Category'
    ])

# Fetch venues for all NYC neighborhoods
nyc_venues = get_venues_cleaned(neighborhoods_df, radius=500, limit=100)
print(f"Total venues fetched: {nyc_venues.shape[0]}")
print(f"Unique venue categories: {nyc_venues['Venue Category'].nunique()}")
`

### Foursquare V3 API (Newer Syntax - Free Places API)
`python
import requests
import json

# V3 API uses API Key header instead of client_id/secret
API_KEY = 'YOUR_FOURSQUARE_API_KEY_V3'

def search_venues_v3(lat, lon, query='', radius=500, limit=50):
    url = "https://api.foursquare.com/v3/places/search"
    headers = {
        "Accept": "application/json",
        "Authorization": API_KEY
    }
    params = {
        "ll": f"{lat},{lon}",
        "radius": radius,
        "limit": limit,
        "query": query
    }
    response = requests.get(url, headers=headers, params=params)
    data = response.json()
    return data.get("results", [])

# Get coffee shops near Times Square
results = search_venues_v3(lat=40.758, lon=-73.985, query="coffee", radius=500)
for place in results[:5]:
    print(place["name"], "|", place["categories"][0]["name"] if place.get("categories") else "N/A")
`

---

## Module 3: Neighborhood Segmentation and Clustering

### Step 1 - Scrape Neighborhood Data (NYC Example using Wikipedia)
`python
import requests
import pandas as pd
from bs4 import BeautifulSoup

# NYC neighborhoods by borough from Wikipedia
url = "https://en.wikipedia.org/wiki/List_of_neighborhoods_in_New_York_City"
response = requests.get(url)
soup = BeautifulSoup(response.content, "html.parser")

# Extract neighborhood table(s)
tables = pd.read_html(url)
# Usually the first or second table has borough + neighborhood columns
neighborhoods_raw = tables[0]
print(neighborhoods_raw.head())
print(neighborhoods_raw.columns.tolist())
`

### Step 2 - Get Coordinates for Each Neighborhood
`python
from geopy.geocoders import Nominatim
import time

geolocator = Nominatim(user_agent="capstone_ibm")

def get_coordinates(neighborhood, city="New York City, NY"):
    try:
        location = geolocator.geocode(f"{neighborhood}, {city}")
        if location:
            return location.latitude, location.longitude
        return None, None
    except Exception:
        return None, None

# Apply geocoding with a delay to avoid rate limiting
neighborhoods_df['Latitude'] = None
neighborhoods_df['Longitude'] = None

for idx, row in neighborhoods_df.iterrows():
    lat, lon = get_coordinates(row['Neighborhood'])
    neighborhoods_df.at[idx, 'Latitude'] = lat
    neighborhoods_df.at[idx, 'Longitude'] = lon
    time.sleep(1)   # 1 second delay between requests

# Drop rows with no coordinates
neighborhoods_df.dropna(subset=['Latitude', 'Longitude'], inplace=True)
print(f"Neighborhoods with coordinates: {len(neighborhoods_df)}")
`

### Step 3 - One-Hot Encode Venue Categories
`python
# Get venue category dummies (one-hot encoding)
# One row per neighborhood, one column per venue category
onehot = pd.get_dummies(nyc_venues[['Neighborhood', 'Venue Category']],
                         columns=['Venue Category'])

# Group by neighborhood: mean frequency of each venue category
neighborhood_grouped = onehot.groupby('Neighborhood').mean().reset_index()

print(f"Shape: {neighborhood_grouped.shape}")  # (n_neighborhoods, n_categories + 1)
print(neighborhood_grouped.head(2))
`

### Step 4 - Top N Most Common Venue Categories per Neighborhood
`python
def return_most_common_venues(row, num_top_venues=10):
    """Return the top N venue categories (sorted by frequency) for a neighborhood row."""
    row_categories = row.iloc[1:]   # Skip 'Neighborhood' column
    row_categories_sorted = row_categories.sort_values(ascending=False)
    return row_categories_sorted.index.values[0:num_top_venues]

# Build DataFrame with top venues per neighborhood
num_top_venues = 10
columns = ['Neighborhood'] + [f'Top {i+1} Venue' for i in range(num_top_venues)]

indicators = pd.DataFrame(columns=columns)
for idx, row in neighborhood_grouped.iterrows():
    indicators = pd.concat([indicators, pd.DataFrame(
        data=[[row['Neighborhood']] + list(return_most_common_venues(row, num_top_venues))],
        columns=columns
    )], ignore_index=True)

print(indicators.head(5))
`

### Step 5 - K-Means Clustering of Neighborhoods
`python
from sklearn.cluster import KMeans
import matplotlib.cm as cm
import matplotlib.colors as colors
import numpy as np

# Drop the Neighborhood name column before clustering
X = neighborhood_grouped.drop('Neighborhood', axis=1)

# Choose K (typically 5 for neighborhood segmentation)
k_clusters = 5
kmeans = KMeans(n_clusters=k_clusters, random_state=0, n_init=10)
kmeans.fit(X)

# Add cluster labels back to neighborhoods DataFrame
neighborhood_grouped.insert(0, 'Cluster Labels', kmeans.labels_)

# Merge with original neighborhood data (coordinates + borough)
nyc_merged = neighborhoods_df.copy()
nyc_merged = nyc_merged.join(neighborhood_grouped.set_index('Neighborhood'), on='Neighborhood')
nyc_merged.head()
`

### Step 6 - Visualize Clusters on Folium Map
`python
import folium

# NYC center coordinates
NYC_LAT, NYC_LON = 40.7128, -74.0060

# Create base map
map_clusters = folium.Map(location=[NYC_LAT, NYC_LON], zoom_start=11)

# Color palette for K clusters
x = np.arange(k_clusters)
ys = [i + x + (i * x) ** 2 for i in range(k_clusters)]
colors_array = cm.rainbow(np.linspace(0, 1, len(ys)))
rainbow = [colors.rgb2hex(i) for i in colors_array]

# Add circle markers for each neighborhood
for lat, lon, poi, cluster in zip(
    nyc_merged['Latitude'],
    nyc_merged['Longitude'],
    nyc_merged['Neighborhood'],
    nyc_merged['Cluster Labels']
):
    label = folium.Popup(f"<b>{poi}</b><br>Cluster {cluster}", parse_html=True)
    folium.CircleMarker(
        [lat, lon],
        radius=5,
        popup=label,
        color=rainbow[cluster - 1],
        fill=True,
        fill_color=rainbow[cluster - 1],
        fill_opacity=0.7
    ).add_to(map_clusters)

map_clusters.save("nyc_clusters.html")
map_clusters
`

### Step 7 - Analyze Each Cluster
`python
# Examine cluster contents and top venues

def analyze_cluster(cluster_id):
    """Print all neighborhoods in a cluster with their top venues."""
    cluster_df = nyc_merged[nyc_merged['Cluster Labels'] == cluster_id]
    print(f"\n=== CLUSTER {cluster_id} ({len(cluster_df)} neighborhoods) ===")
    print(cluster_df[['Neighborhood', 'Borough', 'Top 1 Venue', 'Top 2 Venue', 'Top 3 Venue']].to_string(index=False))

for i in range(k_clusters):
    analyze_cluster(i)
`

---

## Module 4: Capstone Final Report and Presentation

### Required Report Sections

1. Introduction / Business Problem (10 points)
   - Clearly describe the problem and the target audience
   - Explain why this problem is worth solving
   - State specific business/research questions

2. Data (10 points)
   - Describe all data sources used (Wikipedia, Foursquare API, etc.)
   - Explain how data was collected and processed
   - Justify why this data is appropriate for the problem

3. Methodology (30 points)
   - Exploratory Data Analysis (EDA): Describe the data exploration process
   - Inferential Statistical Analysis: Hypotheses and statistical tests used
   - Machine Learning: Which algorithms were applied and why
   - Step-by-step description of the analytical workflow

4. Results (15 points)
   - Present and describe all visualizations
   - Show clustering results and maps
   - Report quantitative findings

5. Discussion (10 points)
   - Interpret the results in the context of the business problem
   - Compare clusters and identify the best neighborhoods
   - Acknowledge limitations and potential biases

6. Conclusion (15 points)
   - Summarize key findings
   - Recommend the best neighborhood(s) for the business
   - Suggest future work or improvements

7. References (10 points)
   - All data sources, libraries, and papers cited

### Report Grading Rubric
| Section | Points |
|:---|:---:|
| Introduction / Business Problem | 10 |
| Data Description | 10 |
| Methodology (EDA + ML approach) | 30 |
| Results | 15 |
| Discussion | 10 |
| Conclusion | 15 |
| References | 10 |
| **Total** | **100** |

### Project Notebook Structure (Template)
`python
# Cell 1: Title and Introduction
# Title: Battle of the Neighborhoods - Finding the Best Location for a New Restaurant in Toronto

# Cell 2: Import Libraries
import pandas as pd
import numpy as np
import json
import requests
import folium
from bs4 import BeautifulSoup
from sklearn.cluster import KMeans
from geopy.geocoders import Nominatim
import matplotlib.cm as cm
import matplotlib.colors as colors
import matplotlib.pyplot as plt
import seaborn as sns

print("Libraries imported successfully!")

# Cell 3: Define API Credentials
CLIENT_ID     = 'YOUR_CLIENT_ID'
CLIENT_SECRET = 'YOUR_CLIENT_SECRET'
VERSION       = '20230101'
LIMIT         = 100

# Cell 4: Data Collection - Scrape Neighborhood Data
# [Wikipedia scraping code]

# Cell 5: Data Wrangling - Geocode Neighborhoods
# [Geopy geocoding code]

# Cell 6: Create Base Map
# [Initial Folium map]

# Cell 7: Foursquare API - Collect Venue Data
# [get_venues_cleaned() function + execution]

# Cell 8: EDA - Explore Venue Categories
print(f"Total unique venues: {nyc_venues['Venue'].nunique()}")
print(f"Total unique categories: {nyc_venues['Venue Category'].nunique()}")
nyc_venues.groupby('Borough')['Venue'].count().plot(kind='bar', figsize=(10,5))

# Cell 9: One-Hot Encode Venue Categories
# [get_dummies + groupby mean]

# Cell 10: Top N Venues per Neighborhood
# [return_most_common_venues function]

# Cell 11: K-Means Clustering
# [KMeans fit + label assignment]

# Cell 12: Cluster Map Visualization
# [Folium cluster map]

# Cell 13: Analyze Clusters
# [Per-cluster neighborhood analysis]

# Cell 14: Conclusions and Recommendations
# [Final business recommendation]
`

---

## Toronto Capstone Variant (Classic IBM Version)

### Step 1 - Scrape Toronto Postal Code Data from Wikipedia
`python
import requests
from bs4 import BeautifulSoup
import pandas as pd

url = "https://en.wikipedia.org/wiki/List_of_postal_codes_of_Canada:_M"
page = requests.get(url)
soup = BeautifulSoup(page.content, "html.parser")

# Get all tables
tables = pd.read_html(url)
toronto_raw = tables[0]

# Clean: only keep rows where Borough is not "Not assigned"
toronto_df = toronto_raw[toronto_raw['Borough'] != 'Not assigned'].copy()
toronto_df.reset_index(drop=True, inplace=True)

# If Neighbourhood is "Not assigned", use the Borough name
toronto_df['Neighbourhood'] = toronto_df.apply(
    lambda row: row['Borough'] if row['Neighbourhood'] == 'Not assigned' else row['Neighbourhood'],
    axis=1
)

# For same postal code - combine neighbourhoods into one row
toronto_grouped = toronto_df.groupby(['Postal Code', 'Borough'])['Neighbourhood'].apply(
    lambda x: ', '.join(x)
).reset_index()

print(f"Shape: {toronto_grouped.shape}")  # Should be (103, 3)
print(toronto_grouped.head())
`

### Step 2 - Add Coordinates from CSV
`python
# IBM provides a CSV with lat/lon for each postal code
geo_url = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DS0701EN-SkillsNetwork/labs_v1/Geospatial_Coordinates.csv"
geo_df = pd.read_csv(geo_url)
geo_df.columns = ['Postal Code', 'Latitude', 'Longitude']

# Merge coordinates into toronto_grouped
toronto_complete = pd.merge(toronto_grouped, geo_df, on='Postal Code')
print(toronto_complete.shape)  # Should be (103, 5)
print(toronto_complete.head())
`

### Step 3 - Create Toronto Map with Folium
`python
import folium

# Toronto center coordinates
toronto_lat = 43.6532
toronto_lon = -79.3832

toronto_map = folium.Map(location=[toronto_lat, toronto_lon], zoom_start=11)

for lat, lng, borough, neighbourhood in zip(
    toronto_complete['Latitude'],
    toronto_complete['Longitude'],
    toronto_complete['Borough'],
    toronto_complete['Neighbourhood']
):
    label = f"{neighbourhood}, {borough}"
    label = folium.Popup(label, parse_html=True)
    folium.CircleMarker(
        [lat, lng],
        radius=5,
        popup=label,
        color='blue',
        fill=True,
        fill_color='#3186cc',
        fill_opacity=0.7
    ).add_to(toronto_map)

toronto_map.save("toronto_neighborhoods.html")
toronto_map
`

### Step 4 - Focus on Toronto Downtown
`python
# Filter only downtown Toronto boroughs
downtown_toronto = toronto_complete[toronto_complete['Borough'].str.contains('Toronto')].reset_index(drop=True)
print(f"Downtown Toronto neighborhoods: {len(downtown_toronto)}")

# Create focused map
downtown_map = folium.Map(location=[toronto_lat, toronto_lon], zoom_start=12)

for lat, lng, borough, neighbourhood in zip(
    downtown_toronto['Latitude'],
    downtown_toronto['Longitude'],
    downtown_toronto['Borough'],
    downtown_toronto['Neighbourhood']
):
    folium.CircleMarker(
        [lat, lng],
        radius=5,
        popup=f"{neighbourhood}",
        color='red',
        fill=True,
        fill_opacity=0.7
    ).add_to(downtown_map)

downtown_map
`

---

## Key Libraries Quick Reference for the Capstone

### Geopy Quick Reference
`python
from geopy.geocoders import Nominatim
geolocator = Nominatim(user_agent="my_app")

# Forward geocoding (address -> lat/lon)
loc = geolocator.geocode("Empire State Building, New York")
lat, lon = loc.latitude, loc.longitude

# Reverse geocoding (lat/lon -> address)
addr = geolocator.reverse(f"{lat}, {lon}")
print(addr.address)
`

### Folium Quick Reference
`python
import folium

# Base map
m = folium.Map(location=[lat, lon], zoom_start=12, tiles='CartoDB positron')

# Circle marker
folium.CircleMarker([lat, lon], radius=8, color='red', fill=True,
                    fill_opacity=0.7, popup='Venue Name').add_to(m)

# Regular marker with icon
folium.Marker([lat, lon], popup='Label',
              icon=folium.Icon(color='blue', icon='star')).add_to(m)

# Choropleth
folium.Choropleth(geo_data=geo_json, data=df,
                  columns=['key', 'value'], key_on='feature.properties.key',
                  fill_color='YlOrRd', legend_name='Legend').add_to(m)

# Save or display
m.save('map.html')
m   # Displays inline in Jupyter
`

### Foursquare API Response Structure
`
response
  -> groups
    -> [0] (group "recommends")
      -> items
        -> [0] (venue item)
          -> venue
            -> id: "abc123"
            -> name: "Blue Bottle Coffee"
            -> location
              -> lat: 40.7128
              -> lng: -74.0060
              -> address: "123 Main St"
              -> city: "New York"
            -> categories
              -> [0]
                -> id: "4bf58dd8d48988d1e0931735"
                -> name: "Coffee Shop"
                -> shortName: "Coffee Shop"
`

---

## Assessment Structure

| Assessment | Weight | Description |
|:---|:---:|:---|
| Peer-Reviewed Assignment 1 | 20% | Segmentation Notebook (Toronto/NYC scraping + map) |
| Peer-Reviewed Assignment 2 | 20% | Foursquare API + Venue Exploration Notebook |
| Peer-Reviewed Assignment 3 | 20% | Clustering Notebook (K-Means + cluster analysis) |
| Peer-Reviewed Final Report | 40% | Full capstone report (Introduction to Conclusion) |
| **Total** | **100%** | **Peer-reviewed by 3 classmates** |

---
IBM Data Science Professional Certificate - Course 10 - Pandya Shashank
