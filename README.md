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
Before anything else, importing the needed library is the first thing we need to do for the code to run smoothly.

    import pandas as pd
    import matplotlib.pyplot as plt
    import seaborn as sns

Next is loading the data from the csv file from Kaggle.

    spotify_data = pd.read_csv('spotify-2023.csv', encoding='ISO-8859-1')
    spotify_data

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
