# PDA Homework 3 Report

Each classifier has its own code file.

## Getting started

Wait, before we start, I just want to tell you that I misunderstood the question all the way until 3 days before the deadline. I thought we are predicting the "close price". That makes me stucked at the first classifier for so long... 

Ok, now I am clear that we have to predict the "up and down" of the stocks, so we have to make our own Y play the game.

### Make Y for the dataset

Since our train and test data are downloaded separately in advance. We don't have to split a raw data (Although I leave the split function in my code), so we just need a function that helps up to get the Y.

Y "Up and Down" : If today's close price is greater than yesterday's. Y is 1. Otherwisw, 0.

We start it from the second day, the first day we make a default 0 for it (first day no close price rise).

Implementation down below:
```
# iterate over rows with iterrows()
close_prices = x_train['Close Price']
up_down_train = [0]
for i in range(1, len(close_prices)):
    if close_prices[i] - close_prices[i-1] > 0:
        up_down_train.append(1)
    else:
        up_down_train.append(0)
up_down_train

y_train = pd.Series(up_down_train)

close_prices = x_test['Close Price']
up_down_test = [0]
for i in range(1, len(close_prices)):
    if close_prices[i] - close_prices[i-1] > 0:
        up_down_test.append(1)
    else:
        up_down_test.append(0)
up_down_test

y_test = pd.Series(up_down_test)
```

Well now we have x_train, y_train, x_test, y_test

# Review on each classifier

I will discuss the classifier one by one, and give some review or answer specifically. Any other question not answered in this section, I may answer them in next section.

## Logistic Regression

I did Ridge Regression, but the result is not like the tutorial, I didn't get a convex like graph for the regression. And the min alpha is just 0.

Then, print the coefficients:
```
Coefficient for Open Price:	-0.030494102247274543
Coefficient for Close Price:	0.020299523321612965
Coefficient for High Price:	0.004630137630256997
Coefficient for Low Price:	0.005569014766322785
Coefficient for Volume:	5.539475520257828e-12
```
Well, first we get alpha = 0, then now coefficient are all small.

So look for info from the internet I get :
> So, if the alpha value is 0, it means that it is just an Ordinary Least Squares Regression model. 

and
> Ridge Regression is a remedial measure taken to alleviate multicollinearity amongst regression predictor variables in a model. Often predictor variables used in a regression are highly correlated.

They are about Ridge regression are used to do feature selection, but I know the high coefficient is the one to keep, but all coefficients came out of this dataset are all much lower than tutorial one.

**Version 1**:
- How did I process the data:
    - Discard Date immediately after reading the csv
- Result:
    - Train accuracy: 0.5459363957597173
        Test accuracy: 0.5198412698412699
        
**Version 2**:
- How did I process the data:
    - Dropping Volume : As I said all features seem like removable, so i picked the magnitude witch is the smallest. 
- Result:
    - Train accuracy: 0.9394876325088339
        Test accuracy: 0.8214285714285714
        The result are promissing good, some how..
        
**Do it with Titanic dataset**:
Train accuracy: 0.7979041916167665
Test accuracy: 0.7937219730941704

**How did I improve this classcifier**:
`clf=linear_model.LogisticRegression(solver='lbfgs',max_iter=100000, tol=0.00000001).fit(x_train, y_train`
only train accuracy rised a little bit.
Train accuracy: 0.9403710247349824
Test accuracy: 0.8214285714285714

## Support Vector Machine

When I first try the SVM, I can't run the model, the model ran endlessly. After noticing the tutorial data make 4 features to 2 features, so I do it the same way with the stocks dataset.

**What did I preprocess the data**:
Same as above, make x_train y_tain x_test y_test at once. This time to make the SVM at least working. I kept only "Close Price" and "Open Price"

**Result for SVM**:
Penalty = 0.05, Train Accuracy = 93.77 %
Penalty = 0.05, Test Accuracy = 82.54 %

**How did I improve this classcifier**:
increase penalty to 5
Penalty = 5.00, Train Accuracy = 93.60 %
Penalty = 5.00, Test Accuracy = 82.94 %
## Neuron Network

This classifier makes me wonder the most, for my aknowledgement, I thought NN will bbe most accuracy one, but in fact, it is the worst one.

**What did I preprocess the data**:
Same as above, make x_train y_tain x_test y_test at once. 
I did use tensorflow and keras before, we have to make data flat everytime we used it. So which is, normalize the training data.

**Result for NN**:
Training accuracy: 0.5459363958650258
Testing accuracy: 0.5198412703143226
![](https://i.imgur.com/nHZXK7K.png)


**Do it with Titanic dataset**:
Training accuracy: 0.7020958083832335
Testing accuracy: 0.7219730912302642
![](https://i.imgur.com/N1mNHbY.png)

**How did I improve this classcifier**:
Tweak the parameters
- increase hidden node
- increase epochs

But actually I can;t say its an improvement for the stocks dataset. The accuracy are mostly the same.

We can see the stocks learning curve are progressed badly where as titanic is stable.


## Best classifier - SVM

**Why?**
First we discuss two near result SVM and Logistic Reggression

> SVM try to maximize the margin between the closest support vectors while LR the posterior class probability. Thus, SVM find a solution which is as fare as possible for the two categories while LR has not this propert

SVM has a more balanced boundary between two categories.
So we can say SVM are getting a slightly better result than LR. 

But both SVM and Logistic Regression work better than Neuron Network. 
From my understanding, the problem must be the dataset itslef, we can see there is an unually learning curve occured in training for the stocks, while the titanic has normal loss curve.

I think the dataset like stocks for this homework is hard to predict by any mean. But SVM and LR can predict better, I think another reason is that the stocks dataset is small. It is an advantage for SVM and LR, but disadvantage for neuron network.

**Compare with titanic and what happend**

Titanic dataset is smaller than stocks too, but the winner for accuracy is still the LR (I did do titanic in SVM, it takes time). But this time neuron network for titanic did pretty well too, I think if we give large margin of dataset for neuron network and know how to tweak the model, NN might do it better. 

