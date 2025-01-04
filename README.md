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

<img src="/images/spotify_request_data_screen.jpeg" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="/images/spotify_json_files.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
<br />
<br />


- <b>Setting Up Spotify Developer Access</b>
<p>To go further, I needed to connect to Spotify's API to access the genre data which was not available in the json files provided.  I headed over to the Spotify Developers Page to create a developer account and enerate a Client ID and Client Secret for API authentication.</p>

<img src="/images/spotify_request_data_screen.jpeg" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<br />
<br />

<p>With these initial steps completed, I was ready to move on to data retrieval and exploration.</p>
<br />
<h2>Project walk-through:</h2>

- <b>PowerShell</b> 
- <b>Diskpart</b>

<p align="center">
Launch the utility: <br/>
<img src="https://i.imgur.com/62TgaWL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<img src="/images/spotify_wrapped_python_dataset_image.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<br />
<br />
