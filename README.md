
last updated: 15 May 2020 

# Tweet Analysis for Camp Fire (CA, 2018)
*INSERT short summary of project*
- Project team: Justin Fischer, Matt Burke

# Contents:
- [Repo Organziation](#Repo-Organization)
- [Problem Statement](#Problem-Statement)
- [Executive Summary](#Executive-Summary)
- [Data Summary](#Data-Summary)
- [Models and Techniques](#Models-and-Techniques)
- [Conclusions](#Conclusions)
- [Next Steps](#Next-Steps)
- [Sources and References](#Sources-and-References)

# Repo Organization
- *Datasets:* includes source and cleaned datasets used to model. Further detail [below](#Data-Summary)
- *Code:* includes project code. For this project all code is in Jupyter Notebooks:
	- *00_Preprocessing.* copy description included at the top of the notebook
  - *01_EDA.* copy description included at the top of the notebook
  - *02_Modeling.* copy description included at the top of the notebook
- *Presentation_name.* This is the deck used for the project presentation, delivered on DATE to AUDIENCE. 
- Any other aspects of repo, including: assets, resources/references, etc.

# Problem Statement
*Insert problem statement*

# Executive Summary
***Paragraph 1: give project context. Restate problem statement. Describe data source(s). 1-liner of findings***
*Example:* In this binary classification problem, we used Natural Language Processing (NLP) to train a classifier model on which subreddit a given post came from. The post text was scraped using Pushshift's API from subreddits *r/userexperience* and *r/UXResearch*, where *r/userexperience* was the positive class (1) and *r/UXResearch* the negative class(0). The dataset used to model consisted of 13,611 documents (comments & submissions), with a baseline accuracy of 53%/47% between positive class/negative class.


***Paragraph 2: describe model(s) and technique(s) used.***
*Example:* The following classifier models were trained: Logistic Regression, Multinomial Naive Bayes, Random Forest, AdaBoost, Voting Classifier. Models were trained using pipelines and GridSearch to test different vectorizers - i.e., CountVectorizer and TfidfVectorizer - and hyperparameters. Models were then evaluated based on their accuracy scores. The best performing model scored 74% accuracy on the train data and 72% accuracy on the test data. This was a Logistic Regression model passed through the TfidfVectorizer with an n-gram range of (1,2) and an l1 penalty (i.e., lasso regression).


***Paragraph 3: summarize / compare findings and their significance.***
*Example:* The test accuracy score of 72% was 19 percentage points than the baseline accuracy of 53% for the positive class. There were two primary challenges with improving the accuracy score. First, the content of each subreddit is very similar. User Experience and User Experience Research share many of the same types of posts with similar language. The second, and perhaps most important, challenge was computing constraints. With 6 cores, my computer was limited in the amount of information it could process and time in which it could process it. Some models took up to 30 minutes to fit while others simply timed out. This was a challenge when I tried to fit complex models (e.g., Radom Forest) and/or test various hyperparameters simultaneously.


***Paragraph 4: summarize recommendations and next steps. Can also describe challenges and how they should be addressed moving forward.***
*Example:* Before putting this model into any sort of production, the accuracy scores would need to be increased by at least 15 percentage points. To that end, next steps would be acquiring more computing power and testing multiple hyperparameters (e.g., different stopwords, n-grams, penalities, etc) over various models. Upon training a model with a sufficient accuracy score, a few interesting implementations of it might include analyzing posts to recommend posting in the other subreddit in order to get more interaction or analyzing post quality to make recommendations to employers for potential candidates. These are just a few ideas that, especially when paired with other features from the API (e.g., time of post, number of upvotes), could lead to some very interesting insights. Before any idea is to be implemented, though, the accuracy score must improve.

# Data Summary
**Data Source:**
- *Describe where the dataset came from, how it was acquired (e.g., scraped, shared from stakeholder, aggregated).*
- *Example:* The data for this project was scraped from Reddit using [Pushshift’s API](https://github.com/pushshift/api). [r/userexperience](https://www.reddit.com/r/userexperience/) and [r/UXResearch](https://www.reddit.com/r/UXResearch/) were the subreddits from which the text data was extracted.

**Datasets Analyzed:**
- [dataset.csv](./path-to/file.csv)
	- 000 rows, 00 columns
    - describe dataset
- [merged_processed.csv](./datasets/merged_processed.csv)
  -  13,611 rows, 2 columns


### Data Dictionary
[here's a helpful data dictionary template](https://docs.google.com/spreadsheets/d/1QWgMUvDJi9jWF86o58mie_JHXCi3TkN01XY7vqyNR58/edit#gid=206608275)

*Example:*

|Feature|Type|Dataset|Description|
|--|--|--|--|
|**Overall Qual**|*ordinal*|train|Rates the overall material and finish of the house (1-10 with 1 being Very Poor and 10 Very Excellent)|
|**Garage Area**|*float*|train|Size of garage in square feet|
|**1st Flr SF**|*float*|train|First Floor square feet|
|**MS SubClass**|*nominal*|train|Identifies the type of dwelling involved in the sale (16 categories)|
|**Lot Frontage**|*float*|train|Linear feet of street connected to property|

# Models and Techniques
- *This section should provide more detail on the selected model(s), explain why they were chosen, how they were evaluated, and describe any technique(s) used (e.g., NLP, GridSearch, if cloud computing was used, etc).
- *Basically think of this section as a detailed answer to the question, "why did you do what you did?"
*Example:*
- The goal was to find a classification model that performed best when classifying a subreddit post. *userexperience (1)*  was the positive class and *UXResearch (0)* the negative class. In order to do this I performed various tests on the following models:
  - Logistic Regression
  - Multinomial Naive Bayes
  - Random Forest
  - AdaBoost
  - Voting Classifier
- I used two vectorizers from sklearn:
  - CountVectorizer()
  - TfidfVectorizer()
- For each model, I tested various combinations hyperparameters for the respective vectorizers. To do this I set up pipelines with the transformers and estimators (e.g, CountVectorizer, Logistic Regression), tested various hyperparameters (e.g., stop words, n-gram range), and ran this through a GridSearch.
- Each model was then fit using training data.
- Both train and test datasets were scored using accuracy and evaluated against the baseline accuracy (53% positive class; 47% negative class).

# Conclusions
- Conclusions should include: 
	- which model(s) performed best? what were their scores? what were their parameters?
    - commentary on model performance (Analysis). why do I think the models performed like this? were there certain parameters that made a significant difference? 
    - constraints and challenges
    - general qualitative commentary
*Example:*
- *Model #4: Logistic Regression, TfidfVectorizer* performed best
  - Train accuracy score: 74%
  - Test accuracy score: 72%
  - Parameters:
    - `stopwords: 'english'` (native to sklearn)
    - `n-gram range: (1,2)`
    - `solver: liblinear`
    - `penalty: l1` (lasso regression)
  - Analysis:
    - Though 72% accuracy was not the best across all test sets, it was the closest to that of the train set. For other models, I was getting test scores at least 20 percentage points less than those of train scores.
    - Therefore, I selected this model as the best as it would perform most closely with expectations on unseen data.
    - The most notable hyperparameter adjustment was changing the penalty from the default `l2` (ridge regression) to `l1` (lasso regression). All else held equal, the train/test respectively scored 92.3%/75.6% accuracy using the `l1` penalty. Allowing the coefficients to zero out using `l2` makes the train/test accuracy scores more consistent with one another.
- Though I would have liked to see a higher accuracy score in my best model, a 72% accuracy isn't bad considering how closely related the subreddits of choice were. Compared to the baseline accuracy (53% positive class) my best model performed 19 percentage points higher.
- The largest challenge with this project was computing constraints. Given the complexity of some of the models and their hyperparameters, my computer wasn't able to process everything. For example, when I tried to compare the `elasticnet` and `l1` hyperparameters for Logistic Regression, my computer ran for 20+ minutes before the Jupyter kernel froze. Given sufficient time and computing resources, I believe I could continue tuning the models to get an accuracy score well above 72%.

# Next Steps
- *The format (e.g. paragraphs or bullet points), length, and detail of Next Steps section will depend upon factors such as: the problem statement, the audience, the goal of the project, etc*
- *Example:* Before putting this model into any sort of production, the accuracy scores would need to be increased by at least 15 percentage points. To that end, next steps would be acquiring more computing power and testing multiple hyperparameters (e.g., different stopwords, n-grams, penalities, etc) over various models. Upon training a model with a sufficient accuracy score, a few interesting implementations of it might include analyzing posts to recommend posting in the other subreddit in order to get more interaction or analyzing post quality to make recommendations to employers for potential candidates. These are just a few ideas that, especially when paired with other features from the API (e.g., time of post, number of upvotes), could lead to some very interesting insights. Before any idea is to be implemented, though, the accuracy score must improve.

# Sources and References 
*If applicable, cite any sources and references (e.g., external research, datasets, quotes cited, etc)
