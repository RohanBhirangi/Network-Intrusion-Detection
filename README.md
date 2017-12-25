# Network Intrusion Detection

* The problem is to identify whether a network connection is good or malicious.
* We are building a predictive model distinguishing intrusions or attacks from normal connections.
* The data set contains nine weeks of TCP dump data peppered with multiple attacks.
* Each connection is labelled as 'normal' or as the attack type.

## KDD CUP 1999 DATA SET

We will use 3 data sets for training and testing:<br>
* kddcup.data - Full data set<br>
* kddcup.data_10_percent - 10% subset of the full data set<br>
* corrected - Test data with labels<br>

The data sets can be downloaded at [http://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html](http://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html).

Since our data set does not have headers, we will be using an additional file to extract column names and attack type names for the training data set:<br>
* kddcup.names - List of features (column names) and attack types

## Summary
We started by loading and preprocessing the data. While preprocessing the data, we had to consider the categorical features, scale numeric features and make sure that we had the same categorical columns for both the training and test data sets. One thing that we observed was that the data set had many redundant rows. We dropped such redundant rows. We also checked for any missing values and converted the mutli-class connection types to binary labels.

Then, we did some exploratory analysis on the data. We found out that the number of normal connections significantly dominated the attacks. Thus there was a class imbalance. This made us not rely on accuracy alone for gauging the goodness of a model. We plotted some histograms to observe the distribution of numeric attributes in the data and find how those features were correlated to the target variable. The other thing that we concluded was that we had to be careful about false negatives in the system. All these conclusions made us choose the Area Under Curve as a reliable metric for our the models.

## Inference and Results
We trained 5 different classifiers of which Random Forest, Gradient Boosting and Multi Layer Perceptron gave us the best results. These models gave a high Area Under Curve (0.95) and were able to minimize the false negative rate.
We figured out the feature importances of Random Forest and Gradient Boosting which led to the conclusion that basic TCP features and network traffic features were the most important features. 

Most notably the 'count' value which was the number of connections to the same host as the current connection in the past two seconds. Two very notable features were same_srv_rate which denotes the % of connections to the same service, and diff_srv_rate which denotes otherwise. Another very important feature was src_bytes attribute, which denotes the number of bytes transferred from the source to destination was also notable.

We then tried to tweak those models by tuning its parameters and testing them for different values which did not show much significant improvements in results. At this point, Random Forests is the best performer, but in the long term for more incoming data and eager learning, multi layer perceptron would be more robust due to it's ability to recognize even complex underlying patterns.