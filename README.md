# Python: Exploratory Data Analysis on Spotify 2023 Dataset

Created by: **Custodio, Louise Angela G.** from **2ECE-D** 

Date and Time Started: **October 23, 2024, 10:15 AM**  

Date and Time Ended: **October 30, 2024, 3:08 AM**

## Brief Description
**Exploratory Data Analysis (EDA)** is a process of analyzing and summarizing datasets to identify patterns, trends, and relationships using visual and statistical methods. It helps in understanding the data's structure and guiding further analysis.

This repository contains a comprehensive exploratory data analysis (EDA) of the Most Streamed Spotify Songs 2023 dataset, sourced from Kaggle. The analysis aims to visualize and interpret key features of popular tracks to extract meaningful insights about music trends and attributes. 

#### Key Objectives:
- **Dataset Overview**: Familiarize with the dataset structure, check for missing values, and identify data types.
- **Summary Statistics**: Provide metrics on streams, release dates, and musical attributes like BPM and danceability.
- **Visualizations**: Create well-labeled visualizations (bar charts, histograms, scatter plots) to uncover trends and patterns in the data.
- **Correlation Analysis**: Investigate relationships between streams and musical characteristics, and explore correlations between attributes like danceability and energy.
- **Insights & Recommendations**: Offer insights into what makes tracks popular, including patterns in artist frequency and platform preferences.

## Guide Questions
- Before anything else, importing the needed library is the first thing we need to do for the code to run smoothly.

      # Importing the Pandas for data manipulation 
      import pandas as pd
    
      # Importing the Matplotlib and Seaborn for visualization
      import matplotlib.pyplot as plt
      import seaborn as sns
To start, we import essential libraries: Pandas for data manipulation, allowing easy handling of datasets, and Matplotlib with Seaborn for creating clear and visually appealing charts. These ensure smooth data processing and insightful visualizations. Together, these libraries make it possible to explore data thoroughly and present insights effectively.

- Next is loading the data from the csv file from Kaggle.

      # Load the data from the CSV file and print the results
      spotify_data = pd.read_csv('spotify-2023.csv', encoding='ISO-8859-1')
      spotify_data

