# Travel_Trend(Intention)_of_HKers
Travel Trend (Intention) of HKers between 2020 and 2022

### Objective

The main objective of travel intention analysis of HongKongers between 2020 and 2022 is to identify places that they will visit after borders reopening by the Hong Kong government. In order to cover the maximum population in Hong Kong, this project has utilized social monitoring by collecting all relevant posts and comments from LIHKG (which significantly represent the younger generation) and Baby Kingdom (which significantly represent groups who have already married and mostly with children).

As Hong Kong border has been closed since 2020 due to the pandemic and travel has also slowed down since the social movement in 2019, people have strong desire to travel and hope to take action once travel restriction has been lifted. Individuals could leverage this result to avoid crowded places and companies such as travel agencies, airlines and hotels could leverage this result for better resource management. 

### Data Collection

All data from 01 January 2020 has been crawled from LIHKG and Baby Kingdom, this is then cleaned to remove repeated comments (this often happens when a forum user replies to a comment in a thread) and to remove emoji, gif, photo which are irrelevant to our project for this instance. 

Do also take care of the date format as different forums might store dates in a different format. 

### Data Processing

Once we are satisfied with the cleaned data, we then load all data into Google Cloud Platform (GCP). This is the main platform for us to use jupyterlab, conduct Vertex AI model training and perform batch predictions.

Stanza by Stanford NLP group is used as our NLP analysis backbone. It is used to extract features of text using a bag-of-words model and identify named-entity recognition. However, Stanza has a better performance with Simplified Chinese and our raw data is in Traditional Chinese, OpenCC has been used to translate all data sources into Simplified Chinese for better analysis performance. 

To better manage our GCP resources, only data with NER tagged as location will be stored into the result list and only columns that are useful for further analysis will be kept. 

### Vertex AI Model Training

This NLP project caters the number of “Mention” in all the posts and comments for ranking purpose. 
Vertex AI model training has been used to identify whether the comment associated is a positive (User wants to go to this place) or negative (Users do not want to go to this place) or neutral (Just mentioned as a question or a statement which can be a historical event).

1623 records have been tagged and fed into Vertex AI to perform initial training. An average precision of 86.7% has been achieved and hence we have decided to apply this model to the rest of the data. 

### Batch Prediction

Do note that there is a specific JSONL format required for batch prediction on GCP and all processed data is then stored in Google Cloud Storage. Depending on CPU performance, 7 hours have been taken to run all 257,581 rows of data under batch prediction. 

Once batch prediction is complete, use gsuil command to download all the prediction data to one folder. Lastly, consolidate all data into a single list. 

Note: It is always a good practice to print the first few rows of data to check the data structure and format. 

As you may be aware from above, the content is in url.txt format and uuid needs to be used to extract the relevant data. 

### Data Analysis
Join dataframe with the initial data records with dataframe with the batch prediction result. 

For house-keeping purposes, we have checked the prediction result distribution with histogram. 

Result table is then split into 2 dataframes where 1 represents “Places that users want to go” and 1 represents “Places that users do not want to go”. 

The table below represents location, places, cities that ranked top among the rest.

### Insights

In summary, the top countries that HongKongers are eager to go are:

Japan 日本
Taiwan 台灣
United Kingdom 英國
Thailand 泰國
Korea 韓國
Switzerland 瑞士
Australia 澳洲
United States 美國
Iceland 冰島
China 中國大陸

Individuals who are planning to travel once travel restriction has been lifted should try to avoid these places as it may be crowded with people from Hong Kong. These countries could also start initializing some trips or promotions to welcome HongKongers and to better utilize available resources such as tour guides, hotels and air traffic.  

### Limitation
There has been a significant number of “Hong Kong” mentioned, ranked number 2, because some comments such as “Flying from Hong Kong to Dubai” are not removed and have been counted towards the total.
User profile has not been taken into consideration with the assumption that forum users only mention each country once and no repetition has happened. Repetitively mentioning one country in a post by a single user should be rare. 


