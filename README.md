# WeRateDogs Twitter Account Analysis
This report is a part of the Wrangle and Analyze data project in the Data Analyst Nanodegree offered by  Udacity. In this project, the aim is to gather data about WeRateDogs® from multiple sources including; Twitter API, to clean the data, analyze it, and generate insights

## Wrangling
The wrangling process included the following phases:
1. Gathering data
Three different types of datasets were used in this project.
    - Twitter_archive_enhanced.csv - WeRateDogs tweets archived in one CSV file. This was directly downloaded. This archive contains basic tweet data (tweet ID, timestamp, text, etc.) for 2356 which have ratings out of their 5000+ tweets as they stood on August 1, 2017, .
    - Image_predictions.tsv – This file contains the respective tweets top three image predictions, image url and number of images. What kind of; dog, animal, or object, was present in each tweet according to a neural network. This data file was downloaded using Requests library.
    - tweet_json.txt – Retweet count, favorite count, as well as any other additional data we found interesting for each tweet. This additional data was queried via the Twitter API using Tweepy library.
    This was the first step in the wrangling process. twitter_archive_enhanced.csv file was read using pandas. Image_predictions.tsv was downloaded using the Requests library. While tweet_json.txt was obtained through querying Twitter’s API and getting JSON objects of all the tweet_ids using Tweepy. After all that was done, the data was imported into our programming environment for the ‘assessing data’ phase.

2. Assessing data
Upon assessing the data visually and programmatically, the following quality and tidiness issues were found:
- Quality Issues
  Some quality issues identified were: 
  - Issue 1: There are missing values in the in_reply_to_status_id, in_reply_to_user_id, retweeted_status_id, retweeted_status_user_id and retweeted_status_timestamp columns in the twitter_archive data.
  - Issue 2: Timestamp columns in twitter_archive is in string instead of datetime datatype. We need the year and the month for each tweet.
  - Issue 3: Tweet_id columns in twitter_archive and image_predictions is in integer format instead of string.
  - Issue 4: Name column header is non-descriptive.
  - Issue 5: Some dog names are invalid.
  - Issue 6: Some tweets are retweets. We need the original tweets and not retweets.
  - Issue 7: Some tweets are not about dogs.
  - Issue 8: Inaccurate ratings for some tweets. The rating denominator of the tweets should be fixed at 10.
  - Issue 9: Incorrect datatypes and missing values as a result of merging.

- Tidiness Issues
Some tidiness issues identified were:
    - Issue 1: The variables; doggo, floofer, pupper and puppo, should be in one column.
    - Issue 2: The additional_data and image_predictions tables should be part of the twitter_archive table.

3. Cleaning data
In this phase, the issues detected in the ‘assessing data’ phase were cleaned.
- Quality Issues
    - Issue 1:
         - Fill missing values in twitter_archive table with None.
    - Issue 2:
        - Convert timestamp column to datetime datatype. Extract the year and the month from timestamp column and create new columns called tweet_month and tweet_year.
    - Issue 3:
        - Convert tweet_id columns in twitter_archive and image_predictions from integer to string datatype 
using astype.
    - Issue 4 & 5:
        - Rename 'name' column as 'dog_name'. Subset columns where 'dog_name' does not start with capital letter and replace with 'None'.
    - Issue 6:
        - Select and drop rows where retweet_status_timestamp is not 'None'. Drop 'in_reply_to_status_id', 'in_reply_to_user_id', 'retweeted_status_id', 'retweeted_status_user_id' and 'retweeted_status_timestamp' columns.
    - Issue 7:
        - Subset rows where 'dog_class' column value is others, 'p1_dog', 'p2_dog' and 'p3_dog' column values are False and where 'p1_conf', 'p2_conf' or 'p3_conf' values are greater than 0.95, create a list of the tweet ids that satisfy those criteria and drop rows with tweet id that appears in list.
    - Issue 8:
        - Select rows where rating denominator is not equal to 10 and drop those rows. Convert rating columns from string to integer datatype.
    - Issue 9:
        - Fill missing values in columns with integer and float datatypes with 0. Convert: retweet_count and favorite_count columns to integer datatype, p1_conf, p2_conf and p3_conf columns to float, and p1_dog, p2_dog and p3_dog columns to boolean. Fill missing values in columns with object datatype with 'None'. Reset the index of the cleaned data.
- Tidiness
    - Issue 1:
        - Create a copy of the cleaned twitter_archive data. Melt the doggo, floofer, pupper and puppo columns into one column called 'class' and the values in another column called 'dog_class'. Select rows where the dog_class column is not with the value 'None' and name the new data table 'classified_dogs'. Merge the classified_dogs data to the copy of the cleaned twitter_archive data. Drop the doggo, floofer, pupper, puppo and class columns. Fill missing values with 'others' and drop duplicate tweet ids.
    - Issue 2:
        - Merge additional_data table and image_predictions table to twitter_archive table.

4. Storing data
The wrangled data was then saved to a csv file named “twitter_archive_master.csv”.

5. Additional Wrangling Efforts
In order to facilitate analysis, additional wrangling efforts were made and these include:
    - Creating a new column in clean_twtarchive_master data called 'interact_count' which is the sum of retweet_count and favorite_count.
    - Subsetting tweets where predicted image for the 3 predictions is a breed of dog, and creating a 'predicted_breed' column in the new ‘dog’ data with column values being the image prediction with the highest probability. Then, dropping rows where the predicted_breed is 'No breed'.
    - Creating a new dataframe called ‘tweets_per_year’ with the total number of tweets based on year and month and subsetting the tweet counts for each year.
    
## Insights
1. The tweet with the highest number of interactions has 214,397 interactions. It was published on the 18th of June in the year 2016 and it is about a doggo, predicted to be a Labrador retriever at a confidence level of 82%, with a dog rating of 13/10.
2. Tweets about dogs who are classified as doggos have more retweets than floofers, puppers and puppos. Conversely, tweets about floofers are the least retweeted.
3. Tweets that are about puppers are favored more as compared to doggos, floofers, and puppos.
4. Tweets which are about dogs who are classified as floofers are the least retweeted and also the least favored, hence they have the lowest interaction.
5. Tweets about dogs which are puppers have the most interaction in terms of retweets and favorites as compared with doggos, floofers and puppos.
6. WeRateDogs tweeted a lot more in the year, 2016 as compared to 2015 and they significantly tweeted more in the months of November, December and January.
7. Dog that are golden retrievers have the highest retweet count and favorite count; basically the highest interaction.
8. WeRateDogs tweeted most towards the ending of 2015 and the beginning of 2016, after which their tweet frequency steadily declined until their lowest periods in 2017.

## Summary and Conclusion
In summary, WeRateDogs tweet volume has steadily reduced over the years, while the same cannot be said for their engagements. For their engagements however, it appears that WeRateDogs’ followers prefer content about puppers in general, and golden retrievers specifically, since tweets about these seem to drive the most engagements.
