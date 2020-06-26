---
layout: post
title: k Nearest Neighbors in Python from Scratch
subtitle: How to develop a k Nearest Neighbor Classification Model with base packages in Python
gh-repo: Lord-Kanzler/CS-Data-Science-Build-Week-1
gh-badge: [star, fork, follow]
image: /img/KnnClassification.jpg
tags: [Python, Models, Alogrithm, Data Science]
comments: false
---

In this tutorial I will walk through a basic implementation of the **k-Nearest Neighbors algorithm** including how it works and how to implement it from scratch in Python (only using numpy). 
The k-Nearest Neighbors algorithm is a relatively simple but powerful approach for making predictions. At it's core the principle behind the k-Nearest Neighbors algorithm is to use most similar historical examples to make predictions on new data. As such it is a great starting point to learn about classification algorithms. There are no models that need training, everything is open, and each step is straight forward, and easy to follow.


### k Nearest Neighbors Overview
The k-Nearest Neighbors algorithm or KNN for short is a pretty simple technique.
The entire training dataset is stored, and when a prediction is required, the k-most similar records to a new record from the training dataset are then located. From these neighbors, a summarized prediction is made.

Similarity between records can be measured many different ways. A great starting point is the Euclidean distance since we're mostly dealing with tabular data when approaching Data Science related problems in Python. According to [Wikipedia](https://en.wikipedia.org/wiki/Euclidean_distance), it is the straight-line distance between two points. I won't go into to much details regarding Euclidian distance, but if you're interested, the Wikipedia link above is a good starting point for your research.

Once neighbors are discovered, a summary prediction can be made by returning the most common outcome or taking the average. As such, KNN are pretty versitile, and can be used for classification or even regression problems. 


### The 3 basic steps for building a k-Nearest Neighbors algorithm

This k-Nearest Neighbors implementation is broken down into 3 simple steps:

  * Step 1: Calculate the Euclidean Distance.

  * Step 2: Fit and Predict.

  * Step 3: Getting the Nearest Neighbors.

I will start, by using code snippets relevant to the individual steps to help walking through the process. Afterwards I will provide a an assembled version of the code.


### Step 1: Calculate the Euclidean Distance

The first step is to calculate the distance between two rows in a dataset. It is calculated as the square root of the sum of the squared differences between the two vectors. That's a mouth full.

Basically, all we want is to iterate through each row of a data set at a given index and subtract the two values from each other (a and b in example code). Afterwards we square the difference, and add that value to the existing distance total. 

Finally, we take the square root of the total distance to get the Euclidean distance between two points of data. Rinse and repeat for each value pair as we're going through the data.


Below is a function named euclidean_distance() that implements this in Python. 

```py
def euclidean_distance(self, a, b):

        eucl_distance = 0.0  # initializing eucl_distance at 0

        for index in range(len(a)):
        
            eucl_distance += (a[index] - b[index]) ** 2

            euclidian_distance = np.sqrt(eucl_distance)

        return euclidian_distance

```



### Step 2: Fit and Predict

The next step is to fit the KNN classifier. This step isn't strictly necessary for making predictions, but loading the entire historical data into memory is useful for the calcuation of accurate predictions in the next step.

In python this would look something like this:

```py
  def fit_knn(self, X_train, y_train):
    self.X_train = X_train
    self.y_train = y_train
```

This is now followed by implementing the predictive part:

```py
    def predict_knn(self, X):
        
        # initialize prediction_knn as empty list
        prediction_knn = []

        # # initialize euclidian_distances as empty list
        # euclidian_distances = []

        for index in range(len(X)):  # Main loop iterating through len(X)

            # initialize euclidian_distances as empty list
            euclidian_distances = []

            for row in self.X_train:
                # for every row in X_train, find eucl_distance to X using
                # euclidean_distance() and append to euclidian_distances list
                eucl_distance = self.euclidean_distance(row, X[index])
                euclidian_distances.append(eucl_distance)

            # sort euclidian_distances in ascending order, and retain only k
            # neighbors as specified in n_neighbors (n_neighbors = k)
            neighbors = np.array(euclidian_distances).argsort()[: self.n_neighbors]

            # initialize dict to count class occurrences in y_train
            count_neighbors = {}

            for val in neighbors:
                if self.y_train[val] in count_neighbors:
                    count_neighbors[self.y_train[val]] += 1
                else:
                    count_neighbors[self.y_train[val]] = 1

            # max count labels to prediction_knn
            prediction_knn.append(max(count_neighbors, key=count_neighbors.get))

        return prediction_knn
```
Let me quickly explain what this part of the code is doing. Essentially we have several **for loops. A Main loop which in turn includes several sub loops.

