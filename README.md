<h1>Spotify Wrapped: A Deep Dive Into 8 Years of Data
<br />
Part 1. Retrieving Data From Spotify Using Python and Spotify API
</h1>



<h2>Description</h2>

<p>When Spotify Wrapped didn’t quite meet my expectations this year, I decided to create my own version! 
This project is all about retrieving and exploring my Spotify listening habits using Python and the Spotify API. 
There is a second part of this project interely focused on data analysis using SQL.</p>
<br />


<p>Before diving into the analysis, I first had to complete 2 essential steps.</p>
<br />

- <b>Requesting My Listening Data</b>

<p>Spotify allows users to request their historical listening data. I submitted my request through the Spotify Privacy Page. After a few days, I received several JSON files containing my listening history, along with a helpful "Read Me First" guide that explained the structure of the data.</p>
<p align="center">
<img src="/images/spotify_request_data_screen.jpeg" height="50%" width="50%" alt="Download Spotify Data"/>
<br />
<br />
<img src="/images/spotify_json_files.png" height="80%" width="80%" alt="Spotify listening history json files"/>
</p>
<br />
<br />


- <b>Setting Up Spotify Developer Access</b>
<p>To go further, I needed to connect to Spotify's API to access the genre data which was not available in the json files provided.  I headed over to the Spotify Developers Page to create a developer account and generate a Client ID and Client Secret for API authentication.</p>
<p align="center">
<img src="/images/spotify_for_developers.png" height="80%" width="80%" alt="Spotify for developers account"/>
</p>
<br />


<p>With these initial steps completed, I was ready to move on to data retrieval and exploration.</p>
<br />
<br />
<h2>Project walk-through:</h2>

<h3>Step 1. Importing relevant Python packages </h3> 

```python
# Step 1.Import relevant Python packages

import pandas as pd
import numpy as np

```
<br />

<h3>Step 2. Uploading data </h3> 

```python
#Step 2.Upload data

# File paths to 3 JSON files from Spotify
file_path_1 = 'Streaming_History_Audio_2017-2019_0.json'
file_path_2 = 'Streaming_History_Audio_2019-2021_1.json'
file_path_3 = 'Streaming_History_Audio_2021-2024_2.json'

# Read each JSON file into a DataFrame
df1 = pd.read_json(file_path_1)
df2 = pd.read_json(file_path_2)
df3 = pd.read_json(file_path_3)

# Combine the DataFrames using concat function, ingnore Index
df_combined = pd.concat([df1, df2, df3], ignore_index=True)

# Pring the combined DataFrame
print(df_combined.head())

```
<br />

<div align="center">
<img src="/images/spotify_wrapped_python_dataset_image.png" height="50%" width="50%" alt="The output of a DataFrame in Python"/>
<p><i><sub>This image shows the output of a DataFrame in Python, specifically the first few rows of a combined dataset</sub></i></p>
</div>

<br />

<h3>Step 3. Preparing the data </h3> 
<p>I loaded 3 JSON files containing 8 years of my listening history into Python, cleaned the data by removing sensitive information like my IP address, transformed it by converting milliseconds into seconds and minutes, and enriched the dataset by adding new columns such as year/month from timestamps and categorizing tracks into “Music” and “Podcasts.”</p>

```python
#Step 3. Preparing the data
# 3.1 Drop the ip_addr column as it is sensitive information
df_combined = df_combined.drop(columns=['ip_addr'])

```
<br />

```python
# 3.2 Create additional time variables by converting the ts column to a datetime object, 
#then formating it to extract the year, year_month, month and date.

df_combined['year'] = pd.to_datetime(df_combined['ts']).dt.strftime('%Y') 
df_combined['year_month'] = pd.to_datetime(df_combined['ts']).dt.strftime('%Y-%m')
df_combined['month'] = pd.to_datetime(df_combined['ts']).dt.strftime('%m')
df_combined["date"] = pd.to_datetime(df_combined["ts"]).dt.strftime('%Y-%m-%d')


```
<br />


