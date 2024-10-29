# Python: Exploratory Data Analysis on Spotify 2023 Dataset

Created by: **Custodio, Louise Angela G.** from **2ECE-D** 

Date and Time Started: **October 23, 2024, 10:15 AM**  

Date and Time Ended: **October 30, 2024, 3:08 AM**

## Brief Description
**Exploratory Data Analysis (EDA)** is a process of analyzing and summarizing datasets to identify patterns, trends, and relationships using visual and statistical methods. It helps in understanding the data's structure and guiding further analysis.

This repository contains a comprehensive exploratory data analysis (EDA) of the Most Streamed Spotify Songs 2023 dataset, sourced from Kaggle. The analysis aims to visualize and interpret key features of popular tracks to extract meaningful insights about music trends and attributes. 

#### Key Objectives:
◦ **Dataset Overview**: Familiarize with the dataset structure, check for missing values, and identify data types.

◦ **Summary Statistics**: Provide metrics on streams, release dates, and musical attributes like BPM and danceability.

◦ **Visualizations**: Create well-labeled visualizations (bar charts, histograms, scatter plots) to uncover trends and patterns in the data.

◦ **Correlation Analysis**: Investigate relationships between streams and musical characteristics, and explore correlations between attributes like danceability and energy.

◦ **Insights & Recommendations**: Offer insights into what makes tracks popular, including patterns in artist frequency and platform preferences.

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

The next step involves loading the dataset from a CSV file obtained from Kaggle. Using `pd.read_csv('spotify-2023.csv', encoding='ISO-8859-1')`, the data is read into a Pandas DataFrame, allowing for easy organization and manipulation in a table-like structure. The specified encoding, `ISO-8859-1`, ensures special characters are correctly interpreted, which is sometimes necessary for datasets with non-standard characters. Displaying spotify_data confirms successful loading and provides a quick preview of the data’s structure, making it ready for further analysis and visualization.

