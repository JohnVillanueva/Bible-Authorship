# Bible Exploration - Predicting NT, OT, and Authors

### Background
The purpose of this notebook is merely to practice and test NLP and unsupervised clustering methods. Using the Bible as text data we utilize BOW and TFIDF vectorization, SpaCy, and topic modeling using various matrix decomposition methods in order to tune two different predictive models.

### The Data
The raw data is pulled directly from the [Bible API github raw data](https://raw.githubusercontent.com/bibleapi/bibleapi-bibles-json/master/asv.json). Individual Bible verses were consolidated to create a dataframe with each row representing an individual book_chapter of the Bible. Bible Authors were manually compiled from various online resources and appended to their corresponding book_chapters. The testament feature (New Testament vs. Old Testament) was also created as the outcome variable for one of the models.

Miscellaneous Data Cleaning Details:
Authors with less than 10 book_chapters were removed from the dataset, providing the author prediction model enough data to learn about the authors it's trying to predict.
All text was parsed and tokenized using SpaCy with default stopwords removed. The text was parsed to include known entities, noun_phrases, lemmatized tokens, and POS tags.

### Goals:
1. Tune model that predicts whether or not a Bible Chapter is in the OT or NT based only on the text.
2. Evaluate different clustering methods (Kmeans, Spectral Affinity) using scree plots, silhouette scores, and SSE scores, thus optimizing the number of topics for topic modeling.
3. Tune model that predicts the book_chapter author based solely on text. Optimize topic modeling method, supervised learning model, and document vectorizing method.

## Part 1. Predicting OT vs. NT
Comparing BOW and TFIDF document vectors on a logistic regression classifier, BOW features returned an accuracy of 98.7% while TFIDF features returned a 93.7%. While they are both impressive accuracies, I'm skeptical of the BOW accuracies since ultimately a smaller data set was used. The BOW vectors were vectorized with SpaCy which could only parse a small portion of the data, thus constraining the training and test data for the BOW vectorized model. This is a great accuracy given that the number of 

## Part 2. Optimizing Number of Topics
The initiale inclination is that the optimal number of topics should be equal to the number of authors included in the data set (22 Authors). To rigorously evaluate the best number of topics, we created scree plots for silhouette scores, SSE scores, and explained variance ratios over increasing number of topics. Scree plots were generated using KMeans, Mean Shift, and Sprectral Affinity clustering methods. For SSE and EVR scree plots, 22 topics were subjectively good topic numbers to declare elbows. However, Silhouette Score Scree Plots across clustering methods showed that the score always increased with number of topics or leveled out around 100 topics. With the silhouette score being the most robust unsupervised cluster evaluation method, we chose 100 topics as the optimum number of topics.

## Part 3. Predict Authors of each Bible Chapter
Models were created over every combination of vectorization method (BOW, TFIDF), Topic Modeling Method (PCA, LSA, LDA, NNMF), and Modeling Method (Random Forest Classifier, Gradient Boosting Classifier).
We found that the method combination that returned the greatest predicitive accuracy was a Random Forest Classifier using TFIDF features and LSA Topic Modeling, ultimately returning a 75.7% accuracy across 22 different authors of the Bible.

### Conclusions
While a direct use case for the predictive models was never intended, it was very interesting to see how the clustering methods grouped Bible chapters together. Often, the clusters very clearly clustered together the same and similar authors. Some clusters would group together seemingly very different authors and Bible Books, and upon further inspection of the bible_chapters that were grouped together, there were thematic similarities between those seemingly disparate Bible texts.

Unsupervised clustering of Bible chapters could potentially be used for academic or religious studies of Bible texts when considering how Bible verses, chapters, and authors are similar and different in their writings.
