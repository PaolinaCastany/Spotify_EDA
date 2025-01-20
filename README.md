### Spotify Data Analysis Project 

# Exploratory Data Analysis (EDA)

This project explores my personal Spotify listening data to uncover patterns and insights about my music preferences and habits. Using Python, SQL, and Tableau, I processed raw data, performed analysis, and created visualizations to summarize my listening behavior. Below, I outline the steps taken in the project.

Data Processing with Python
To begin, I used Python in Google Colab to process the raw data files provided by Spotify. The data was in JSON format, which I loaded into a Pandas DataFrame for cleaning and transformation. Key steps included:

Cleaning and Enrichment: Converted timestamps into human-readable dates and extracted useful features like listening time (in minutes), date, hour, and month.
Spotify API Integration: To enhance the analysis, I used Spotify’s API to match track URIs in my data with additional metadata, such as artist genres. This allowed for deeper exploration of my genre preferences.
Exporting for SQL Analysis: After cleaning and enriching the dataset, I exported the data into CSV files, structured for SQL analysis.
SQL Analysis
Next, I used SQL in DataCamp’s DataLab to perform the core analysis. I created three tables:

Streams: Contained detailed listening session data (e.g., track name, artist, time played).
Tracks: Aggregated total playtime and play counts for each track.
Artists: Captured artist-level metrics, including total listening time and genre diversity.
Key analyses included:

Overall Statistics: Calculated total listening time and the number of unique tracks, artists, and albums in my data.
Top Tracks: Identified the most listened-to songs based on total minutes played.
Daily Patterns: Explored how my listening habits varied day-to-day.
Love-Hate Songs: Highlighted tracks with high play counts but frequent skips.
Artist Diversity: Analyzed how my music taste evolved over time in terms of variety.
The results were exported as CSV files for visualization.

Visualization with Tableau
To present the results, I created a dashboard in Tableau that showcases my listening patterns. After importing the SQL query results into Tableau, I built the following visualizations:

Top Tracks: A bar chart displaying my most played songs by total listening time.
Daily Trends: A line chart showing changes in listening behavior over time.
Love-Hate Relationship Songs: A dual-axis chart comparing the number of plays and skips for specific tracks.
Artist Diversity: A line chart illustrating the number of unique artists I listened to monthly.
