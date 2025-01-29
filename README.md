# Spotify Data Analysis Project 
A data-driven way to explore my music journey

![Spotify Dashboard](https://github.com/user-attachments/assets/ae802f86-45fc-4298-a165-85f6681067ff)

## Exploratory Data Analysis (EDA) - Part 1: Python

This project explores my personal Spotify listening data to uncover patterns and insights about my music preferences and habits. Using Python, SQL, and Tableau, I processed raw data, performed analysis, and created visualizations to summarize my listening behavior. Below, I outline the steps taken in the project.

#### A. Data cleaning and integration (Spotify_data_cleaning_integration.ipynb notebook)
To begin, I used Python in Google Colab to process the raw data files provided by Spotify. The data was in JSON format, which I loaded into a Pandas DataFrame for cleaning and transformation. Key steps included:
- Cleaning and Enrichment: Converted timestamps into human-readable dates and extracted useful features like listening time (in minutes), date, hour, and month.
- Spotify API Integration: To enhance the analysis, I used Spotify’s API to match track URIs in my data with additional metadata, such as artist genres. This allowed for deeper exploration of my genre preferences.

#### B. Genre Clustering (Spotify_genres.ipynb)

Once I enriched the data with genre information from Spotify's API, I discovered there were over 900 unique genres. To make sense of this diversity, I applied a K-means clustering algorithm to group them.

Initially, the clustering results were inconclusive. To improve accuracy, I iteratively refined the process by introducing weighted clustering and implementing custom rules.

Here is an overview of these steps:

- **Genre Preprocessing**: Standardized genre names and created a hierarchical mapping of related genres. Here's an example of how genres are preprocessed:

```python
def preprocess_genres(genres):
    # Convert genre strings to lowercase and remove special characters
    cleaned = [re.sub(r'[^\w\s-]', '', g.lower()) for g in genres]
    
    # Map similar genres to standardized names
    genre_mapping = {
        'r&b': 'rnb',
        'rhythm and blues': 'rnb',
        'hip hop': 'hiphop',
        'hip-hop': 'hiphop',
        # ... more mappings
    }
    return [genre_mapping.get(g, g) for g in cleaned]
```

- **Weighted Clustering**: Applied weights to different genres based on their prominence and specificity. Example of the weighting system:

```python
def apply_genre_weights(genres):
    weights = {
        'pop': 1.5,
        'rock': 1.5,
        'hiphop': 1.3,
        'electronic': 1.2,
        'indie': 1.1,
        # More genre weights...
    }
    return [weights.get(genre, 1.0) for genre in genres]
```

- **Custom Rules**: Implemented specific rules to ensure logical grouping of genres. Here's a fragment showing how custom rules are applied:

```python
genre_rules = {
    'hip_hop': ['hip hop', 'rap', 'trap', 'drill', 'grime'],
    'rnb_soul': ['r&b', 'neo soul', 'soul', 'rnb', 'rhythm and blues'],
    'pop_rock': ['pop rock', 'power pop', 'soft rock', 'melodic rock'],
    # More genre groupings...
}

def create_custom_clusters(df, genre_rules):
    for cluster_name, genres in genre_rules.items():
        mask = df['cleaned_genres'].apply(lambda x: any(g in x for g in genres))
        df.loc[mask, 'cluster'] = cluster_name
    return df
```
#### C. Genre Classification (Spotify_genres.ipynb)

I ultimately shifted my approach to classification for more accurate genre identification. The classification model was built using genre mappings and utilized features extracted from the cleaned dataset.

Here are the key components:

1. **Genre Hierarchy**: A dictionary that defines primary genre categories along with their associated sub-genres and priorities. This structure allows for organized classification based on genre similarities.

2. **Cleaning Function** (clean_genres_improved):
- Input: A DataFrame containing a column of genres.
- Process:
    - Cleans and standardizes genre entries.
    - Determines the primary genre for each entry using the defined hierarchy.
    - Applies additional rules to refine classification, such as recognizing specific keywords (e.g., 'children', 'comedy').
- Output: A DataFrame with an added column for improved genre classifications.

3. **Cluster Analysis Function** (analyze_clusters):
- Input: The cleaned DataFrame.
- Process: Analyzes the distribution of genres within each cluster, identifying the top genres and counting the total number of tracks per cluster.
- Output: A summary of genre distribution for each cluster.

Exporting for SQL Analysis: After cleaning and enriching the dataset, I exported the data into CSV files, structured for SQL analysis.

## EDA Part 2 - SQL Analysis
Next, I used SQL in DataCamp’s DataLab to perform the core analysis. I created three tables:
- Streams: Contained detailed listening session data (e.g., track name, artist, time played).
- Tracks: Aggregated total playtime and play counts for each track.
- Artists: Captured artist-level metrics, including total listening time and genre diversity.

Key analyses included:
- Overall Statistics: Calculated total listening time and the number of unique tracks, artists, and albums in my data.
- Top Tracks: Identified the most listened-to songs based on total minutes played.
- Daily Patterns: Explored how my listening habits varied day-to-day.
- Love-Hate Songs: Highlighted tracks with high play counts but frequent skips.
- Artist Diversity: Analyzed how my music taste evolved over time in terms of variety.

Here is a code example of Songs I have a love-hate relationship with: Songs that appear in my top 100 tracks (2024) but that I frequently skipped

```SQL
-- Love-Hate Relationship Songs 2024

-- Temporary table to store additional details
CREATE TEMPORARY TABLE top_tracks_details AS
SELECT 
    track_name,
    artist_name,
    album_name,
    COUNT(*) as total_plays,
    SUM(CASE WHEN skipped THEN 1 ELSE 0 END) as skip_count,
    SUM(minutes_played) as total_minutes
FROM '2023_2024.csv'
WHERE 
    year = 2024 
    AND artist_name NOT LIKE '%ASMR%'
GROUP BY track_name, artist_name, album_name;

-- Select the top 100 tracks based on play count and join with the temporary table to get the final result
SELECT 
    d.track_name,
    d.artist_name,
    d.album_name,
    d.total_plays,
    d.skip_count,
    d.total_minutes
FROM top_tracks_details d
ORDER BY d.total_plays DESC, d.skip_count DESC
LIMIT 101;
```
![SQL 1](https://github.com/user-attachments/assets/a9f31980-267e-481a-ad60-6bf6e9e8ad4a)

## Part 3: Visualization with Tableau
To present the results, I created a dashboard in Tableau that showcases my listening patterns of last year (2024) and adding a few overall statistics about my listening patterns since 2013. After importing the SQL query results into Tableau, I made a template for my dashboard in Figma, uploading it it to Tableau and building the following visualizations:

![Spotify Dashboard](https://github.com/user-attachments/assets/fe8d26a6-edbb-4271-a2e8-0908eb7c72a8)

This visualization presents my 2024 Spotify Wrapped data, showcasing listening habits, favorite artists, genres, and patterns over the year. 

Highlights include:
- **Total Listening**: 4,529 unique tracks, 1,947 artists, and 30,114 minutes on Spotify (I also added an average time I spent playing guitar)
- **Top Artists**: Hozier tops my list with 469.5 minutes played, followed by Fleetwood Mac, Natalia Lafourcade, and ABBA.
- **Favorite Genres**: Pop, rock, folk, and dance dominate my listening habits, visualized as a genre bubble chart.
- **Listening Trends**:
    - Most-played track each month includes hits like "Indigo Night" (January) and "Higher Ground" (November).
    - Songs I frequently skip despite being in my top 100, like "3 Nights" and "Back to Black."
    - Declining favorites, such as "Tamacun" and "Wasting My Young Years."
- **Daily & Quarterly Breakdown**: Peaks in December with 536.6 minutes in a single day and a steady increase in listening from Q1 to Q4.
