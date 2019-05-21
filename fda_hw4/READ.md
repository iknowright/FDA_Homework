# FDA Homework

## Dataset chosen : DOTA 2

![dota](https://imgur.com/pGT42n3.png)

### More implementation detail on code file

## About The Data

Attribute Information:

Each row of the dataset is a single game with the following features (in the order in the vector):
1. Team won the game (1 or -1)
2. Cluster ID (related to location)
3. Game mode (eg All Pick)
4. Game type (eg. Ranked)
5 - end: Each element is an indicator for a hero. Value of 1 indicates that a player from team '1' played as that hero and '-1' for the other team. Hero can be selected by only one player each game. This means that each row has five '1' and five '-1' values.


## Problem - WIN or LOSE
Now let me introduce the problem I propose for the dataset, to predict WIN or LOSE of a game

## Method - Logistic Regression

Why logistic regression?
My first attempt is to use neuron network (keras), but the result seems all wrong, the prediction accuracy stays 0, and the loss maintain at 0.8549 (something like that) Then, SVM I have a horrible momory about it, since it too a lot of time to train, I dare not to use it first, and probably not doing clustering for now.

## 1 - Logistic Regression - Base Model

#### Did not do any data preproccessing

#### Review - The accuracy is actually quite high ... 
Method | Train Accuracy | Test Accuracy |
| -------- | -------- | -------- |
| Base Line | 0.6009390178089584   | 0.5976296871964251     | 
## 2 -  Logistic Regression - Taking out some Hero by my own rule

### My Rule - Ignore the less chosen heros

We have 90000++  entries and some of hero be less chosen ones, I think they are not important, so i filter the hero with selection up to 10000 times and discard those which are not.

Function taken
```
list_attr = []

for i in range(3,116):
    series = data_train[i+1].value_counts()
    series = series.drop([0], axis=0).sum(axis=0)
    if series > 10000:
        list_attr.append(i+1)

print(list_attr)
print(len(list_attr))
```

### Review

The result is the worst in this assignment.

### Reason

Actually in DOTA, every hero counts, I mean, we can't just delete some heros if they are less chosen ones. Every Dota hero is powerful when it is "FAT", it depends on how one player plays it, or its popularity among the all heros.

Method | Train Accuracy | Test Accuracy |
| -------- | -------- | -------- |
| Own Filter| 0.5653211009174312     | 0.5640178744899942     |

## 3 - Data Preproccessing - Dummy Attributes

### Features are not always continuous

Dummy Variables is one that takes the value 0 or 1 to indicate the absence or presence of some categorical effect that may be expected to shift the outcome.

So we make the rank dummy

### Mode is also not continuous variable

It is not only not continuous, by the described table, we can see the distribution of mode column that most of the mode played by team is 2, while there is a maximun number 9.

Just make them dummy~

### Team Region is not continuous too

The region is already a clustering result, it is kind of category. But the data is too huge, I am not making it dummy this time, but doing OneHot Encoding

Method | Train Accuracy | Test Accuracy |
| -------- | -------- | -------- |
| Dummy Variable| 0.601219643820831     | 0.5962696716533903     |

## 4 - Data Preproccessing - Normalization

### make all value 0 to 1

Maybe it is because the region column is still larger than others, so I normalize all the data and re-train

Method | Train Accuracy | Test Accuracy |
| -------- | -------- | -------- |
| Normalization on Dummy | 0.6011440906637885     |0.59636681562075     |

## 5 - Formal Feature Abstraction by Ridge Regression

### 10-fold Cross Validation

To find the best alpha for ridge model

#### After getting best ridge model, taking out some unwanted columns

#### This is the best result i can get so far

Method | Train Accuracy | Test Accuracy |
| -------- | -------- | -------- |
| Feature Abstraction by Ridge and Dummy | 0.601036157582299     | 0.599475422576258     |

## 6 - Just Use Logistic Regression CV

#### Without dropping any feature

Method | Train Accuracy | Test Accuracy |
| -------- | -------- | -------- |
| LogisticRegressionCV original data| 0.60061521856449     |0.5995725665436177    |
## 7 - Use Logistic Regression CV only Hero

Method | Train Accuracy | Test Accuracy |
| -------- | -------- | -------- |
| LogisticRegressionCV original data only heros| 0.6008310847274689     |0.5973382552943463     |

# Conclusion

## Result


Method | Train Accuracy | Test Accuracy |
| -------- | -------- | -------- |
| Base Line | 0.6009390178089584   | 0.5976296871964251     | 
| Own Filter| 0.5653211009174312     | 0.5640178744899942     |
| Dummy Variable| 0.601219643820831     | 0.5962696716533903     |
| Normalization on Dummy | 0.6011440906637885     |0.59636681562075     |
| Feature Abstraction by Ridge and Dummy | 0.601036157582299     | 0.599475422576258     |
| LogisticRegressionCV original data| 0.60061521856449     |0.5995725665436177    |
| LogisticRegressionCV original data only heros| 0.6008310847274689     |0.5973382552943463     |


## Best Score

- Train - 0.601219643820831 on Dummy Variable 
- Test - 0.5995725665436177	original data with cross validation

## What can we learn from this problem

- We can gamble the game base on the hero selection
- The result is like 60% thru out the learning, just 10% better than guessing the head or tail of a coin

## Suggestion on the data set

Attributes that could help this problem 
- Per player playing time on dota
- MMR of a player
- Game Match Time
- recent performace