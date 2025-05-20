# Scraping Youtube for Using Data API v3
In this project i am collecting video metadata and thumbnails using youtube data api v3. i am collecting videos related to skincare. The data collection is effctively useful for analysing trends and marketing strategies for skincare products. 
## Setting up API
To set up API 
1. Go to Google cloud console
2. Create or sellect a project
3. Go to APIs and Services
4. Eneble youtube data api v3
5. Create credential 
API key is now ready


## Sraping metadata to csv file
***Importing Neccessary librairies ***
```python
from googleapiclient.discovery import build
import pandas as pd
```
*** Scraping data  ***
1. Set bup API using build() from google.client.discovery. 
using search of function video data of 50 videos are requested. 
from the respose video id object is created which contains video ids of the 50 videos
using the id metadat of each videos are scraped.
```python
api_key="Your api key"
youtube=build("youtube","v3",developerKey=api_key)
request=youtube.search().list(q="Skincare",part='snippet',maxResults=50,type='video').execute()
video_id=[item["id"]["videoId"] for item in request["items"]]
video_request=youtube.videos().list(part="snippet,statistics,contentDetails",id=",".join(video_id)).execute()
```
***creating datframe of meta data and saving into csv file ***
The metadata 
1. Title
2. Channel	
3. Published_Date	
4. Description	View	
5. Likes	
6. Comment	
7. Duration	
8. Video_id
are  appended in toa dictionary and then converted into a dataframe.
```python
video_data=[]
for item in video_request['items']:
    video_data.append({'Title':item['snippet']['title'],'Channel':item['snippet']['channelTitle'],'Published_Date':item['snippet']
                       ['publishedAt'],'Description':item['snippet']['description'],'View':item['statistics']['viewCount'],
                       'Likes':item['statistics'].get('likeCount'),'Comment':item['statistics'].get('commentCount'),'Duration':item["contentDetails"]
                       ['duration'],'Video_id': item['id']})
```
the data frame is saved as csv file using pandas to_csv() function
```python
df=pd.DataFrame(video_data)
df.to_csv('meta_data.csv',index=False)
```
## Scraping thumbnail images
***Creating a folder***
the library os is used for creating a folder for saving all thumbnail images
```python
import os
folder="thumbnails"
os.makedirs(folder,exist_ok=True)
```
***Requesting Thumbnail urls and saving into thumbnails older***
```python
import requests
or item in request['items']:
    Video_id=item['id']["videoId"]
    thumbnail_url=item['snippet']['thumbnails']['high']['url']
    thumbnail_response=requests.get(thumbnail_url)
    if thumbnail_response.status_code==200:
        with open(f"{folder}/{Video_id}.jpg","wb")as f:
            f.write(thumbnail_response.content)
```
## Conclusion
Meta data and thumbnail of 50 videos are collected from youtube using data api v3 by google. finding this data manually could cause misinformation and is time consuming. APIs are eefective way to extract data, large data can be collected using API quickly.
By sing Data API v3 I scrapped videos related to skincare. The meta data can be usefull for analysis the trend in skincare by analysing the title and video descriptions using NLP.from a quick glance i found that korean glass skin is a trend among creators. from the thumbnails what type of content are done by coretors can be identified. MOst creator's content are skin care routtine and skincare tips.