The Main loop iterates through the data points of X. On every iteration, the euclidean_distance() function from Step 1 is called to calculate the distance to every data point in X_train (yes there are more efficient ways to do this). 

As we are not interested in every distance value of every data point combination, we exclude most of these euclidian distance values, and only keep the k smallest values (with k being the number of neighbors). 

Finally, we just need to iterate over neighbors and count the occurences of values(labels?) and return the most frequent as our predictions. 



### Step 3: Getting the Nearest Neighbors
We have a method of making predictions, and can calculate how accurate our kNN classifier is. Ideally we need a method to simply return predicted neighbors.

The function below returns a list of tuples containing k nearest neighbors and the euclidian distances to the corresponding value inputs.

```py
    def display_knn(self, x):
        """
        Inputs: x : vector x

        Output: display_knn_values : returns a list containing nearest
        neighbors and their correscponding euclidian distances
        to the vector x wrapped in tuples
        """

        # initialize euclidian_distances as empty list
        euclidian_distances = []

        # for every row in X_train, find eucl_distance to x
        # using euclidean_distance() and append to euclidian_distances list
        for row in self.X_train:
            eucl_distance = self.euclidean_distance(row, x)
            euclidian_distances.append(eucl_distance)

        # sort euclidian_distances in ascending order, and retain only k
        # neighbors as specified in n_neighbors (n_neighbors = k)
        neighbors = np.array(euclidian_distances).argsort()[: self.n_neighbors]

        # initiate empty display_knn_values list
        display_knn_values = []

        for index in range(len(neighbors)):
            neighbor_index = neighbors[index]
            e_distances = euclidian_distances[index]
            display_knn_values.append(
                (neighbor_index, e_distances)
            )
        # print(display_knn_values)
        return display_knn_values
```



### Comparing this basic build with a kNN Classifier from Scikit-learn

So, we have build a a working kNN classifier algorithm from scratch. Let's see how it stacks up against one of the most commonly used kNN classifier, Scikit-learn.

***Loading the relevant Scikit-learn packages, and preparing some sample data
```py
# Import packages
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.neighbors import KNeighborsClassifier
from sklearn.metrics import accuracy_score

# Load data
iris = load_iris()
data = iris.data
target = iris.target

# Splitting the data into Train and Test sets
X_train, X_test, y_train, y_test = train_test_split(data, target, test_size=0.3)
```


***kNN Classifier from Scikit-learn***
```py
# Instantiate model
clf = KNeighborsClassifier(n_neighbors=10)
# Fit
clf.fit(X_train, y_train)
# Prediction
predict = clf.predict(X_test)
# Accuracy Score
print(f"Scikit-learn KNN classifier accuracy: {accuracy_score(y_test, predict)}")
```
Scikit-learn KNN classifier accuracy: 0.9555555555555556


***kNN Classifier build from Scratch***
```py
# Instantiate model
classifier = k_nearest_neighbors(n_neighbors=10)
# Fit
classifier.fit_knn(X_train, y_train)
# Prediction
predict = classifier.predict_knn(X_test)
# Accuracy Score
print(f"Build k_nearest_neighbors model accuracy: {accuracy_score(y_test, predict)}")
```
Build k_nearest_neighbors model accuracy: 0.9333333333333333

NOT TO SHABBY!

### Summary
We have implemented a simple but reasonably accurate version of a kNN classification algorithm in python. It performs very similarly to Scikit-learn kNN KNeighborsClassifier.

Files for the full implementation of this kNN classification algorithm as well as Usage instructions and functional test data examples can be found in my [GitHub Repo](https://github.com/Lord-Kanzler/CS-Data-Science-Build-Week-1).