```python
# 3.3 Convert miliseconds to seconds and minutes
df_combined["sec_played"] = round(df_combined["ms_played"]/1000,4) #Convert milliseconds to seconds
df_combined["min_played"] = round(df_combined["ms_played"]/1000/60,2) #Convert milliseconds to minutes


```
<br />


```python
# 3.4 Classify the type of content as either "Music", "Podcast", or "Error" 
df_combined["type"] = np.where(
    df_combined["spotify_track_uri"].notnull(), "Music",
    np.where(df_combined["spotify_episode_uri"].notnull(), "Podcast", "Error")
)

```
<br />



```python
# 3.5 Classify the devices into 'mobile' or 'desktop' using regex
df_combined['device_type'] = np.where(
    df_combined['platform'].str.contains(r'android|ios|iphone|ipad|redmi|samsung', case=False, na=False), "mobile",
    np.where(
        df_combined['platform'].str.contains(r'os x|osx|web_player|desktop', case=False, na=False), "desktop",
        "unknown"
    )
)

# Drop the 'platform' column as it is sensitive information
df_combined = df_combined.drop(columns=['platform'])
```
<br />

```python
# 3.6 Rename some variables for readability
df_combined = df_combined.rename(columns = {'master_metadata_track_name' : 'track_name',
                                   'master_metadata_album_artist_name' : 'artist_name',
                                   'master_metadata_album_album_name' : 'album_name'
                                             })

```
<br />

<h3>Step 4. Exporting data to CSV </h3> 

```python
#Step 4. Export the DataFrame to a CSV file
output_file = 'spotify_data_processed.csv'
df_combined.to_csv(output_file, index=False)

print(f"DataFrame has been exported to {output_file}")
```

<br />
<h3>Step 5. Retrieving Genre data using Spotify API </h3> 
<p>The original files prepared by Spotify didn’t contain genre information, so I used the Spotify API to retrieve it. In addition to using Python packages like Pandas and NumPy for data manipulation and analysis, I also needed to install the Spotipy library to interact with the Spotify API.</p>
<p>
I set up Spotify API credentials and created a function to fetch genres for each artist by searching the Spotify database, then applied this function to a list of unique artist names from a CSV file I’ve prepared at step 4, added the genres to the dataset, and saved the updated file with the new "Genres" column.</p>
<p>
Then I exported the cleaned and transformed data into 2 CV files for further exploration using SQL in Snowflake.
</p>

```python
import spotipy
from spotipy.oauth2 import SpotifyClientCredentials

# Step 1: Set up Spotify API credentials
client_id = "My client ID"  
client_secret = "My client secret"  

# Authenticate with Spotify API
sp = spotipy.Spotify(client_credentials_manager=SpotifyClientCredentials(client_id=client_id, client_secret=client_secret))

# Step 2: Function to fetch genres for an artist
def get_artist_genres(artist_name):
    try:
        # Search for the artist
        results = sp.search(q=f'artist:{artist_name}', type='artist', limit=1)
        if results['artists']['items']:
            artist = results['artists']['items'][0]
            return ', '.join(artist.get('genres', []))  
        else:
            return "Not Found"
    except Exception as e:
        return f"Error: {e}"

# Step 3: Load the CSV file
input_file = "spotify_data_processed.csv"  
output_file = "artists_with_genres_all.csv"  

# Load the list of artists
df = pd.read_csv(input_file)
artist_names = df['artist_name'].dropna().unique()  # Get unique artist names

# Step 4: Add the `Genres` column
genres = []  

for artist in artist_names:
    print(f"Fetching genres for: {artist}...")
    genre = get_artist_genres(artist)  # Fetch genres
    genres.append(genre)  # Append to the list
    time.sleep(1)  # Pause for 1 second to avoid hitting rate limits

# Step 5: Save the updated DataFrame
df['Genres'] = genres[:len(df)]  
df.to_csv(output_file, index=False)

print(f"Genres added and saved to {output_file}!")
```

<br />
<br />

---

<h3>For the second part of the project, please refer to: ⤵️</h3>

[Spotify Wrapped: A Deep Dive Into 8 Years of Data. Part 2. Analyzing Spotify Data Using SQL](https://github.com/ilonahetsevich/spotify-wrapped-part2-sql/tree/main) 
