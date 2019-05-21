# PDA Homework 2 Report

## Subject : Whether a person is a Credit Card Holder

### Data Generation

- ####  Data Features and Attributes
    First, we have to define some attributes for the dataset. The following are the rules and attributes I set:
    - Online Shopping Experience
        - This attribute is most important index for this experiment. It is because nowadays 90% of online purchases is paid by credit card. We don't use cash online. How much you pay interest in online shopping is directly related to how likely you have a credt card.
    - Income
        - Why income? It is because you have to get cartain amount of money to earn a credit card issue by the bank normally, and people who spend more are more likely to own a credit card. 
    - Expenses
        - We all know credit card is an evil thing to have. If one spends more than he learns, we can probably say he's having a credit card for sure.
    - Age
        - From my aknowledgement, people under 20 is not allowed to have a credit by thier own. And when you are getting older (after 65)， it will be harder and harder to apply for a credit card by age.
    - Sex
        - In my opinion, both men and women have things they obsessed on. But women are more likely to go crazy about shopping and buying stuff, which indicates they may spend money not in a rational way, credit card might help them. 

- #### Rules I set by using these attributes:
![](https://i.imgur.com/c2Gsvxx.png)


- #### Function that generate he sameple data:
    - Age generation
        - Random integer generation from 16 to 80
        - It is in a **uniform distribution**
    - Sex generation
        - Random （MALE or FEMALE) generation 
        - It is in a **uniform distribution**
    - Debit Card Holder
        - Random (Holder or Non-Holder) generation
        - It is in a **uniform distribution**
    - Income
        - It is in a **Normal distribution**
        - mean is 35k, var is 15k
    - Expenses
        - It is in a **Normal distribution**
        - mean is 30k, var is 17.5k.
        - Since some guy spends more than he earns.

- #### Function that generate he sameple data:
```
def apply_result(x):
        if x['Online Shopping Experience'] == 0 or x['Online Shopping Experience'] == 1:
            if x.Income > 100000:
                return 'YES'
            else:
                if x.Age < 25:
                    return 'NO'
                else:
                    if x.Expenses > 50000:
                        return "YES"
                    else:
                        return "NO"
        elif x['Online Shopping Experience'] == 2 or x['Online Shopping Experience'] == 3:
            if x.Income > 75000:
                return "YES"
            else:
                if x.Age < 25:
                    if x.Sex == 'Male':
                        if x['Debit Card Holder'] == 1:
                            return "NO"
                        else:
                            if x.Expenses > 35000:
                                return "YES"
                            else:
                                return "NO"
                    else:
                        if x.Expenses > 35000:
                            return "YES"
                        else:
                            return "NO"
                else:
                    if x.Income > 45000:
                        return "YES"
                    else:
                        if x.Age>60:
                            return "NO"
                        else:
                            return "YES"              
        elif x['Online Shopping Experience'] == 4 or x['Online Shopping Experience'] == 5:
            return "YES"
```
Forcefully apply data, get the corresponding 'right' result ruled by myslef.

## Comparison between decision tree and my tree

### Decision tree result:
![](https://i.imgur.com/oddpfR3.png)

### My tree again:
![](https://i.imgur.com/c2Gsvxx.png)

### Accuracy:
0.98

### How different
- Debit card user and Sex is not relevent to my right data.
- Level conditions place in different ways
- Condition break points are slightly shifted
- Income conditions are different

### How Similar
- the result are well pridicted

## My review on the trees
- The decision tree is almost the same as my right data without using sex and debit card attribute
    - I can get it because my sex and debit card conditions appear in the low level node, they might out of reach by the process of result generating.

- The income value is shifted a lot
    - I think it is because the generation of sample data is based on normal distribution, and some condition is just too hard to get from the sample. (like income > 100k)

- Decision tree is so accurate.

### Which tree will I prefer?
After this experiment, I prefer using sklearn's decision tree. It is because my tree and decision tree has different approaches, and I think decision tree make it better.

- For me, the right data is formed by the life experience, and what I think the data is right.
- For decision tree. It does not decide without any evaluation. It decides based on the gini, which makes the result more reliable.
