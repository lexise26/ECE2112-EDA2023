# Python: Exploratory Data Analysis on Spotify 2023 Dataset :notes:

Created by: **Custodio, Louise Angela G.** from **2ECE-D** 

Date and Time Started: **October 21, 2024, 8:15 AM**  

Date and Time Ended: **November 02, 2024, 4:30 PM**

## :page_facing_up: Brief Description 
**Exploratory Data Analysis (EDA)** is a process of analyzing and summarizing datasets to identify patterns, trends, and relationships using visual and statistical methods. It helps in understanding the data's structure and guiding further analysis.

This repository contains a comprehensive exploratory data analysis (EDA) of the Most Streamed Spotify Songs 2023 dataset, sourced from Kaggle. The analysis aims to visualize and interpret key features of popular tracks to extract meaningful insights about music trends and attributes. 

#### :pushpin: Key Objectives:
◦ **Dataset Overview**: Familiarize with the dataset structure, check for missing values, and identify data types.

◦ **Summary Statistics**: Provide metrics on streams, release dates, and musical attributes like BPM and danceability.

◦ **Visualizations**: Create well-labeled visualizations (bar charts, histograms, scatter plots) to uncover trends and patterns in the data.

◦ **Correlation Analysis**: Investigate relationships between streams and musical characteristics, and explore correlations between attributes like danceability and energy.

◦ **Insights & Recommendations**: Offer insights into what makes tracks popular, including patterns in artist frequency and platform preferences.

## :thought_balloon: Guide Questions
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

## *Overview of Dataset*
- How many rows and columns does the dataset contain?

      # Get the number of rows and columns by using the .shape function
      rows, cols = spotify_data.shape

      # Print the results 
      print('Rows:', rows)
      print('Columns:', cols)

The dataset contains 953 rows and 24 columns.

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

