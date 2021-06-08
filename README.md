 <h1 align='center'>CHILLAR TUNES - SPOTIFY RECOMMENDATIONS</h1>
 
 In this project I have built a Song Recommendation model to recommend songs from a users existing playlist.  I had over 140,000 Minutes of playtime, with songs being from various languages like English, Hindi, Telugu, Spanish, German and Korean.  The playlist and streaming data is pulled using Spotify API and I will be using a Content-Based Filtering mechanism.
 
 ## Technologies Used

<details>
<a name="Technologies_Used"></a>
<summary>Show/Hide</summary>
<br>
 
 * **Python**
 * **Pandas**
 * **Numpy**
 * **Seaborn**
 * **Matplotlib**
 * **SpotiPy API**
 * **Sci-kit Learn**
 * **Google Collab**
 </details>
 
  ## About the Data
 
 <details>
<a name="Technologies_Used"></a>
<summary>Show/Hide</summary>
<br>
  
  The project uses three separate datasets. *StreamingHistory.csv* has Track_name, track_id, msPlayed and the Date of play. Each record is a song being played on the Spotify web player or app. *Playlists.csv* has the playlist data of a user. It has over 40 columns including information such as artist_name, artist_id, playist_id, playlist_name, created_at and other such informatio but more importantly it contains the various attributes of a song like danceability, speechiness, acousticness, instrumentalness, liveness and tempo. The final dataset is from kaggle (https://www.kaggle.com/yamaerenay/spotify-dataset-19212020-160k-tracks). This contains all the songs currently on Spotify USA and their relavent metadata.
</details>

## Retrieving the Data

<details>
<a name="Technologies_Used"></a>
<summary>Show/Hide</summary>
<br>
  
 I used a Spotify Web API called [Spotipy](https://spotipy.readthedocs.io/en/2.18.0/). It is a really easy-to-use API which makes it easy to make calls to the Spotify servers. To start I created a [Spotify for Developers](https://developer.spotify.com/) account. This acts as a HQ to monitor who has access to your app. Here we need to give acces to a redirect URI. I used  http://localhost:9001/callback. Put the client_id, client_secret and redirect_uri in a secrets.yml file, for API calls and OAuth. From here I retrieved StreamingHistory and Playlist data.
 
  </details>
  
  ## Data Cleaning & Engineering

<details>
<a name="Technologies_Used"></a>
<summary>Show/Hide</summary>
<br>
  
 The *StreamingHistory.csv* is essentially a history of minutes listened to on Spotify. I used the *groupby* function on Track Name and Track ID and created two new columns for total time a song was played for and the number of times a song was played. 
  
  The *Playlist* dataset had a ton of useless data like playlist id, playlist name, created_at etc. After removing the useless data I got to cleaning the data. Spotify sometimes has the same song with different IDs as explicit and non-explicit. So I looked at the total listen time and deleted those non-explicit songs which I didnt listen as much. For feature engineering I added a Tokenization-count feature which is a count for an artist. If the artist is listened often, then the count feature is higher, acting as a sort of weight in favour of the record.  
  
I am the type of guy to just add the song to a playlist and forget about it. To get a representative target label, I used visualized the amount of minutes listened and amount of times a song was played. I found out that there is a sharp drop of songs at 20 count. So I chose this to represent my label as these are my true favouite songs. 
</details>  

## Feature Selection & Model Building
<details>
<a name="Technologies_Used"></a>
<summary>Show/Hide</summary>
<br>
 
  I used Recursive Feature Elimination with Cross_Validation(RFECV) to figure out what features are useful. After plotting the various features and their param_scores, I made a decision to cut-off the 4 features which had very low scores. I used Stratified 10-Fold with GridSearchCV train my model. I used three estimators: K-NN, Random Forest and XGBoost. I used the class_weights param in XGBoost as my data was imbalanced. The models performed like so: Random Forest 92% F-1, XGboost: 94% F-1, K-NN: 81% F-1.
 
I also discovered that the song features that had the most association with a favorite song of mine were popularity (+ association), danceability (+ association), and instrumentalness (- association). This finding indicates that my music taste is generally mainstream with good characteristics of a dance song but also with more focus on words. All these features appear to represent the Pop genre, which may indicate that is may favorite genre.
  
Finally I used a open-source Kaggle dataset containing all songs from Spotify USA, to recommend me my new songs. This I had a array of new songs to listen, not being constrained by Spotify's 30 songs recommendations in Discover Weekly. 
  
### Playlist Creation
  
  I wrote a script which takes the new prediction from a pandas DataFrame into a Spotify playlist. I again used SpotiPy API to achieve this.
  </details>
  
  ## Future Work
<details>
<a name="Technologies_Used"></a>
<summary>Show/Hide</summary>
<br>
  
While this model works well as a Content-based recommendation system, it runs into the problem of cold-start. We can solve this by using the SpotifyFeatures playlists 'popularity' feature and recommend songs based on that to new users.
  
 While this project talks to Spotify API, I want to produtionize this and make it web-facing so that anyone can use it. For that I'm planning on learning Flask or FastAPI which are web-app APIs used to put apps on the web.