### *Overview of Dataset*
- How many rows and columns does the dataset contain?

      # Get the number of rows and columns by using the .shape function
      rows, cols = spotify_data.shape

      # Print the results 
      print('Rows:', rows)
      print('Columns:', cols)

  ![image](https://github.com/user-attachments/assets/efccf85b-8f89-4e33-a0b8-924ebef08a96)

To assess the dataset’s size, we use the `shape` attribute in Pandas, which indicates it contains 953 rows and 24 columns. This means there are 953 data entries, each with 24 attributes. The `shape` attribute quickly reveals the dimensions of the DataFrame, helping to understand the dataset's scale.

- What are the data types of each column? Are there any missing values?

      # Get the data types of each column
      data_types = spotify_data.dtypes

      # Print the results
      print("Data Types of Each Column:")
      print(data_types)

      # Check for missing values
      missing_values = spotify_data.isnull().sum()

      # Display missing values for columns with missing data
      print("\nMissing Values in Each Column:")
      print(missing_values[missing_values > 0])

![image](https://github.com/user-attachments/assets/36fc112c-9d88-4ced-8ddb-5655a7ab043d)

To identify the data types of each column and check for missing values in the dataset, we use the following code. The `dtypes` attribute provides the data type for each column, which helps us understand how the data is structured and what kind of operations can be performed. Additionally, the `isnull().sum()` method counts any missing values in each column. This information is crucial for ensuring data quality before analysis.

### *Basic Descriptive Statistics*
- What are the mean, median, and standard deviation of the streams column?

      #Convert the streams column to numeric, setting non-numeric values to NaN
      spotify_data['streams'] = pd.to_numeric(spotify_data['streams'], errors='coerce')

      #Remove rows with NaN in the streams column
      spotify_data.dropna(subset=['streams'], inplace=True)

      #Calculate the mean, median, and standard deviation of the streams column
      spd_mean = spotify_data['streams'].mean()
      spd_mid = spotify_data['streams'].median()
      spd_sd = spotify_data['streams'].std()

      #Print the results of the mean, median, and standard deviation
      print('Mean of the Streams:', spd_mean)
      print('Median of the Streams:', spd_mid)
      print('Standard Deviation of the Streams:', spd_sd)

![image](https://github.com/user-attachments/assets/dd82e3f8-b0b3-437c-bdcc-7b4068571465)

To analyze the "streams" column, we first convert all entries to numeric format using `pd.to_numeric()`, which replaces non-numeric values with NaN. This is crucial to avoid calculation errors. We then remove any rows with NaN values in the "streams" column using `dropna()` to ensure our calculations are based on valid data. We calculate the mean, median, and standard deviation of the "streams," where the `.mean` gives the average, the `.median` indicates the middle value, and the standard deviation measures the variation among stream counts. Finally, we print these results to summarize the statistical properties of the "streams" column, enhancing our understanding of streaming patterns in the dataset.

- What is the distribution of released_year and artist_count? Are there any noticeable trends or outliers?

      # Set Seaborn style
      sns.set(style='whitegrid')

      # Plot distribution of 'released_year'
      plt.figure(figsize=(14, 6))

      # Histogram for released_year
      plt.subplot(1, 2, 1)
      sns.histplot(spotify_data['released_year'], bins=30, kde=True, color='skyblue')
      plt.title('Distribution of Released Year')
      plt.xlabel('Released Year')
      plt.ylabel('Frequency')

      # Histogram for artist_count
      plt.subplot(1, 2, 2)
      sns.histplot(spotify_data['artist_count'], bins=30, kde=True, color='lightgreen')
      plt.title('Distribution of Artist Count')
      plt.xlabel('Artist Count')
      plt.ylabel('Frequency')

      # Displaying the plot
      plt.tight_layout()
      plt.show()
  
![Screenshot 2024-10-28 222342](https://github.com/user-attachments/assets/e915d06a-2320-4dee-9141-fea925b18baf)

To analyze the distributions of "released_year" and "artist_count," we create histograms using Seaborn's `histplot()` with KDE overlays for better clarity. The **first subplot** displays the frequency of songs released across various years, revealing trends and peaks in specific years. The **second subplot** illustrates the distribution of artist counts, highlighting the frequency of songs with varying numbers of collaborating artists. These visualizations help identify noticeable trends, such as years with higher release frequencies and patterns in collaboration, while also revealing any potential outliers in the dataset.

###  *Top Performers*
- Which track has the highest number of streams? Display the top 5 most streamed tracks.

      # Get top 5 tracks by sorting streams in descending order and reset index
      top_5_tracks = spotify_data.sort_values(by='streams', ascending=False).head(5).reset_index()

      # Display the results with its column names
      top_5_tracks[['track_name', 'streams']]

![image](https://github.com/user-attachments/assets/61b86032-ab83-4a7d-a0d9-0ceddf8f6041)

To identify the track with the highest number of streams and display the top five most streamed tracks, we sort the dataset by the "streams" column in descending order using `sort_values()`. This allows us to arrange the tracks from the most to the least streamed. We then select the top five entries with the `head(5)` function and reset the index for a cleaner output. Finally, we display the "track_name" and "streams" columns to showcase the most popular tracks.

- Who are the top 5 most frequent artists based on the number of tracks in the dataset?

      # Group by the artist(s)_name column and count the occurrences
      grp = spotify_data['artist(s)_name'].value_counts().head(5).reset_index()

      # Convert the grouped data into a new DataFrame
      top_5_freqarts = pd.DataFrame(grp)

      # Rename columns to make them more descriptive and print the results
      top_5_freqarts = top_5_freqarts.rename(columns={'index': 'artist(s)_name'})
      top_5_freqarts

![image](https://github.com/user-attachments/assets/d85cff10-7cb4-4477-b37e-2cbbc27f99df)

To determine the top five most frequent artists based on the number of tracks in the dataset, we utilize the `value_counts()` method on the "artist(s)_name" column. This method counts how many times each artist appears in the dataset, allowing us to identify the most contributors. We then select the top five artists with `head(5)` and reset the index for a clearer presentation. The result is converted into a DataFrame for easy manipulation and is renamed to provide clarity in the output.

### *Temporal Trends*
- Analyze the trends in the number of tracks released over time. Plot the number of tracks released per year.

      # Count the number of tracks released each year from the dataset.
      tracks_per_year = spotify_data['released_year'].value_counts().reset_index()

      # Create a new DataFrame with columns for released year and track count.
      tracks_per_year.columns = ['released_year', 'track_count']

      # Sort the DataFrame by released year
      tracks_per_year = tracks_per_year.sort_values(by='released_year')

      # Set up a figure for plotting
      plt.figure(figsize=(12, 6))

      # Create a bar plot to visualize the number of tracks per year
      sns.barplot(data=tracks_per_year, x='released_year', y='track_count', color='gold')

      # Add title and labels to the plot
      plt.title('Number of Tracks Released per Year')
      plt.xlabel('Released Year')
      plt.ylabel('Number of Tracks')

      # Rotate x-axis labels for better readability
      plt.xticks(rotation=45)  
      plt.grid(axis='y')

      # Display the plot
      plt.tight_layout()
      plt.show()

![image](https://github.com/user-attachments/assets/42779727-01df-4e72-9bce-2392f60cab3a)

This code analyzes the trends in the number of tracks released over time by first counting the occurrences of each release year in the "released_year" column of the `spotify_data` DataFrame. The counts are organized into a new DataFrame with clear column names, which is then sorted chronologically. A bar plot is created using Seaborn, with the x-axis representing the release years and the y-axis showing the corresponding track counts, colored gold for clarity. The plot includes a title and axis labels, with x-axis labels rotated for better readability. Grid lines on the y-axis enhance the visualization, making it easier to identify trends and shifts in music production over the years.

- Does the number of tracks released per month follow any noticeable patterns? Which month sees the most releases?

      # Convert the column to string type
      spotify_data['released_month'] = spotify_data['released_month'].astype(str)

      # Count the number of tracks released per month
      tracks_per_month = spotify_data['released_month'].value_counts().sort_index()

      # Convert month strings to integers and sort them
      month_mapping = {str(i): i for i in range(0, 13)}
      tracks_per_month.index = tracks_per_month.index.map(month_mapping)

      # Sort by month
      tracks_per_month = tracks_per_month.sort_index()

      # Plotting the trends
      plt.figure(figsize=(12, 5))
      sns.barplot(x=tracks_per_month.index, y=tracks_per_month.values, color='LightBlue')
      plt.title('Number of Tracks Released Per Month')
      plt.xlabel('Month')
      plt.ylabel('Number of Tracks Released')
      plt.xticks(ticks=range(0, 12), labels=['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'])
      plt.grid(axis='y')

      # Display the plot
      plt.tight_layout()
      plt.show()

      # List of month names
      month_names = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']

      # Get the month index with max releases
      most_releases_month = tracks_per_month.idxmax()  

      # Get the count of max releases
      most_releases_count = tracks_per_month.max()  

      # Print the month with the most releases
      print('Month with the most releases:', month_names[most_releases_month - 1], '(', most_releases_count, 'tracks)')

![image](https://github.com/user-attachments/assets/4996406d-f1ad-4dce-ac87-e41e8370738e)
![image](https://github.com/user-attachments/assets/c8bf1b25-d3b8-4db3-95b9-edf49d879681)

The code analyzes the number of tracks released each month in the dataset to identify patterns and determine the month with the highest releases. It begins by converting the `released_month` column to string format, ensuring consistent data processing. Next, it counts the tracks released in each month using `value_counts()` and sorts the results. A `dictionary maps` month strings to integers, allowing for proper sorting and indexing. The code then creates a bar plot using Seaborn to visualize the number of tracks released per month, with the x-axis representing the months and the y-axis showing the track counts. Finally, it identifies the month with the highest number of releases using `idxmax()` and prints this information along with the corresponding track count. 

### *Genre and Music Characteristics*
- Examine the correlation between streams and musical attributes like bpm, danceability_%, and energy_%. Which attributes seem to influence streams the most?

      # Specify the attributes for correlation analysis
      attributes = ['streams', 'bpm', 'danceability_%', 'energy_%']
      correlation_data = spotify_data[attributes].corr()

      # Display correlation of streams with each attribute
      print("Correlation of streams with each attribute:")
      print(correlation_data['streams'].sort_values(ascending=False))

      # Set up the figure size
      plt.figure(figsize=(18, 5))

      # Scatter plot for BPM vs Streams
      plt.subplot(1, 3, 1)
      sns.scatterplot(data=correlation_data, x='bpm', y='streams', color='purple')
      plt.title('Correlation between BPM and Streams')
      plt.xlabel('BPM (Beats Per Minute)')
      plt.ylabel('Streams')

      # Scatter plot for Danceability vs Streams
      plt.subplot(1, 3, 2)
      sns.scatterplot(data=correlation_data, x='danceability_%', y='streams', color='red')
      plt.title('Correlation between Danceability and Streams')
      plt.xlabel('Danceability (%)')
      plt.ylabel('Streams')

      # Scatter plot for Energy vs Streams
      plt.subplot(1, 3, 3)
      sns.scatterplot(data=correlation_data, x='energy_%', y='streams', color='orange')
      plt.title('Correlation between Energy and Streams')
      plt.xlabel('Energy (%)')
      plt.ylabel('Streams')

      # Adjust layout and display the results of plots
      plt.tight_layout()
      plt.show()

![image](https://github.com/user-attachments/assets/b424e216-aa9f-4a2b-bf70-b524a3086bc9)
![image](https://github.com/user-attachments/assets/5957ce18-d666-4230-a636-9aac9ec05ecc)

The code analyzes the correlation between streams and musical attributes such as BPM, danceability, and energy percentage. It selects these attributes from the dataset and computes their correlation with the `streams` variable using the `corr()` method, displaying the results in descending order to identify the strongest relationships. Additionally, it creates scatter plots for each attribute against streams, allowing for a visual examination of their relationships. This analysis helps to determine which musical features most significantly influence streaming performance, offering insights into factors that contribute to a track's popularity.

- Is there a correlation between danceability_% and energy_%? How about valence_% and acousticness_%?

      # Define the attributes of interest for correlation analysis
      attributes = ['danceability_%', 'energy_%', 'valence_%', 'acousticness_%']

      # Calculate the correlation matrix for the selected attributes
      correlation_matrix = spotify_data[attributes].corr()

      # Display correlations between the specific pairs
      print("Correlation between danceability_% and energy_%:")
      print(correlation_matrix.loc['danceability_%', 'energy_%'])
      print("Correlation between valence_% and acousticness_%:")
      print(correlation_matrix.loc['valence_%', 'acousticness_%'])

      # Set Seaborn as sns with the style of whitegrid
      sns.set(style='whitegrid')

      # Create a figure with subplots
      plt.figure(figsize=(12, 6))

      # Scatter plot for danceability_% and energy_%
      plt.subplot(1, 2, 1)
      sns.scatterplot(data=spotify_data, x='danceability_%', y='energy_%', alpha=0.7)
      plt.title('Danceability vs Energy')
      plt.xlabel('Danceability (%)')
      plt.ylabel('Energy (%)')

      # Scatter plot for valence_% and acousticness_%
      plt.subplot(1, 2, 2)
      sns.scatterplot(data=spotify_data, x='valence_%', y='acousticness_%', alpha=0.7)
      plt.title('Valence vs Acousticness')
      plt.xlabel('Valence (%)')
      plt.ylabel('Acousticness (%)')

      # Show the plots
      plt.tight_layout()
      plt.show()

![image](https://github.com/user-attachments/assets/a8810a8d-01e6-45c7-8edb-398d1b8e8c07)
![image](https://github.com/user-attachments/assets/16ecaf42-5153-4d57-8819-386266a5cfe5)

The code shows the correlation between musical attributes, specifically danceability and energy, as well as valence and acousticness. First, it defines the relevant attributes and calculates their correlation matrix using the `corr()` method. The correlations for the pairs of interest are printed to assess their relationships. Following this, scatter plots are created for both pairs to visualize the correlation between danceability and energy, and valence and acousticness. These visualizations help to discern patterns in how these musical characteristics interact, providing insights into their relationships within the dataset.

### *Platform Popularity*
- How do the numbers of tracks in spotify_playlists, spotify_charts, and apple_playlists compare? Which platform seems to favor the most popular tracks?

      # Calculate the total number of tracks included in different playlists and charts
      spotify_playlists_count = spotify_data['in_spotify_playlists'].sum()
      spotify_charts_count = spotify_data['in_spotify_charts'].sum()
      apple_playlists_count = spotify_data['in_apple_playlists'].sum()

      # Print track counts for each platform
      print("Spotify Playlists Track Count:", spotify_playlists_count)
      print("Spotify Charts Track Count:", spotify_charts_count)
      print("Apple Playlists Track Count:", apple_playlists_count)

      # Determine and print the platform with the highest track count using elif
      if spotify_playlists_count > spotify_charts_count and spotify_playlists_count > apple_playlists_count:
          most_favored_platform = "Spotify Playlists"
      elif spotify_charts_count > apple_playlists_count:
          most_favored_platform = "Spotify Charts"
      else:
          most_favored_platform = "Apple Playlists"

      # Print the results of most popular tracks
      print("\nPlatform with the most popular tracks:", most_favored_platform)

![image](https://github.com/user-attachments/assets/1aa8eb62-87d1-4bdc-a9a3-0063f7c33d9f)

The code evaluates the number of tracks featured in different playlists and charts across Spotify and Apple Music. It calculates the total count of tracks in Spotify playlists, Spotify charts, and Apple playlists by summing the respective columns in the dataset. The results for each platform are printed to compare their track counts. Subsequently, an `if-elif` statement determines which platform favors the most popular tracks based on the highest count, concluding with the identification of the platform that showcases the most popular tracks. This analysis provides insight into the distribution of popular tracks across various streaming services.

### *Advanced Analysis*
- Based on the streams data, can you identify any patterns among tracks with the same key or mode (Major vs. Minor)?

      # Set the figure size for the plot
      plt.figure(figsize=(12, 6))

      # Create a box plot to show the distribution of streams by key and mode
      sns.boxplot(data=spotify_data, x='key', y='streams', hue='mode', palette='Set3')

      # Set the title of the plot
      plt.title('Distribution of Streams by Key and Mode (Major vs. Minor)')

      # Label the x and y axes
      plt.xlabel('Key')
      plt.ylabel('Streams')

      # Rotate x-axis labels for better readability
      plt.xticks(rotation=45) 

      # Add a legend with title for the hue variable (mode)
      plt.legend(title='Mode')

      # Enable grid lines on the y-axis 
      plt.grid(axis='y')

      # Adjust layout to prevent overlap and show the result
      plt.tight_layout()
      plt.show()

![image](https://github.com/user-attachments/assets/405c5d69-9d06-4bcd-8228-3a953ec5388c)

The code examines the distribution of streams for tracks categorized by musical key and mode (Major vs. Minor). A box plot is created using Seaborn to visualize how streams vary across different keys, with the mode distinguished by color. The plot is titled "Distribution of Streams by Key and Mode (Major vs. Minor)" and includes labeled axes for clarity. The x-axis labels are rotated for better readability, and a legend is added to differentiate between Major and Minor modes. Grid lines on the y-axis enhance the readability of stream counts, while the layout adjustments ensure that all elements are displayed clearly. This analysis allows for the identification of patterns in streaming performance based on key and mode.

- Do certain genres or artists consistently appear in more playlists or charts? Perform an analysis to compare the most frequently appearing artists in playlists or charts.

      # Count the occurrences of each artist in the dataset
      artist_counts = spotify_data['artist(s)_name'].value_counts().reset_index()

      # Rename the columns
      artist_counts.columns = ['artist(s)_name', 'appearance_count']

      # Set the figure size for the plot
      plt.figure(figsize=(12, 6))

      # Create a bar plot for the top 10 artists by appearance count
      sns.barplot(data=artist_counts.head(10), x='appearance_count', y='artist(s)_name', hue='artist(s)_name', palette='pastel', legend=False)
      
      # Set the title of the plot
      plt.title('Top 10 Artists by Appearance Count in Playlists/Charts')

      # Label the x and y axes
      plt.xlabel('Number of Appearances')
      plt.ylabel('Artist(s) Name')

      # Enable grid lines on the x-axis 
      plt.grid(axis='x')

      # Adjust layout and display the results
      plt.tight_layout()
      plt.show()

  ![image](https://github.com/user-attachments/assets/1c5a3743-90d5-4fe4-bb38-3f57b6659ca1)

The code analyzes the frequency of artist appearances in playlists and charts by counting each artist's occurrences in the dataset. It uses the `value_counts()` function on the `artist(s)_name` column to tally the number of times each artist appears and resets the index for organization. The resulting DataFrame is renamed for clarity, and a bar plot is generated to visualize the top 10 artists by appearance count. The plot features the number of appearances on the x-axis and artist names on the y-axis, with distinct colors for each artist. This visualization highlights which artists are most frequently featured in playlists and charts, indicating their popularity in the music industry.

## Conclusion
The analysis of the Spotify dataset offers a comprehensive exploration of music streaming trends, revealing insights into consumer preferences and artist performances. Upon loading the data into a Pandas DataFrame, a total of 953 entries and 24 attributes were examined. This initial step included checking data types for each column and identifying missing values to ensure data integrity and reliability. Descriptive statistics for the "streams" column provided key metrics, such as the mean, median, and standard deviation, which are critical for understanding overall streaming behaviors and popularity.

The visualization of the "released_year" and "artist_count" columns highlighted significant trends over time, indicating shifts in music production and collaboration practices among artists. This analysis not only illustrated the increase in track releases in specific years but also showed the growing tendency for artists to collaborate, which has become a hallmark of the contemporary music industry. Identifying the top-performing tracks and artists further emphasized the dataset's richness, pinpointing those who dominated streaming platforms and capturing listener interest.

Moreover, a temporal trend analysis illustrated fluctuations in the number of tracks released both annually and monthly, reflecting industry patterns and seasonal trends that influence music consumption. The correlation analysis conducted between streams and musical attributes, including BPM (beats per minute), danceability, and energy, revealed significant relationships that suggest how these factors may affect streaming success. This multifaceted analysis not only highlights key patterns in music streaming but also sets the stage for further investigation into the evolving landscape of music trends and audience engagement within the Spotify ecosystem. Overall, this exploration underscores the value of the dataset in understanding contemporary music dynamics and consumer behaviors.


  