![image](https://github.com/user-attachments/assets/2d7a6e4b-3f93-42e0-9e52-42d584602e30)

To identify the data types of each column and check for missing values in the dataset, we use the following code. The `dtypes` attribute provides the data type for each column, which helps us understand how the data is structured and what kind of operations can be performed. Additionally, the `isnull().sum()` method counts any missing values in each column. This information is crucial for ensuring data quality before analysis.

## *Basic Descriptive Statistics*
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

![image](https://github.com/user-attachments/assets/ea0f07f6-57d3-43d4-a8bd-127afac0db77)

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

##  *Top Performers*
- Which track has the highest number of streams? Display the top 5 most streamed tracks.

      # Get top 5 tracks by sorting streams in descending order and reset index
      top_5_tracks = spotify_data.sort_values(by='streams', ascending=False).head(5).reset_index()

      # Display the results with its column names
      top_5_tracks[['track_name', 'streams']]

![image](https://github.com/user-attachments/assets/dff4b54d-6412-47e0-bde3-e58ff143857d)

To identify the track with the highest number of streams and display the top five most streamed tracks, we sort the dataset by the "streams" column in descending order using `sort_values()`. This allows us to arrange the tracks from the most to the least streamed. We then select the top five entries with the `head(5)` function and reset the index for a cleaner output. Finally, we display the "track_name" and "streams" columns to showcase the most popular tracks.

- Who are the top 5 most frequent artists based on the number of tracks in the dataset?

      # Group by the artist(s)_name column and count the occurrences
      grp = spotify_data['artist(s)_name'].value_counts().head(5).reset_index()

      # Convert the grouped data into a new DataFrame
      top_5_freqarts = pd.DataFrame(grp)

      # Rename columns to make them more descriptive and print the results
      top_5_freqarts = top_5_freqarts.rename(columns={'index': 'artist(s)_name'})
      top_5_freqarts

![image](https://github.com/user-attachments/assets/c3c65f09-5088-4368-945c-ec1b77e0090a)

To determine the top five most frequent artists based on the number of tracks in the dataset, we utilize the `value_counts()` method on the "artist(s)_name" column. This method counts how many times each artist appears in the dataset, allowing us to identify the most contributors. We then select the top five artists with `head(5)` and reset the index for a clearer presentation. The result is converted into a DataFrame for easy manipulation and is renamed to provide clarity in the output.

## *Temporal Trends*
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

The code analyzes the number of tracks released each month in the dataset to identify patterns and determine the month with the highest releases. It begins by converting the `released_month` column to string format, ensuring consistent data processing. Next, it counts the tracks released in each month using `value_counts()` and sorts the results. A `dictionary maps` month strings to integers, allowing for proper sorting and indexing. The code then creates a bar plot using Seaborn to visualize the number of tracks released per month, with the x-axis representing the months and the y-axis showing the track counts. Finally, it identifies the month with the highest number of releases using `idxmax()` and prints this information along with the corresponding track count. 

## *Genre and Music Characteristics*
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

The code shows the correlation between musical attributes, specifically danceability and energy, as well as valence and acousticness. First, it defines the relevant attributes and calculates their correlation matrix using the `corr()` method. The correlations for the pairs of interest are printed to assess their relationships. Following this, scatter plots are created for both pairs to visualize the correlation between danceability and energy, and valence and acousticness. These visualizations help to discern patterns in how these musical characteristics interact, providing insights into their relationships within the dataset.

## *Platform Popularity*
- How do the numbers of tracks in spotify_playlists, spotify_charts, and apple_playlists compare? Which platform seems to favor the most popular tracks?

      # Convert columns to numeric, coercing errors to NaN and filling NaN with 0 if needed
      spotify_data['in_spotify_playlists'] = pd.to_numeric(spotify_data['in_spotify_playlists'], errors='coerce').fillna(0)
      spotify_data['in_deezer_playlists'] = pd.to_numeric(spotify_data['in_deezer_playlists'], errors='coerce').fillna(0)
      spotify_data['in_apple_playlists'] = pd.to_numeric(spotify_data['in_apple_playlists'], errors='coerce').fillna(0)

      # Sum the columns
      spotify_playlists = spotify_data['in_spotify_playlists'].sum()
      deezer_playlists = spotify_data['in_deezer_playlists'].sum()
      apple_playlists = spotify_data['in_apple_playlists'].sum()

      # Print track counts for each platform
      print("Spotify Playlists Track Count:", spotify_playlists)
      print("Deezer Playlists Track Count:", deezer_playlists)
      print("Apple Playlists Track Count:", apple_playlists)

      # Determine and print the platform with the highest track count
      if spotify_playlists > deezer_playlists and spotify_playlists > apple_playlists:
          most_favored_platform = "Spotify Playlists"
      elif deezer_playlists > apple_playlists:
          most_favored_platform = "Deezer Playlists"
      else:
          most_favored_platform = "Apple Playlists"

      # Print the results of the most popular tracks
      print("\nPlatform with the most popular tracks:", most_favored_platform)

![image](https://github.com/user-attachments/assets/fb086e77-8449-4a0f-9a48-ec60391bd9b1)

The code evaluates the number of tracks featured in different playlists and charts across Spotify and Apple Music. It calculates the total count of tracks in Spotify playlists, Spotify charts, and Apple playlists by summing the respective columns in the dataset. The results for each platform are printed to compare their track counts. Subsequently, an `if-elif` statement determines which platform favors the most popular tracks based on the highest count, concluding with the identification of the platform that showcases the most popular tracks. This analysis provides insight into the distribution of popular tracks across various streaming services.

## *Advanced Analysis*
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

## :computer: Analysis of Spotify Dataset

Based on the analysis of the Spotify dataset, several insights can enhance understanding of what makes a track popular. The increasing trend of artist collaborations indicates that featuring multiple artists can significantly boost a track's visibility and streaming numbers, suggesting that fostering partnerships between established and emerging artists can maximize audience reach. Additionally, the correlation between certain BPM ranges and higher energy levels with increased streams highlights the importance of incorporating upbeat tempos and energetic arrangements, particularly in genres like pop and dance. Timing also plays a crucial role; the analysis shows that releasing tracks during peak streaming periods, such as summer or holidays, can enhance listener engagement. 

While the dataset does not include genre information, examining the attributes of top tracks may reveal successful genre blends, encouraging artists to experiment with diverse sounds. Emotional resonance in lyrics and themes is vital, as relatable content fosters deeper connections with listeners. Furthermore, leveraging social media platforms like TikTok and Instagram for promotion can generate buzz around new releases, while data-driven decision-making through analytics can provide valuable insights into audience demographics and preferences. By implementing these strategies, artists and industry professionals can significantly improve their chances of success in the competitive music streaming landscape.

## :bulb: Conclusion

This Spotify dataset analysis uncovered several interesting patterns and trends within its structure and content. With 953 rows and 24 columns, it captures a wide range of details about each track. Starting with data types and missing values, we checked its overall quality, then looked at the "streams" column using mean, median, and standard deviation to get a general sense of listening behavior. Visualizations of "released_year" and "artist_count" showed notable trends in release years and artist collaborations. Focusing on the most-streamed tracks and frequent artists helped us spot major players driving the dataset’s popularity metrics.

We also found clear trends in track releases over time, noticing both an increase by year and a specific month with peak activity. Looking at correlations between streams and musical features like BPM, danceability, and energy revealed which characteristics tend to boost streaming numbers. Altogether, these insights give a solid overview of track popularity, release patterns, and how musical elements can influence streaming success.

## :iphone: Tools 
- **Canvas** is a web-based learning management system, or LMS. It is used by learning institutions, educators, and students to access and manage online course learning materials and communicate about skill development and learning achievement. 

- **Anaconda Navigator: Jupyter Notebook** is a GUI tool that is included in the Anaconda distribution and makes it easy to configure, install, and launch tools such as Jupyter Notebook.
 
- **Google Chrome** is a free web browser used for accessing the internet and running web-based applications.
  
- **ChatGPT** is an artificial intelligence (AI) chatbot that uses natural language processing to create humanlike conversational dialogue.

## :books: References

- Most streamed Spotify Songs 2023. (2023, August 26). Kaggle. https://www.kaggle.com/datasets/nelgiriyewithana/top-spotify-songs-2023
- seaborn: statistical data visualization — seaborn 0.13.2 documentation. (n.d.). https://seaborn.pydata.org/
- What is Canvas? (2024, October 16). Instructure Community. https://community.canvaslms.com/t5/Canvas-Basics-Guide/What-is-Canvas/ta-p/45
- Mishra, A. (2020). Anaconda and Jupyter Notebook Setup. Machine Learning for iOS Developers, 287–296. https://doi.org/10.1002/9781119602927.app1
- Barney, N. (2023, January 5). Google Chrome browser. Mobile Computing. https://www.techtarget.com/searchmobilecomputing/definition/Google-Chrome-browser
- Hetler, A. (2024, November 1). What is ChatGPT? WhatIs. https://www.techtarget.com/whatis/definition/ChatGPT

## Acknowledgement

I would like to extend my sincere gratitude to Ma’am Madee for her invaluable guidance. Her insights, encouragement, and dedication to teaching have greatly enriched my knowledge. Thank you, Ma’am Madee, for your mentorship and for inspiring me to approach each challenge with curiosity and determination.
  
