---
layout: post
title: Text Analytics
category: DataScience
tags: [R, The_Analytics_Edge, edx]
---

<!-- more -->

## 1. Tweet analytics
### 1.1 A Bag of Words
- Count the number of times each words appears  
	-> used as baseline in text analytics projects
- preprocessing
	+ cleaning up irregularities  
		- change all words to either lower-case or upper-case
		- tailor approach to punctuation标点
	+ removing unhelpful terms  
		remove words only meaningful in a sentence(**stop words**) like *the, is, at, which*
	+ stemming  
	*argue  argued  argues*  -> *argu*
		- could build a database of words and their stems
		- can write a rule-based algorithm -> **the popular way**  
		eg.if word ends in "ed","ing","ly", remove it

```r
tweets = read.csv("tweets.csv", stringsAsFactors=FALSE)
install.packages("tm")
library(tm)
install.packages("SnowballC")
library(SnowballC)
corpus = Corpus(VectorSource(tweets$Tweet))
corpus[[1]] #show the first tweet in the data
corpus = tm_map(corpus, tolower) # or "content_transformer(tolower)"  change to lower-case
corpus = tm_map(corpus, removePunctuation) # remove punctuation
stopwords("english")[1:10] # see stop words in English
corpus = tm_map(corpus, removeWords, c("apple", stopwords("english")))
corpus = tm_map(corpus, stemDocument) # stemming
# may need to set local by using "Sys.setlocale("LC_ALL", "C")"
```

### 1.2 using A Bag of Words

```r
frequencies = DocumentTermMatrix(corpus)
# see Docs(tweets) from 1000 to 1005, terms(words) from 505 to 515
inspect(frequencies[1000:1005, 505:515])
# show words appears more than 20 times  
findFreqTerms(frequencies, lowfreq=20) 
# 0.98, only keep terms that appear in 2% or more of the tweets
# 0.99, only keep terms that appear in 1% or more of the tweets
# 0.995, only keep terms that appear in 0.5% or more of the tweets
sparse = removeSparseTerms(frequencies, 0.98) 
tweetsSparse = as.data.frame(as.matrix(sparse))
# convert our variable names to make sure they're all appropriate names
colnames(tweetsSparse) = make.names(colnames(tweetsSparse)) 
tweetsSparse$Negative = tweets$Negative

#split train & test set
library(caTools)
set.seed(123)
split = sample.split(tweetsSparse$Negative, SplitRatio=0.7)
train = subset(tweetsSparse, split==TRUE)
test = subset(tweetsSparse, split==FALSE)
```

### 1.3 create cart model
```r
library(rpart)
library(rpart.plot)
# CART model
tweetCART = rpart(Negative ~ ., data=train, method="class")
prp(tweetCART)
predictCART = predict(tweetCART, newdata=test, type="class")
table(test$Negative, predictCART)
# can create random forest model here to do the comparation
```

## 2. Watson
### 2.1 Tools Watson uses
- Lexicon  
	Describe relationship between different words  
	eg."water" is a "clear liguid" but not all "clear liguids" are "water"
- Part of speech tagger & parser  
	Identify functions of words in text  
	eg.some words("Race") can be a verb or a noun

### 2.2 How Watson Works
- Question analysis  
	figure out what the question is looking for.  
	+ Try to find the Lexical Answer Type(LAT) of the question    
	*LAT: word or noun in the question that specifies the answer*  
	+ perform **relation detection** to find relationships among words, and **decomposition** to split the question into different clues
- Hypothesis generation  
	search information sources for possible ansers  
	+ generate candidate answers(a lot)
	+ each candidate answer plugged back into the question in place of the LAT is considered a hypothesis
- Scoring hypothesis  
	compute confidence levels for each answer  
	+ start with "lightweight scoring algorithms" to prune down large set of hypotheses. If this likehood is not very high, throw away the hypothesis  
	+ gather supporting evidence for each candidate answer  
		- retrieve passages that contain the hypothesis text
		- determine the degree of certainty that the evidence supports the candidate answers
- Final ranking  
	look for a highly supported answer
	+ merge similar answers
	+ rank the hypotheses and estimate confidence -> **logistic regression**
		- training data: a set of historical Jeopardy questions
		- independent variable: all of the candidate answers