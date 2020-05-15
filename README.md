15 May 2020 

# Tweet Analysis for Camp Fire (CA, 2018)
*Can we build a list of keywords to help detect that a disaster is happening from social media posts?*
- Project team: Justin Fischer, Matt Burke

# Contents:
- [Repo Organziation](#Repo-Organization)
- [Problem Statement](#Problem-Statement)
- [Executive Summary](#Executive-Summary)
- [Data Summary](#Data-Summary)
- [Models and Techniques](#Models-and-Techniques)
- [Conclusions](#Conclusions)
- [Next Steps](#Next-Steps)


# Repo Organization
- *Datasets:* includes source and cleaned datasets used to model. See [below](#Data-Summary) for data description.
- *Code:* includes project code. For this project all code is in Jupyter Notebooks:
	- *00_Data-Collection.* scraping twitter data with [GetOldTweets3 API](#https://github.com/Mottl/GetOldTweets3)
	- *01_dictionarybuilding.*  feature engineering inlcuding: more precise location data, sentiment analysis, classifying tweets as during fire or not. 
	- *02_sentiment.* sentiment analysis using Twitter NLP Toolkit.
	- *03_clustering.* k-means clustering models.
	- *04_charting.* charts for presentation.
- *CampFire2018-presentation.pdf.* this is the deck used for the project presentation, delivered on 5/15/20 to General Assembly students and instructors. 

# Problem Statement
*Can we build a list of keywords to help detect that an event is happening from social media posts?*

# Executive Summary
**CONTEXT.**
For this project, we aimed to build a list of keywords to help detect if an event - and more specifically, a disaster - is happening from social media posts. For our case study we analyzed Twitter data surrounding the 2018 "Camp Fire" in Butte County, CA. See [here](https://en.wikipedia.org/wiki/Camp_Fire_(2018)) for more details on the disaster. Reasons for selecting Camp Fire included:
- timing (i.e., relatively recent)
- occurred in english-speaking country (so we wouldn't have to translate text)
- high media coverage (allows for ample research)


**DATA.**
We gathered the data using the  [GetOldTweets3 API](#https://github.com/Mottl/GetOldTweets3). Tweets were location-based and within a 100 mile radius of Paradise, CA, where the fire started. Tweet date range was between 11/1/18 to 11/26/18 (*note: the fire spanned from 11/8-11/25*). In addition to tweet location, timestamp, and text, we used the API to pull mentions, number of replies, and tweet links. Before modeling and analysis, we also did some basic feature engineering. This included binary classification of if a tweet was fire-related or not as well as if it was during the fire date range or not, and well as scoring tweets based on the number of fire-related keywords it contained. 

**MODELING & ANALYSIS.**
Our modeling and analysis only included text data. The primary model we used was k-means clustering on tweets during the fire in order to identify categories of tweets. We determined that the optimal amount of clusters was 5, as it struck the best balance between interpretability and granularity. Cluster labels included: traffic-related, emotional, California (general), photo-related, and general information.

We also inlcuded sentiment analysis in order to get a sense of the positive/negative split of tweets before and during the fire. The positive/negative splits were as follows: 86/14 pre-fire; 60/40 during fire; 65/35 overall. The lowest point in sentiment occurred on 11/12, 4 days after the fire started. It's also noteworthy that sentiment trended upward, with an average sentiment of ~0.8 on 11/24. 

Finally, we analyzed on our key scores in a number of ways. First, we looked at key score by sentiment. Next, we looked at key score by date. An interesting finding here was that average key scores peaked the 11/8, the day the fire started, then trended downward, averaging out a key score of 1 per day. We also looked at average key score by location. Unsurprisingly, this was highest for Paradise and Chico, those cities most impacted by the fire.

**CHALLENGES.**
Our biggest challenge was pulling a sufficient amount of location-based tweets due to: a) API constraints, and; b) lack of location data users tagged in their tweets. Another challenge was that we only had text data to work with. There is ample, valuable information that can be gained from image-based data during times of disaster. Due to time and resource constraints, however, we were unable to include image-based data. A third constraint is the fact that we only used Twitter data. Finally, not all tweets within the 100 mile radius we were concerned with were captured; only tweets with a location tag. 

This final constraint raised an interesting ethical question: *where is the line between the right to data privacy with social media and using social media data in times of disasters to potentially help save lives?* Exploring answers to that question was beyond the scope of our project, however, it is of utmost importance in considering these types of problems.

**NEXT STEPS.**
In order to address these challenges and further develop a prototype, immediate next steps include:
- Pull additional Twitter Camp Fire data in order to build out a more robust model. 
- Improve keyword list in order to more accurately classify tweets
- Try other models beyond k-Means clustering (e.g., integrate image recognition).
- Test and improve on other disasters.

# Data Summary
**Data Source:**
- All tweet data was scraped using [GetOldTweets3 API](#https://github.com/Mottl/GetOldTweets3). Tweet criteria included:
	- Timeframe: 11/1/18 to 11/26/18
	- All location-tagged tweets within 100 mile radius of Paradise, CA
	- Cities searched: Butte County, Paradise, Chico, Magalia, Oroville

**Datasets Analyzed:**
- [stacked](../datasets/stacked_clean_again.csv)
	- includes both pre-fire tweets and during fire tweets
- [sentiment](../datasets/stacked_sentiment_again.csv)
	- subset of stacked dataset with fire-only tweets, used for sentiment analysis 

### Data Dictionary
|Feature|Type|Dataset|Description|
|--|--|--|--|
|**tweet_count**|*int*|stacked|count of tweet pulled|
|**city**|*string*|stacked|city used to pull tweet using API|
|**id**|*integer*|stacked|twitter-generated unique id for tweet|
|**timestamp**|*datetime*|stacked|time of tweet|
|**tweet_text**|*string*|stacked|tweet text with url|
|**text**|*string*|stacked|tweet text without url|
|**hashtags**|*string*|stacked|tweet hashtags|
|**username**|*string*|stacked|who tweeted|
|**mentions**|*string*|stacked|@mentions in tweet|
|**during_fire**|*binary*|stacked|1: during fire; 0: not during fire|
|**is-fire-related**|*binary*|stacked|1: tweet is fire related; 0: is not|
|**key_score**|*integer*|stacked|number of fire-related keywords in tweet (0-5)|
|**sent**|*binary*|sentiment|sentiment of tweet(1: positive; 0: negative)|


# Models and Techniques

## Feature engineering:
### Key Scoring

We created a list of root words to build a list of words from.  
```
Root words: 'fire', 'evac', 'smok', 'burn', 'wild', 'blaz', 'hell', 'department', 'inferno', 'help'
```

Using this list of root words and comparing them to the hashtags used in tweets, we built a dictionary.  We used root words as a way to account for plurals and other conjugations of the words or phrases.  The key score was calculated by counting each instance of a word in the keyword dictionary in a tweet (including the hashtags). The process was able to pull some of the more user generated hastags used for this specific incident.  

```
Sample Keywords: 'smokey', 'firefighter', 'evacuees', 'califirefighters', 'campfire2018', 'wildfire'
```


### Sentiment

Using the [Twitter-NLP-Toolkit](https://github.com/eschibli/twitter-toolbox) we loaded the `small-ensemble-model` to analyze the tweets sent that were related to a fire.  Since our datarange also includes the week before the fire started, it establishes somewhat of a baseline (albeit there were a few small fires during that time period).  It is not a perfect model and since social media posts during a disaster are very nuanced it is difficult.  If we had a larger amount of posts, additional training would have been beneficial.  Sentiment also does not always indicate a disaster since there was much praise for firefighters, emergency crews and Guy Fieri tweeted about during the fire.  (Guy Fieri teamed up with World Central Kitchen and Jose Andres to help feed those displaced by the fire using a smoker)

**Negative Sentiment Example**
>“Paradise Lost”...
we will never forget what the fire took away...hoping that some day we could return to see it again
...I will be posting photo archives that I still have...the fire took… https://instagram.com/p/BqT4fTslWJD/?utm_source=ig_twitter_share&igshid=irywss0s3sx7  

[source](https://twitter.com/heywoodphotog/status/1064033991261544449)

**Positive Sentiment Example**
> Great to join forces with wckitchen @guyfieri & @chefjoseandres  to cook 15,000 meals for those who lost their homes in the California Fires.  Happy Thanksgiving 🦃 @ Chico, California https://instagram.com/p/BqficAsn5af/?utm_source=ig_twitter_share&igshid=ku7jckq0s0eo
 
 [source](https://twitter.com/ChefHales/status/1065674347581513728)

<sub><sup><sub><sup><sub><sup><sub><sup>(Note: Jose Andres and WC Kitchen are currently operating in a number of cities right now to help feed those affected by the COVID-19 Pandemic)

## Model:
### K-Means Clustering

Using the KMeans clustering algorithms in SK-Learn we attempted to sort tweeted words into clusters.  For this, we only used the subset of tweets about and during the 2018 Camp Fire to try to build the most useful list of terms to flag.  Initially, we just used 3 and 5 clusters but then tried to cluster for 2-20 clusters.  However, this is not the best for this because sentence structure in the English language relies on building off many of the same terms.  
# Conclusions
- Our keyword-based model is a good start, however, it still includes an insufficient amount of data. In order to accurately predict emerging events and distasters, we need significantly more data from sources beyond Twitter. 
- Keyword scoring is a highly effective way to identify the topic of a tweet. The keywords we selected could further be refined for other fires. This same methodology can also be applied to other types of distasters such as hurricanes, floods, and tornados.
- For k-means clustering, the optimal amount of clusters was 5, as it struck the best balance between interpretability and granularity. Cluster labels included: traffic-related, emotional, California (general), photo-related, and general information.
- Though the sentiment analysis was insightful in terms of understanding the positive/negative tones of a tweet over the course of a disaster, sentiment analysis may not be all that effective when trying to identify an emerging disaster.

# Next Steps
- Pull additional Twitter Camp Fire data in order to build out a more robust model. This may include using additional APIs, such as [Twitter's API](https://developer.twitter.com/en/docs).
- Aggregate data from additional social media sources
	- e.g., 880 of the 1,130 tweets analyzed were originally posted to Instagram
- Improve keyword list in order to more accurately classify tweets
- Try other models beyond k-Means clustering (e.g., integrate image recognition)
- Test and improve on other disasters
	- “Slow” disasters (e.g., fires, floods, hurricanes)
	- “Fast” disasters (e.g., tornados, shootings) 
- Develop proof of concept
- *Ideal state*: develop product that agencies can implement to monitor social media data






