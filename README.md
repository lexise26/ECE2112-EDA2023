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
