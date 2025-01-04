<h1>Spotify Wrapped: A Deep Dive Into 8 Years of Data
<br />
Part 1. Retrieving Data From Spotify Using Python and Spotify API
</h1>



<h2>Description</h2>

<p>When Spotify Wrapped didn’t quite meet my expectations this year, I decided to create my own version! This project is all about retrieving and exploring my Spotify listening habits using Python and the Spotify API. There is a second part of this project interely focused on data analysis using SQL.</p>
<br />


<p>Before diving into the analysis, I first had to complete 2 essential steps.</p>
<br />

- <b>Requesting My Listening Data</b>

<p>Spotify allows users to request their historical listening data. I submitted my request through the Spotify Privacy Page. After a few days, I received severalJSON files containing my listening history, along with a helpful "Read Me First" guide that explained the structure of the data.</p>

<img src="/images/spotify_request_data_screen.jpeg" height="50%" width="50%" alt="Download Spotify Data"/>
<br />
<br />
<img src="/images/spotify_json_files.png" height="80%" width="80%" alt="Spotify listening history json files"/>
<br />
<br />


- <b>Setting Up Spotify Developer Access</b>
<p>To go further, I needed to connect to Spotify's API to access the genre data which was not available in the json files provided.  I headed over to the Spotify Developers Page to create a developer account and enerate a Client ID and Client Secret for API authentication.</p>

<img src="/images/spotify_for_developers.png" height="80%" width="80%" alt="Spotify for developers account"/>
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

<img src="/images/spotify_wrapped_python_dataset_image.png" height="50%" width="50%" alt="The output of a DataFrame in Python"/>
<p><i>This image shows the output of a DataFrame in Python, specifically the first few rows of a combined dataset</i></p>
<br />
