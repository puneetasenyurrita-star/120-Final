# Puneeta's 120-Final
This final project will show my audience of peers and instructor that I incorporate math programming and software to gain insight in analyzing music data through different comparisons. Specifically, I am using Rihanna's music albums. By doing this, I will be analyzing across her albums, how sentiment changes, how stopwords impact them, and how the most occuring keywords play a role.

# Instructions to run notebook (Tenative till Final Draft is submitted)

## 1. Check if running in Google Colab (everything else TBC)
import os
import sys
try:
    import google.colab
    IN_COLAB = True
    print("Running in Google Colab")

    # Clone repository if in Colab
    if not os.path.exists('/content/drive/MyDrive/120-Final/'):
        !git clone https://github.com/puneetasenyurrita-star/120-Final.git

    # Change to project directory
    # The repository is cloned into /content/120-Final by default
    os.chdir('/content/120-Final')

except ImportError:
    IN_COLAB = False
    print("Running locally")

## Add src directory to Python path
if 'src' not in sys.path:
    sys.path.append('src')
print(f"Current working directory: {os.getcwd()}")

## 2. Load dataset (in 2 separate cells)
from google.colab import drive
drive.mount('/content/drive')

import pandas as pd
## Load the CSV file directly into a pandas data frame and read the data
df_chat = pd.read_csv('/content/drive/MyDrive/120-Final/Rihanna.csv')
df_chat.head()

## 3. Test one song's lyrics to check if it uploads fully without errors
import pandas as pd
csv = '/content/drive/MyDrive/120-Final/Rihanna.csv'
df_chat = pd.read_csv(csv)

## Display the full lyrics of the first song in the df_chat data frame by index-based
first_song_lyrics = df_chat.iloc[0]['Lyric']
print(first_song_lyrics)

## 4. Use vader_lexicon we have used in class for the sentiment analysis.
import nltk
from nltk.sentiment.vader import SentimentIntensityAnalyzer

## Download the vader_lexicon if not already downloaded
try:
    nltk.data.find('sentiment/vader_lexicon.zip')
except LookupError:
    nltk.download('vader_lexicon')

## instantiate a sentiment classifier
sid = SentimentIntensityAnalyzer()

## For now, let's do a test placeholder is defined to avoid another NameError
test = 'PATH'
print(test)
sid.polarity_scores(test)

## 5. Filter out each album from most recent released to oldest.
unique_albums = df_original_songs[~df_original_songs['Album'].isnull()]['Album'].unique()

for album in unique_albums:
    print(f"\n--- Album: {album} ---")
    album_songs = df_original_songs[df_original_songs['Album'] == album]
    for index, row in album_songs.iterrows():
        song_title = row['Title']
        song_lyrics = row['Lyric']
        print(f"\nTitle: {song_title}")
        print(f"Lyrics:\n{song_lyrics}")

## 6. Calculate average sentiment score of each album.
sentiment_columns = ['neg', 'neu', 'pos', 'compound']
df_album_sentiment = df_original_songs.groupby('Album')[sentiment_columns].mean().reset_index()
## small set (few albums) test
display(df_album_sentiment.head())
## non-sorted (might delete / clean)
display(df_album_sentiment)
## for organized / sorted order of time
display(df_album_sentiment_sorted)

## 7 visualization in bar chart for visualizing average sentiment per album
import matplotlib.pyplot as plt
import numpy as np

## Sort the DataFrame by compound sentiment for better visualization
df_album_sentiment_sorted = df_album_sentiment.sort_values(by='compound', ascending=False)

## Generate a color map for each album
colors = plt.cm.viridis(np.linspace(0, 1, len(df_album_sentiment_sorted)))
plt.figure(figsize=(12, 8)) # Adjusted figure size for horizontal bars
plt.barh(df_album_sentiment_sorted['Album'], df_album_sentiment_sorted['compound'], color=colors)
plt.ylabel('Album') # Swapped labels
plt.xlabel('Average Compound Sentiment Score') # Swapped labels
plt.title('Average Compound Sentiment Score per Album (Horizontal, Diverse Colors)')
plt.tight_layout()
plt.show()


