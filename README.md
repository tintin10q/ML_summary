---
title: Cognitive Neuroscience Report
#subtitle: Group 13
date: June 2021
author:
- Quinten Cabo  
numbersections: true
geometry: margin=2cm
lang: "en"
toc-own-page: true
toc: true
---
# 3 types of models
All the ML techniques can be put into another 3 categories.
- Instance based classifiers
    - Uses observations directly without models
    - The only one we have that does this is KNN
- Generative
    - Build a generative statistical model by assuming data comes from a normal distrobution
    - Bayes classifiers
- Discriminative
    - Directly estimate decision rule/boundary
    - Decision trees

# Reminder about probability theory 
This is how bayies 
## Variables 
The basis of baiyes probability theory lies with **random variables** that represent events in the future that could happen. These refer to an element whose status is unknown. For example ```A = "it will rain tomorrow"``` 

The **domain** is the set of values a random variable can take, also to be seen as the set all the possible answers to a question, or all the possible events that could happen. This can be the following types:
**Binary**: Will it rain? (yes, no)
**Discrete**: How much will it rain? Integers (With numbers), 
**Continues**: How much more did it rain today? Floating points (With change and )

Then we define the P() function which will give you the chance of a certain event happening. 

P() has these rules:
> 1. 0 <= P(A) <= 1  
> The output is always between 0 and 1.

> 2. P(True) = 1, P(false) = 0  
> true = 1, false = 0

> 3. P(A ∪ B) = P(A) + P(B) - P(A ∩ B) 
> Set Theory. The chance for A AND B is P(A) + P(B) - P(A) ∩ P(B) 
> So A AND B is P(A) + P(B) - (What A and B have in common)  

![Set Theory explanation](./setex.png)

How likely one certain outcome is, is called the **prior**. So the the outcome of a P() is a prior. For example:
- P(Rain tomorrow) = 0.8
- P(No rain tomorrow) = 0.2 
The probability of at least something happening is 1. So the sum of P() for all possible outcomes is one.

So a prior is the degree that you believe a certain outcome before it happens and one is chosen. If you have no other information. 

## Example

|Red| Blue|
|---|:---:|
| 1 |  0  |
| 0 |  1  |    
| 1 |  0  |
| 0 |  0  |
| 1 |  0  |
| 0 |  1  |
| 1 |  1  |

Here P(Red) = 0.5 or 4/8 and P(Blue) = 0.5

## Joint probability
This is where you want to know the prior for A and B or P(Red) and P(Blue) or P(A, B) or P(Red, Blue). It is the chance that multiple events happen together. 
In this case it is = 1/8

Now it is very important if the variables are independent or not. If something is independent then the joint probability is just P(A) * P(B). If it is not independent then you need to talk about conditional probability. 

## Conditional probability
In some cases given knowledge of one or more random variables we can improve our prior belief of another random variable. As soon as you know that variables are not independent you need to talk about conditional probability.  

This is where `|` comes in. (A = 1|B = 1) this asks the question of:
> What is the chance that if B happens A also happens the outcomes where A is 
true if B is true. You could say this as the chance of A given B. 

The joint distribution can be specified in terms of conditional probability.

> You can then combine these two to get the joint probability.
> It looks like this: `P(x,y) = P(x|y)*P(x)`

## Bayes Probability Theorom

It turns out that the joint probability of A and B is the prior of A times the conditional probability of A and B. So `P(a,b) = P(a|b)*P(a)`

Now that is really great because we now we can make the bayes rule:
From joint probability we can say `P(x,y) = P(x|y)p(y) = P(y|x)P(x)` and this gives us conditional probability. 

> It looks like so:
> `p(y|x) = p(x|y)p(y)/p(x)`

Now if we put this back into ML terms. x = features, y = label.
> P(y|x) --> What is the chance for this label y given features x

## Example
![Bayes event table](bay_example.png) ![Bayes event table converted](bayes_filled_in_example.png)

We want to know the prior of passing your exam if your teacher is Larry. We need the following. Let's also replace the P(x) and alike with semantics for this.
> P(x) = P(Larry) = 28/80 = 0.35
> P(y) = P(Yes) = 56/80 = 0.7 
> P(x|y) = P(Larry|Yes) = 19/56 = 0.34 (Prior of your teacher being larry if you passing.)

Ok so with that we can calculate P(y|x) like P(y|x) = `P(y|x) = P(|y)p(x)/p(y)` or `P(Yes|Larry) = P(Larry|Yes)*P(Larry)/P(Yes)`
So if we write it out: `P(Yes|Larry) = 0.34*0.7/0.35 = 0.68` Meaning the final probabilty of passing the exam if your theater is larry is 0.68

