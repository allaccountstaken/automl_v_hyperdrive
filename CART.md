# Trees... Wisdom of the crowd, it is all about reducing entropy. But how?
Think about Titanic dataset: first split on gender, second split on age, third split on class. Splits are done based on a simple majority vote for classification tasks or averging for regression tasks.Tree is the model and it also gives you feature importance. Definition of entropy - measure of disorder - split the tree to reduce disorder, i.e. gain information. Problem is that it is gready, locally optimal decisions are made at each point, thus 2 versions of a tree possible for the same data. Big problem. Also, trees are not stable and tend to overfit.

Trees generally work well with categorical data and do not require dummy variables, but can be bias towards categories with more levels.

1. Example from finance, rotation forest used for interest rate modeling: first run PCA on all available zeros and after that train one tree on 3 most important factors, i.e. shift, tilt, flexing.

2. Another example is resampling portfolios for efficient frontier construction. As Markowitz framework is based on estimated sample mean and variance, resampling helps to improve accurace. Consider using subsets of available data (jackknifing) or drawing randomly with replacement from a set of data points (bootstrapping).

Sidenote on bigger picture multiple classifier systems or model averaging: bagging (bootstrap aggregating - resampling with replacement and avergaing), boosting (incremental learning on misclassified instances and weighted voting), buckets of models (random split the sample train on A test on B, select best model), stacked models (output becomes input, think recursive neural networks maybe, typically logistic-regression is used as a combiner), Bayesian model averaging and combination (probability of data given the model).

Sidenote of financial tabular data: In prediction problems involving unstructured data (images, text, etc.), artificial neural networks tend to outperform all other algorithms or frameworks. However, when it comes to small-to-medium structured/tabular data, decision tree-based algorithms are considered best-in-class right now. (source: https://www.mygreatlearning.com/blog/gradient-boosting/)

#### Bagging and Boosting are both ensemble methods in Machine Learning, but what’s the key behind them?

Bagging and Boosting are similar in that they are both ensemble techniques, where a set of weak learners are combined to create a strong learner that obtains better performance than a single one. Generally speaking, with 2 sources of error (bias and variance), bagging seeks variance reduction for a complex model and boosting aims for bias reduction for a simple model.

## What is an ensemble method?
Ensemble is a Machine Learning concept in which the idea is to train multiple models using the same learning algorithm. The ensembles take part in a bigger group of methods, called multiclassifiers, where a set of hundreds or thousands of learners with a common objective are fused together to solve the problem.

The second group of multiclassifiers contain the hybrid methods. They use a set of learners too, but they can be trained using different learning techniques. Stacking is the most well-known. If you want to learn more about Stacking, you can read my previous post, “Dream team combining classifiers“.

The main causes of error in learning are due to noise, bias and variance. Ensemble helps to minimize these factors. These methods are designed to improve the stability and the accuracy of Machine Learning algorithms. Combinations of multiple classifiers decrease variance, especially in the case of unstable classifiers, and may produce a more reliable classification than a single classifier.

To use Bagging or Boosting you must select a base learner algorithm. For example, if we choose a classification tree, Bagging and Boosting would consist of a pool of trees as big as we want. 

## How do Bagging and Boosting get N learners?
Bagging and Boosting get N learners by generating additional data in the training stage. N new training data sets are produced by random sampling with replacement from the original set. By sampling with replacement some observations may be repeated in each new training data set.

In the case of Bagging, any element has the same probability to appear in a new data set. However, for Boosting the observations are weighted and therefore some of them will take part in the new sets more often: 

These multiple sets are used to train the same learner algorithm and therefore different classifiers are produced.

## Why are the data elements weighted?
At this point, we begin to deal with the main difference between the two methods. While the training stage is parallel for Bagging (i.e., each model is built independently), Boosting builds the new learner in a sequential way:

In Boosting algorithms each classifier is trained on data, taking into account the previous classifiers’ success. After each training step, the weights are redistributed. Misclassified data increases its weights to emphasise the most difficult cases. In this way, subsequent learners will focus on them during their training.

## How does the classification stage work?
To predict the class of new data we only need to apply the N learners to the new observations. In Bagging the result is obtained by averaging the responses of the N learners (or majority vote). However, Boosting assigns a second set of weights, this time for the N classifiers, in order to take a weighted average of their estimates.

In the Boosting training stage, the algorithm allocates weights to each resulting model. A learner with good a classification result on the training data will be assigned a higher weight than a poor one. So when evaluating a new learner, Boosting needs to keep track of learners’ errors, too. Let’s see the differences in the procedures:

Some of the Boosting techniques include an extra-condition to keep or discard a single learner. For example, in AdaBoost, the most renowned, an error less than 50% is required to maintain the model; otherwise, the iteration is repeated until achieving a learner better than a random guess.

The previous image shows the general process of a Boosting method, but several alternatives exist with different ways to determine the weights to use in the next training step and in the classification stage. Click here if you like to go into detail: AdaBoost, LPBoost, XGBoost, GradientBoost, BrownBoost.

## Which is the best, Bagging or Boosting?
There’s not an outright winner; it depends on the data, the simulation and the circumstances.
Bagging and Boosting decrease the variance of your single estimate as they combine several estimates from different models. So the result may be a model with higher stability.

If the problem is that the single model gets a very low performance, Bagging will rarely get a better bias. However, Boosting could generate a combined model with lower errors as it optimises the advantages and reduces pitfalls of the single model.

By contrast, if the difficulty of the single model is over-fitting, then Bagging is the best option. Boosting for its part doesn’t help to avoid over-fitting; in fact, this technique is faced with this problem itself. For this reason, Bagging is effective more often than Boosting.

## To sum up:

-   Both are ensemble methods to get N learners from 1 learner…     … but, while they are built independently for Bagging, Boosting tries to add new models that do well where previous models fail.

-   Both generate several training data sets by random sampling…        … but only Boosting determines weights for the data to tip the scales in favor of the most difficult cases.

-   Both make the final decision by averaging  the N learners (or taking the majority of them)…         … but it is an equally weighted average for Bagging and a weighted average for Boosting, more weight to those with better performance on training data.

- Both are good at reducing variance and provide higher stability…      … but only Boosting tries to reduce bias. On the other hand, Bagging may solve the over-fitting problem, while Boosting can increase it.

source: https://quantdare.com/what-is-the-difference-between-bagging-and-boosting/


## Deep dive into GradientBoostingClassifier with Decision Trees as weak learners
Parameters: learning_rate,  n_estimators, max_features, max_depth

Disatvantages: While boosting can increase the accuracy of a base learner, such as a decision tree or linear regression, it sacrifices intelligibility and interpretability.[1][21] Furthermore, its implementation may be more difficult due to the higher computational demand.

Difference between Gradient Boosting and Adaptive Boosting(AdaBoost)
Gradient boosting    Adaptive Boosting
This approach trains learners based upon minimising the loss function of a learner (i.e., training on the residuals of the model) 
This method focuses on training upon misclassified observations. Alters the distribution of the training dataset to increase weights on sample observations that are difficult to classify.

Weak learners are decision trees constructed in a greedy manner with split points based on purity scores (i.e., Gini, minimise loss). Thus, larger trees can be used with around 4 to 8 levels. Learners should still remain weak and so they should be constrained (i.e., the maximum number of layers, nodes, splits, leaf nodes)

The weak learners incase of adaptive boosting are a very basic form of decision tree known as stumps.
All the learners have equal weights in the case of gradient boosting. The weight is usually set as the learning rate which is small in magnitude.    The final prediction is based on a majority vote of the weak learners’ predictions weighted by their individual accuracy.
