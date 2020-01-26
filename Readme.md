
### Amazon Fine Food Reviews

__Objective:__ 

The data for this project comes from a dataset posted on [kaggle.com]. We are using the data to explore text mining, feature engineering and build a model to identify which features make a review on amazon helpful. This will not only help amazon in positioning their reviews in a way that improves customer's purchasing decision but also serve as a guide to writing helpful reviews.

To do so, we will work on the recommendation dataset to encode the text data using Bag of Words and count vectorization. When the data is ready, we will run a logistic regression model to find the most important features. 

[kaggle.com]:https://www.kaggle.com/snap/amazon-fine-food-reviews

__Data Overview:__ 

The dataset consists of ~500,00 reviews of fine foods from amazon. The data span a period of more than 10 years. Reviews include the following:

Variable: Description || Type of Variable
* __ProductId:__ Unique id of the product being reviewed || string
* __ProfileName:__ Profile name of the user || string
* __HelpfulnessNumerator:__ number of users who found the review helpful || continuous
* __HelpfulnessDenominator:__ number of users who indicated whether they found the review helpful or not helpful || continuous
* __Score:__ rating between 1 and 5 || categorical
* __Summary:__ Brief summary of the review || text
* __Text:__ text of the review || text

__Analysis Approach:__

* __I. Understanding the data__
    * Creating Y variable: (Useful)__
    We wanted to create a binary variable that will indicate whether a particular review was useful or not. In order to do so we followed the following approach:
   1. Step 1: Compute the helpfulness i.e. fraction of total users that found the review helpful for each review as follows: 
         helpfulness = (HelpfulnessNumerator / HelpfulnessDenominator)
   2. Step 2: 
       * Decide the threshold to create the variable. In order to do this, we inspected the distribution of data and observed that ~80% of the dataset had helpfulness greater than 50%. That is we cannot take 50% as the threshold.
       * In order to avoid creating an imbalanced dataset, we decided to subset the data by taking only __black and white__ areas. We filtered the records which had helpfulness as __1 - these will be the helpful reviews__ and helpfulness of __less than 0.4 - these will be the not helpful reviews__ 

* __II. Data Cleaning__ 
* __III. Feature Engineering:__
    * Length of reviews:  The total length of review might impact the reader's interest in reading the review and hence impact its usefulness. The length of review has been computed by counting the number of words in a review.
    * Words as features
        * __Step 1:__ Removing stop words (most commonly used english words like and, are, am, you etc.) from the reviews as we are interested only in the important words used in the reviews which might impact its usefulness. 
        * __Step 2__: Once the stop words are removed, we convert the words to its root form. This can be achieved using one of the two approaches:
         1. Stemming: converting the words to their pseudo root form. Eg: preparation, preparing becomes 'prepar'.
          2. Lemmatization : converting words to their root word which is an actual language word. Eg: preparation, preparing becomes 'prepare'. You may read in detail about stemming and lemmatization [here]
        * __Step 3: Bag of Words__ Creating vectors for words in reviews. This means that each word becomes a column with the value equal to number of times that word appears in that review. In order to so, we will use single matrix bag of words technique.
        * __Step 4: Bigram:__ In BOW it doesn't consider related words. For example, if we consider two reviews __This product is very good__ and __This product is not good__ after stopwords removal both sentences will be converted to product good so both giving exact same meaning. The main problem is here we are not considering the front and back words related to every word. So we use Bigram which will consider pair of words. 
    * Score as dummy variables: Converting score (1-5) into dummy variables because we want to evaluate the impact of each of the score independently.
    
* __IV. Preparing X and Y for model building**__
* __V. Model building__ - We run the models to test the significance of coefficients. We started with logit model that will let us know if a feature (word) is significant or not. Models tried: Logit, Logistics Regression, Gridsearch Logistics**

[here]:https://towardsdatascience.com/stemming-lemmatization-what-ba782b7c0bd8

** - Repeating step 4 and 5 for both bag of words (each word as a feature) and bigrams (single word as well as combination of two words).


__Interpretation and Finding:__

* From the analysis done above, it can be inferred that, with __increase in the product rating, the usefulness increases__. However, this seems to be __counterintuitive as extreme product rating(1,5) should also have indicated high usefulness.__
* For Eg: The review with product rating '1' indicates a strong opinion about the purchase made, thus helping the customer reading the review make an informed decision. This is synonymous in terms of __helpfulness__ with a review with product rating '5'.
* This can be because customers are biased towards marking a review as not useful when they perceive the product quality to be low. This indicates that there is a correlation between the usefulness indicator and product quality
* Though in general the trend for Usefulness for lower ratings (1,2,3) is decreasing, further analysis of interaction between some of the top words and score revealed the following:
    * If the rating is greater than 4, the appearance of any word does not significantly impact the usefulness 
    * However, if the review has opinion words  like 'great','best' etc., then with the appearance of the words, the usefulness seems to increase even if the product rating provided is lower
    * Also, if the words are neutral and ambiguous like 'suppose','anyway' etc. the usefulness seems to decrease with the appearance of the word
    

__Managerial Insights:__

* Using the above finding, the positioning of the reviews can be changed so that they are most useful to the customers and can help customers make an informed purchase decision.For Eg: The reviews with highest rating and the ones which consists of superlative words should be displayed on the top
* As the customers are correlating the helpfulness and bad product rating, the option to vote 'Not helpful' should be removed as it is resulting in biased information.
* Also, a guide can be published for the customers on how to write an impactful review


__Future Scope:__


* Creating additional features like special characters count, presemce of smileys/emoticons etc.
* Developing algorithm to predict if a review is helpful or not. Lasso, random forest classifier can be used as the dataset has high dimensionality.
