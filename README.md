### Problem Statement
RitualCravt, a Metaphysical & Witchy Wares business, has seen a decline in their online sales.

After investigating, they have discovered that their online ads have been targeted towards Bushcraft enthusiasts instead of Witchcraft devotees.

I have been hired to create a model that will ensure their Reddit ads will be directed towards the correct user base. I will accomplish this by collecting and differentiating posts from the subreddit’s Spells and Survival. Through my findings RitualCravt will be able to target their online advertisement more effectively.


### Data:
*I chose to collect data from 2 subreddit groups:*
* reddit.com/r/Survival
* reddit.com/r/Spells

I gathered 1000 posts from each subreddit's new, hot, top, and controversial threads. To obtain these posts I used the praw method.

*After combining and dropping the duplicates of this data I ended up with:*
* 2260 posts from Spells
* 3441 posts from survival

This raw data, named 'all_subreddits.csv', can be found in the data folder.


### Methodology:
*Data Cleaning:*
   * Handling nulls:
       * There were 2458 null values found in the 'body' column of my data. As this was a substantial amount of data I decided to combine the 'title' and 'body' columns to analyze.

   * Getting rid of unwanted characters:
        * I removed all special characters and punctuation from the new 'title_body' column. Each word was tokenized, made lower case, and lemmatized. From this cleaned column, I removed stop words to create a new column 'title_body_stop', for which I performed the majority of EDA on.

   * Distributing data evenly:
        * Being that I combined the title and body rows, I wanted to make sure that the data was distributed evenly so that my models would be accurate. To accomplish this I analysed the total word count and length of posts per subreddit. To correct the uneven distribution I eliminated any rows in the 'title_body' column that had less than 6 words in the Survival group. I also eliminated any rows in the 'title_body' column that had less than 2 rows in the Spells group. This created data that was more evenly distributed.


## EDA:
   *Unique users:*
        * The Spells group had 1283 unique users:
            * 654 posted once
            * 357 posted twice
            * 236 posted 3 times
        * The survival group had 1738 unique users:
            * 1353 posted once
            * 315 posted twice

   * From this information I was able to determine that the survival group had more unique content per user than the Spells group. This might make generalizing my model towards other witch forums a problem.

   *Frequently used words per subreddit group:*
        * The most occuring words from the Spells subreddit are:
        ![Spells Wordcloud](https://git.generalassemb.ly/taracelesta/project_3/blob/master/images/wordcloudsurvive.png?raw=true)
        * The most occuring words from the Survive subreddit are:
        ![Survive Wordcloud](/images/wordcloudsurvive.png)


## Data Preprocessing
   *Data augmentation:*
       * I encoded the target column 'subreddit' which included the text 'survival' or 'spells'. The new encoded column 'num_subreddit' attributed a 0 to the posts from the spells subreddit and a 1 for posts from the survival subreddit.

   *Vectorizing:*
       * I used both Countvectorizer and Term Frequency-Inverse Document Frequency Vectorizer for my data. In my notebook 'modeling_3' you will find examples of this per model created.

   *Shared content:*
        * The 2 groups shared a considerable amount of words. To further increase the accuracy of my models I decided to create a second list of stop words from the frequently occurring shared words from each subreddit group.


## Modeling:
  *The features I used for modeling were:*
       * X = df['cleaned_title_body_str']
       * y = df['num_subreddit']

  *The models I used were:*
     * Multinomial Naive Bayes
     * Random Forest
     * Support Vector Machine

  *Grid Search/ hyperparameter tuning:*
     * I grid searched and tuned each model appropriately depending on the output of .best_params_


## Results:
|Test          |   SVC |   RF |  TF/MNB |   CVMNB |
|--------------|--------|-------|---------|---------|
|Test Accuracy |  .947  |  .979 |  .983   |  .978   |
|Train Accuracy|        |  1.   |  .99    |  .986   |
|Specificity   |  .899  |  .979 |  .983   |  .979   |
|Sensitivity   |  .947  |  .970 |  .983   |  .970   |
|Misclassification|  .52|  .024 |  .016   |  .024   |


## Conclusions:
* My model of using TF-IDF to Vectorize text along with Multinomial Naive Bayes will be able to accurately target the correct subreddit threads for RitualCravt online advertising.


## Next Steps:
* To further investigate this data I would like to discover and explore the words that the TF/MND model missed. I would also like to analyse more witchy subreddits to make sure that my analysis can be generalized towards targeting witches in different groups.
