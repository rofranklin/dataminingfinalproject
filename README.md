# Data Mining Final Project: Microsoft Malware Prediction Kaggle Competition
### Introduction
For the data mining final project, I participated in the Microsoft Malware Prediction Kaggle competiton that tasks us to find the probability a machine will be hit with malware based on the given data. 
- data includes machine id, city id, av state, av identifier, chassis type and about 40 others
- my main challenges were dealing with the amt of data (4gb for train) in a google colab environment, the quality of the data - many columns were mostly empty, and the sampling of the data as the train and test set was separted by several months in time, leading to inconsistencies in the train vs. test results

### Reading in the Data
- for data exploration, there was no way to get around reading in the entire data set and analyzing
- analysis by itself took about  8/12 gb of my ram on colab using normal pandas and seaborn
- created a sql lite data base to do large scale transformations bc that is what sql is used for

### Data Exploration
- walked through trying to find what most was the most correlated features
- faced the problem of determining data types for the features - kernels helped here to determine data type
- used seaborn to explore correlation, most looked random but a few like city identifier, av state were more positively correlated
- Also did feature importance analysis on the models and the results.

### Models and Methods
- it rapidly became unwieldy to try and train a model on all features, so I used several kernels that had a subset (around 28 features) compared to the 80
- super blend kernel - this kernel extracted the timestamp from from the av signature column and that along with the ensembling the output of another light gbm classifier that uses only a few features - about 20 features
- k fold cross validation with count encoding and a light gbm classifier - around 20 features and about 20% of the data, sampled randomly - the time separation strikes again! The data seems random, but my results were much poorer when I didn't randomly sample the train data for decreased procssing time.
- more light gbm - with an added frequency encoding! allowed for memory reduction but was overall not very successful in increasing the score. Incorrectly identified smart screen setting on computer as being most important - not likely correct. frequency encoding is a way to utilize the frequency of the categories as labels. In the cases where the frequency is related somewhat with the target variable, it helps the model to understand and assign the weight in direct and inverse proportion, depending on the nature of the data.
- based on the output of previous light gbm classifiers, I decided to take the top 4/5 features of importance and see if that worked 
- tried first doing 5 and including smart screen however, it was shown that the frequency encoding skewed the smart screen cateogory and dropped accuracy to like .62 - not great
- tried again swapping that out for av state and increased score back up to about .65 - not horrible
- overall the best was super blend! super blend got about .71 and the highest before the competition ended was .68 because of the time separation and overfitting to the training set

### Practical Applications 
- There is a similar project going on in the OSE space - so prior to conducting an engagement on a system or application, there is a bunch of reconnaisance that occurs where the engineers try to find out as much basic information about the system or application as possible. The problem is reconassance is actually what takes the most time in an engagement. Because of that, the team is looking to automate parts of reconassance. The output of this project, the probability a machine is compromised can be used to guide the engagement to go faster, and target the areas that have been shown to be weak.