![image](https://github.com/user-attachments/assets/7ed14d3e-6d73-40c2-b184-a53b390a81a9)
The next step involves loading the dataset from a CSV file obtained from Kaggle. Using pd.read_csv('spotify-2023.csv', encoding='ISO-8859-1'), the data is read into a Pandas DataFrame, allowing for easy organization and manipulation in a table-like structure. The specified encoding, ISO-8859-1, ensures special characters are correctly interpreted, which is sometimes necessary for datasets with non-standard characters. Displaying spotify_data confirms successful loading and provides a quick preview of the data’s structure, making it ready for further analysis and visualization.

### Overview of Dataset
- How many rows and columns does the dataset contain?

      rows, cols = spotify_data.shape

      print('Rows:', rows)
      print('Columns:', cols)
  
- What are the data types of each column? Are there any missing values?

      data_types = spotify_data.dtypes

      print("Data Types of Each Column:")
      print(data_types)

      missing_values = spotify_data.isnull().sum()

      print("\nMissing Values in Each Column:")
      print(missing_values[missing_values > 0])

### Basic Descriptive Statistics
- What are the mean, median, and standard deviation of the streams column?

      spotify_data['streams'] = pd.to_numeric(spotify_data['streams'], errors='coerce')
      spotify_data.dropna(subset=['streams'], inplace=True)

      spd_mean = spotify_data['streams'].mean()
      spd_mid = spotify_data['streams'].median()
      spd_sd = spotify_data['streams'].std()

      print('Mean of the Streams:', spd_mean)
      print('Median of the Streams:', spd_mid)
      print('Standard Deviation of the Streams:', spd_sd)

- What is the distribution of released_year and artist_count? Are there any noticeable trends or outliers?

      sns.set(style='whitegrid')

      plt.figure(figsize=(14, 6))
      plt.subplot(1, 2, 1)
      sns.histplot(spotify_data['released_year'], bins=30, kde=True, color='skyblue')
      plt.title('Distribution of Released Year')
      plt.xlabel('Released Year')
      plt.ylabel('Frequency')

      plt.subplot(1, 2, 2)
      sns.histplot(spotify_data['artist_count'], bins=30, kde=True, color='lightgreen')
      plt.title('Distribution of Artist Count')
      plt.xlabel('Artist Count')
      plt.ylabel('Frequency')

      plt.tight_layout()
      plt.show()
  
  ![Screenshot 2024-10-28 222342](https://github.com/user-attachments/assets/e915d06a-2320-4dee-9141-fea925b18baf)

###  Top Performers
- Which track has the highest number of streams? Display the top 5 most streamed tracks.

      top_5_tracks = spotify_data.sort_values(by='streams', ascending=False).head(5).reset_index()
      top_5_tracks[['track_name', 'streams']]

- Who are the top 5 most frequent artists based on the number of tracks in the dataset?
  
      grp = spotify_data['artist(s)_name'].value_counts().head(5).reset_index()

      top_5_freqarts = pd.DataFrame(grp)
      top_5_freqarts = top_5_freqarts.rename(columns={'index': 'artist(s)_name'})
      top_5_freqarts

### Temporal Trends
- Analyze the trends in the number of tracks released over time. Plot the number of tracks released per year.

      tracks_per_year = spotify_data['released_year'].value_counts().reset_index()
      tracks_per_year.columns = ['released_year', 'track_count']
      tracks_per_year = tracks_per_year.sort_values(by='released_year')

      plt.figure(figsize=(12, 6))
      sns.barplot(data=tracks_per_year, x='released_year', y='track_count', color='gold')
      plt.title('Number of Tracks Released per Year')
      plt.xlabel('Released Year')
      plt.ylabel('Number of Tracks')
      plt.xticks(rotation=45)  # Rotate x-axis labels for better readability
      plt.grid(axis='y')
      plt.tight_layout()
      plt.show()

![image](https://github.com/user-attachments/assets/42779727-01df-4e72-9bce-2392f60cab3a)

- Does the number of tracks released per month follow any noticeable patterns? Which month sees the most releases?

      spotify_data['released_month'] = spotify_data['released_month'].astype(str)
      tracks_per_month = spotify_data['released_month'].value_counts().sort_index()

      month_mapping = {str(i): i for i in range(0, 13)}
      tracks_per_month.index = tracks_per_month.index.map(month_mapping)
      tracks_per_month = tracks_per_month.sort_index()

      plt.figure(figsize=(12, 5))
      sns.barplot(x=tracks_per_month.index, y=tracks_per_month.values, color='LightBlue')
      plt.title('Number of Tracks Released Per Month')
      plt.xlabel('Month')
      plt.ylabel('Number of Tracks Released')
      plt.xticks(ticks=range(0, 12), labels=['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'])
      plt.grid(axis='y')
      plt.tight_layout()
      plt.show()

      month_names = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
      most_releases_month = tracks_per_month.idxmax()  
      most_releases_count = tracks_per_month.max()  
      print('Month with the most releases:', month_names[most_releases_month - 1], '(', most_releases_count, 'tracks)')

![image](https://github.com/user-attachments/assets/4996406d-f1ad-4dce-ac87-e41e8370738e)

### Genre and Music Characteristics
- Examine the correlation between streams and musical attributes like bpm, danceability_%, and energy_%. Which attributes seem to influence streams the most?

      attributes = ['streams', 'bpm', 'danceability_%', 'energy_%']
      correlation_data = spotify_data[attributes].corr()
      print("Correlation of streams with each attribute:")
      print(correlation_data['streams'].sort_values(ascending=False))
 
      plt.figure(figsize=(18, 5))
      plt.subplot(1, 3, 1)
      sns.scatterplot(data=correlation_data, x='bpm', y='streams', color='purple')
      plt.title('Correlation between BPM and Streams')
      plt.xlabel('BPM (Beats Per Minute)')
      plt.ylabel('Streams')

      plt.subplot(1, 3, 2)
      sns.scatterplot(data=correlation_data, x='danceability_%', y='streams', color='red')
      plt.title('Correlation between Danceability and Streams')
      plt.xlabel('Danceability (%)')
      plt.ylabel('Streams')

      plt.subplot(1, 3, 3)
      sns.scatterplot(data=correlation_data, x='energy_%', y='streams', color='orange')
      plt.title('Correlation between Energy and Streams')
      plt.xlabel('Energy (%)')
      plt.ylabel('Streams')

      plt.tight_layout()
      plt.show()

![image](https://github.com/user-attachments/assets/b424e216-aa9f-4a2b-bf70-b524a3086bc9)

- Is there a correlation between danceability_% and energy_%? How about valence_% and acousticness_%?

      attributes = ['danceability_%', 'energy_%', 'valence_%', 'acousticness_%']
      correlation_matrix = spotify_data[attributes].corr()

      print("Correlation between danceability_% and energy_%:")
      print(correlation_matrix.loc['danceability_%', 'energy_%'])
      print("Correlation between valence_% and acousticness_%:")
      print(correlation_matrix.loc['valence_%', 'acousticness_%'])

      sns.set(style='whitegrid')
      plt.figure(figsize=(12, 6))

      plt.subplot(1, 2, 1)
      sns.scatterplot(data=spotify_data, x='danceability_%', y='energy_%', alpha=0.7)
      plt.title('Danceability vs Energy')
      plt.xlabel('Danceability (%)')
      plt.ylabel('Energy (%)')

      plt.subplot(1, 2, 2)
      sns.scatterplot(data=spotify_data, x='valence_%', y='acousticness_%', alpha=0.7)
      plt.title('Valence vs Acousticness')
      plt.xlabel('Valence (%)')
      plt.ylabel('Acousticness (%)')

      plt.tight_layout()
      plt.show()

![image](https://github.com/user-attachments/assets/a8810a8d-01e6-45c7-8edb-398d1b8e8c07)

### Platform Popularity
- How do the numbers of tracks in spotify_playlists, spotify_charts, and apple_playlists compare? Which platform seems to favor the most popular tracks?

      spotify_playlists_count = spotify_data['in_spotify_playlists'].sum()
      spotify_charts_count = spotify_data['in_spotify_charts'].sum()
      apple_playlists_count = spotify_data['in_apple_playlists'].sum()

      print("Spotify Playlists Track Count:", spotify_playlists_count)
      print("Spotify Charts Track Count:", spotify_charts_count)
      print("Apple Playlists Track Count:", apple_playlists_count)

      if spotify_playlists_count > spotify_charts_count and spotify_playlists_count > apple_playlists_count:
          most_favored_platform = "Spotify Playlists"
      elif spotify_charts_count > apple_playlists_count:
          most_favored_platform = "Spotify Charts"
      else:
          most_favored_platform = "Apple Playlists"

      print("\nPlatform with the most popular tracks:", most_favored_platform)

### Advanced Analysis
- Based on the streams data, can you identify any patterns among tracks with the same key or mode (Major vs. Minor)?

      plt.figure(figsize=(12, 6))
      sns.boxplot(data=spotify_data, x='key', y='streams', hue='mode', palette='Set3')

      plt.title('Distribution of Streams by Key and Mode (Major vs. Minor)')
      plt.xlabel('Key')
      plt.ylabel('Streams')
      plt.xticks(rotation=45) 
      plt.legend(title='Mode')
      plt.grid(axis='y')

      plt.tight_layout()
      plt.show()

![image](https://github.com/user-attachments/assets/405c5d69-9d06-4bcd-8228-3a953ec5388c)

- Do certain genres or artists consistently appear in more playlists or charts? Perform an analysis to compare the most frequently appearing artists in playlists or charts.

      artist_counts = spotify_data['artist(s)_name'].value_counts().reset_index()
      artist_counts.columns = ['artist(s)_name', 'appearance_count']

      plt.figure(figsize=(12, 6))
      sns.barplot(data=artist_counts.head(10), x='appearance_count', y='artist(s)_name', hue='artist(s)_name', palette='pastel', legend=False)

      plt.title('Top 10 Artists by Appearance Count in Playlists/Charts')
      plt.xlabel('Number of Appearances')
      plt.ylabel('Artist(s) Name')
      plt.grid(axis='x')

      plt.tight_layout()
      plt.show()

  ![image](https://github.com/user-attachments/assets/1c5a3743-90d5-4fe4-bb38-3f57b6659ca1)


  