# Naive Bayes classifier
With a the rules from above you can make a machine learning classifier. It is called Naive Bayes classifier. This is a **generative** based classifier. Meaning it builds a generative statistical model based on the data. This classifier is based on the predictions of that model. We do this by just giving the things below we saw before new names.

> P(y|x): The posterior probability of a class (label) given the data (attribute) -> The probability of the label given the data
> P(y): The prior probability of a label -> How likely is a certain class?
> P(x|y): The likelihood of the data given the class -> Prior Probability of the data given a class. Opposite of (y|x)
> P(x): The prior probability of the data

We are after P(y|x) the label given the data. We can calculate this with P(y|**x**) = P(**x**|y)*P(y)/P(**x**).

In practice, you will always have multiple features so it looks like P(y|**X1** ... **Xn**)

This classifier is called a **Naive** Bayes because it assumes that all the features are independent. This makes it so you P(**X1**|y, **X2**, **X3**...**Xn**) = P(**X1**|y)

This gives us: 
P(y|X1...Xn) = ![Final formula naive Bayes](final_bayes.png) 

His means multiply P(y) with all the priors of your classes and divide by the joint prior of all the data. Then you take the class with the highest **posterior probability**.

> **posterior probability** is the prior * likelyhood or the P(y) * P(**x**|y) of the equation. So we want the highest P(y|x)

## Non discrete data
So far we assumed that the data is always discrete but usually with machine learning this is not the case. The data is often continues. For these types of data we often use a **Gausian model** that assumes your data is normally distributed! In this model we assume thatt the input X is taken from a normal distribution X = N(mean, sigma). 

### Different scikit learn bayes models
There are different implementations of native bayes. The one you want to use depends on the type of your data.
- Gaussian Naive Bayes classifier (FaussianNB())
  - Assumes that features follow a normal distribution 
- Multinomial Naive Bayes 
  - Assumes count data
  - Each feature represents how often something happens. Like how often does a word appear in a sentence.
- Bernoulli Naive Bayes
  - Assumes your feature vectors are binary (Can only take 2 values)
  - Can also be continues values which are precisely split. Like Below 10 is 0 and above 10 is 1.
## Advantages and Disadvantages of Naive Bayes. 
- Advantages:
    - Simple
    - Works well with a small amount of training data
    - The class with the highest probability is considered as the most likely class
    - You get a probability of all the classes
- Disadvantages:
    - You have to estimate parameters of the normal distribution
    - You data needs to be normally distributed
      
      You can use different ones for different types of data.
  
# Decision Trees 
With this technique the idea is build a large logic tree that will at the end give you the label of an input. Kind of like having a lot of if/elif/else statements. You get the label by traversing the tree one node at the time by answering boolean questions about a feature in you data. For example: Is the feature larger then 32? If yes go left if no go right.
![Left or right](simple%20tree.png)
This is like playing guess who. 
![Guess who](guesswho.png)
If you go left you might get to a different question as when you would have gone right. Going left or right is called **branching**. Everytime you branch the tree the **depth** of the tree grows. You want the least depth while separating the data as much as possible. At the end of the tree there is no question anymore and just your label. This node is also called a **leaf**. 

![More complicated tree](complicated_tree.png)

> All decision boundaries are perpendicular to the feature axes, because at each node a decision is made about one feature only. So if you see strait lines it is probably a decision tree.

The goal behind decision trees is to get the best branching. You get the best branching based on the order you check the features. This is expressed in the **attribute value** WHAT IS THIS

You want to split as much "area" as possible like this:
![Each test/node layer in the tree splits your data further. Left is dept 1, Right is depth 2](tree_boundary.png) 

> Every depth increase the amount of decision boundary lines increases with depth as well if that makes sense. This is because every depth down creates exponentially more paths. You are making your decision boundary and eliminating labels as you go along 
> ![Dept 1](tree_depth1.png) ![Dept 2](tree_depth2.png) ![Dept 9](tree_depth9.png) 

#### Example 

In this case what is better X1 or X2?

![X1 or X2](x1-or-x2tree.png)

If you split by asking is X1 true of false and it is true then you immediately get the good y. This is the most error reduction with the least depth. As in that case there are still 0 remaining wrong classified outcomes anymore. Thus splitting by X1 first is better. This will then also be indicated by those three measures.

## Choosing how to split your tree
If you have a tree they using it is easy to use but getting the splits is the tricky part. The only way really is just trying a bunch of values/splits and look at some measures to give indication about the improvement it brought. For this we use the decision tree algorithm. This algorithm has to decide:
- What features to ask about
- What values to use in the question (Numbers for continues values, categories for categorical values)
- What order to do the splitting in
- Decide what the outputs are if you get to the leafs

The algorithm works like this:
1. Start from an empty decision tree
2. Split on the next best **attribute**
3. Repeat

