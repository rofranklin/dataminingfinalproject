# Data Mining Final Project: Microsoft Malware Prediction Kaggle Competition
### Introduction
For the data mining final project, I participated in the Microsoft Malware Prediction Kaggle competiton that tasks us to find the probability a machine will be hit with malware based on the given data. 
- data includes machine id, city id, av state, av identifier and about 40 others
- my main challenges were dealing with the amt of data (4gb for train) in a google colab environment, the quality of the data - many columns were mostly empty, and the sampling of the data as the train and test set was separted by several months in time, leading to inconsistencies in the train vs. test results

### Reading in the Data
- for data exploration, there was no way to get around reading in the entire data set and analyzing
- analysis by itself took about  8/12 gb of my ram on colab using normal pandas and seaborn
- created a sql lite data base to do large scale transformations bc that is what sql is used for

### Data Exploration
- walked through trying to find what most was the most correlated features
- faced the problem of determining data types for the features - kernels helped here

