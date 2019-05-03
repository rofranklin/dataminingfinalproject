# Data Mining Final Project: Microsoft Malware Prediction Kaggle Competition
### Introduction
For the data mining final project, I participated in the Microsoft Malware Prediction Kaggle competiton that tasks us to find the probability a machine will be hit with malware based on the given data. There were 79 features, including machine id, city id, av state, av identifier, chassis type and many others. My main challenges were dealing with the amount of data, the quality of data, and the sampling of the data. The training set alone was about 4gb and I was working in a google colab environment. Loading data itself took about a minute and led to the discovery of sampling the data to process faster. The quality of data was lower than the kaggle class competition and several columns were over 50% empty and led to them being discarded. Finally, the train and test data were separated by months in time, leading to a lot of overfitting to the test data and low private leaderboard scores. I feel that this sampling was especially bad for malware detection because malware is a time sensitive problem and based on how quickly a new exploit is released and patched. There was also a lot of discussion in the kaggle forums about the fairness of the competition and the low overall accuracy of the results. My highest private leaderboard score was about .71 and this was done after the competition had ended. During the competition, the highest overall score was .68 which is a testament to the overall difficulty of the problem. 

### Reading in the Data
For data exploration, there was no way to get around reading the entire data set and seeing what features were present and their correlations to each other. Analysis, including calculation of the correlation and visualizing that correlation. Analysis by itself took 8/12 GB of the google colab ram, and it did need to be reset after using pandas and seaborn to explore the data. To assist in this, I created a sql lite database to process this data, because it is made to do large scale data transformation. 
Example using 24 Features used in a later LGBM model seen in notebook - github won't let me host a picture on the wiki.  


### Data Exploration
To begin the data exploration, I tried to determine what were the most correlated features with the output and what features were highly correlated with each other to reduce the number of columns needed. I also faced the problem of loading and determining data types. I based my data types on a kernel that was posted, and adjusted to make it use less memory. I used pandas/seaborn to explore correlation. Most were uncorrelated with the output by itself, but some had very weak correlation, like city identifier, to the final output. I also did feature analysis on the the results and created another model based on the additional features. 

### Models and Methods
It rapidly became unwieldy to try and train a model on all features, so I used several kernels that had a subset (around 28 features) compared to the 82
- Super Blend Kernel - this kernel extracted the timestamp from from the av signature column and that along with the ensembling the output of another light gbm classifier that uses only a few features - about 20 features. Very successful overall and yielded the highest score of the competition. However, this was very compute heavy because we were adding on the existing data frame and I didn't downsample. 
- K fold cross validation with count encoding and a light gbm classifier - around 20 features and about 20% of the data, sampled randomly - the time separation strikes again! The data seems random, but my results were much poorer when I didn't randomly sample the train data for decreased processing time. Overall performance was .67, which puts it at the upper end of the the private leaderboard before the the end of the competition.
- Light gbm - with an added frequency encoding! This allowed for memory reduction but was overall not very successful in increasing the score. Incorrectly identified smart screen setting on computer as being most important - not likely correct. Frequency encoding is a way to utilize the frequency of the categories as labels. In the cases where the frequency is related somewhat with the target variable, it helps the model to understand and assign the weight in direct and inverse proportion, depending on the nature of the data.
- Ultra-Light GBM Classifier: Based on the output of previous light gbm classifiers, I decided to take the top 4/5 features of importance and see if that worked 
  - tried first doing 5 and including smart screen however, it was shown that the frequency encoding skewed the smart screen cateogory and dropped accuracy to like .62 - not great
  - tried again swapping that out for av state and increased score back up to about .65 - not horrible
- Overall the best was super blend! super blend got about .71 and the highest before the competition ended was .68 because of the time separation and overfitting to the training set

### Practical Applications 
- There is a similar project going on in the OSE space - so prior to conducting an engagement on a system or application, there is a bunch of reconnaissance that occurs where the engineers try to find out as much basic information about the system or application as possible. The problem is reconnaissance is actually what takes the most time in an engagement. Because of that, the team is looking to automate parts of reconnaissance. The output of this project, the probability a machine is compromised can be used to guide the engagement to go faster, and target the areas that have been shown to be weak.