Ok but what is the next best attribute? For classification, you can determine it based on these three things:
- Entropy
- Information gain
- Gini index

For regression, you just minimize mean square error. 

### Entropy
Entropy is **the level of uncertainty**. The higher the entropy the more uncertainty. 
The formula is:

![Entropy tree](entropytree.png)
You saw entropy in logistic regression. Entropy is the level of uncertainty. It is the probability of your class occurring giving the log of that probability. That means the chance that any instance is that class. You get that by doing: occurrences feature/n. Entropy can never be negative. Instead of P for Bayes we have H for entropy. 

We want to minimize entropy because we want to minimize uncertainty. 

>Probabilities are always between 0 and 1 so the log of the p will be negative that is why this works. As the lower the p the lower the log results.   
![Log of probability](logofp.png)

#### Examples of entropy

![Examples of entropy](exampleofEntropy3.png)

In this case the entropy is the lowest on the right.
You can also calculate entropy for sequences.

![Sequence](informationtheory.png)

Now here are some more things about entropy she discussed. 

What this shows is that if a sequence is more equal then as in there is an equal division of p between classes then the entropy is higher. 

<u>The idea of using entropy is to try a split and then to calculate what the entropy is after the split. Take the option that has the **lowest entropy** after the split. This option has the least uncertainty.</u>

#### Joint entropy
To find the entropy of a joint distribution you can use this:

![Joint distribution](joint_entropy.png)

She didn't say anything more about it either. So just use this formula.

#### Conditional entropy
Again we use the joint thing + chain rule to get the conditional rule. Conditional entropy  H(X, Y) = H(X|Y)+H(Y) = H(Y|X) + H(X)
If you try to calculate the entropy of a conditional distribution you do it like this. 

![Joint distribution](conditional_entropy.png)

This is asking what is the entropy of X given Y. So instead of likeliness we are after entropy. You can just calculate it with the formula.

#### Dependent vs independent
If X and Y are independent, then X doesn't tell us anything about Y. So H(Y|X) = H(Y). But of course Y tells us everything about Y. H(y|y) = 0.
By knowing X we can decrease uncertainty about Y: H(Y|X) <= H(Y)

### Information gain
With information gain we look for how much information we gain about the features. 
The formula is IG(Y|X) = H(Y) - H(Y|X)

IG(Y|X) is pronounced as information gain in Y due to X. If X is 

If X is completely uninformative about Y then IG(Y|X) = 0
If X is completely uninformative about Y then IG(Y|X) = H(Y) E.G. everything that is uncertain is gone
This is what you use in making the tree. **You want to find the split with the highest information gain.**
> You can see information gain as taking away entropy

### Gini Coefficient
![Gini index](giniIndex.png)

The gini function is called HGini. She did not really explain the idea behind gini. But the idea is the same. Split, fill in the formula, use the split with the lowest gini.

 #### Example:

![Gini example](giniexample.png)

### Thresholds with continues values 
For all of these methods if you have continues values you not only have to try splits but also different treshholds. Like if X between 10 and 12 or 10 and 13 what gives the best model improvement?

## Complexity of the model 
The tree can get really big really quickly. The complexity of a decision tree model is determined by the depth of the tree. **Increasing the depth** of the tree increases the number of decision boundaries and **may lead to overfitting**. For example all these places might have overfitting:

![Overfitting](tree_depth9overfitting.png)

### Ways of reducing model complexity.  (hyperparameters)
 There are hyper parameters that limit model complicity. You should set alteast one of these as theoretically you can keep growing the tree forever.
- max_depth = The max depth the tree can grow
- max_leaf_nodes = The maximum leaf nodes that can exist
- min_samples_split = A minimum amount of samples that have to be in a split to make a split.   
There are also more parameters


## Bayesian Regression
This one is pretty nice. Take linear regression and just put it in the bayian model. 

With bayesian linear regression you formulate linear regression using probability distributions rather than point estimates. This assumes that y was drawn from a probaility distribution. The sklearn linear regresion can actually use baysion regfor regression aswell 


>**Linear regresssion reminder:**
> 
> y(x) = **W**^T**x**
>
> Cost (sum of squares): y(W) = 1/2N*Sum(y(x<sub>i</sub>)-y<sub>i</sub>)
> 
> This is the frequentist view 

y(x)
![bb](bayseianlinearregresion.png)
So this is j ust linear regresion in baysion pretty cool if you ask me its like a whole another way of looking at things.

You base the predictions on probability insteade of a single point. 
And you can optimese these with gradient decent even still. and there are multiple best values. You are more aftere the posterior distribution for the model parameters. 

### Decision Tree Regression
You can do this with the weighted mean square error. That  Whatt split gives you the smallest erro that is the split you take . 

![NICE](2021-05-27%2023.28.30.png)

This is the methods 
