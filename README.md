# FakeNewsDetector


## Introduction

Here, we explore a method of fake news detection using natural language processing techniques. Analyzing body text of over 13,000 online news articles, we apply n-gram bag-of-words techniques and word embeddings. The resulting features were used with a number of classification algorithms including Naïve-Bayes, SVM, and LSTM networks. We follow up with an analysis of the effects that varying degrees of imbalanced datasets have on the model to imitate the reality of fake news detection. Accuracies upwards of 90% were observed in the most successful model (LSTM), but training on less balanced sources resulted in a degradation of F1 scores.

## Methods
A number of NLP approaches involve analyzing the frequency of certain words or phrases in a body of text. The simplest is the bag-of-words model, where text is represented as a set of word-frequency pairs. By using word frequency and occurrence, features can be identified for use in document classification. The n-gram is a more complex variant of the model, where contiguous sequences of words of length n are collected from a text corpus. Unigram and bigram methods were used, but better results were observed with the former.

Both bag-of-words and its n-gram derivative disregard word order or relations. Contrarily, word embeddings map words into N-dimensional vectors, with semantically similar words having comparable values. In essence, embeddings attempt to capture the relationship between words. An example of one such model is GloVe, an unsupervised model that uses co-occurrence relations between words to derive meaning.

Because fake news is a relatively recent phenomenon, research into its detection is in its infancy. However, initial attempts at linguistic analysis of news reliability yield promising results, with greater uses of repetitive content, stopwords, and decreased punctuation seen in misleading news. The stopwords used during feature extraction was the default list of English stopwords in addition to words identifying the source URL, such as “CNN, guardian, …”.

## Datasets
A complete dataset was compiled by combining two datasets: one containing solely fake news, and another containing legitimate news.
A.	BS Detector 
	BS Detector is a browser extension which checks for reliable and unreliable sources. The extension is powered by OpenSources, a manually and professionally curated list of unreliable sources. The labels assigned to this dataset are derived from these sources. As the dataset contains all articles labelled unreliable, this serves as the ground for the fake news we wish to identify.
B.	Archive.org 
	This dataset is hosted on Kaggle, where it was compiled by scraping news articles from Archive.org. We limited the articles to those in the timeframe of 2016 to July 2017 so both sets encompass the same time. Here, legitimate sources were obtained by cross-referencing “Media Bias/Fact Check”, a website that assesses factual accuracy and political bias in news media. Articles by credible news sources were labelled as legitimate.
After combining the datasets, we obtain roughly 13000 online articles, a third of which are fake news. The four fields for each online article are: the source URL, the headline, body text, and fake news label (as defined in Equation (1)).

## Modelling
We're aiming to produce a method of fake news detection strictly from content contained in the body of the new article. This is a binary classification problem where we aim to classify credible news from fake news.We experimented with three different approaches: Multinomial Naive Bayes, SVM, and LSTM Networks.

1)	Multinomial Naïve Bayes
Naïve-Bayes is widely considered to be a good baseline text classifier due to its simplicity and quick implementation. By representing the body text with a bag-of-words, the Naïve-Bayes classifier can estimate the probability of the article belonging to a class—in this case fake or real news— given the document’s representation by bag-of-words. Here we use the Multinomial Naïve-Bayes implementation, but we will simply refer to the model as Naïve-Bayes.
2)	SVM
SVM provides better general performance in text categorization tasks than Multinomial Naïve-Bayes. Text categorization problems are often linearly separable, so we use a linear SVM as another base model for experimentation. Here, we use an implementation for a linear SVM optimized using stochastic gradient descent.
3)	LSTM Networks
The long short-term memory is an artificial recurrent neural network (RNN) that comprises of both feedforward and feedback connections. It is often used in NLP processing and has the advantage of processing sequential data.
We used the LSTM network with an Embedding layer to perform the necessary binary classification. In order to derive Embedding weights, we stripped the vectors encoded by the GloVe file and created a matrix of weights. We selected a batch size of 32 to satisfy a balance between its ability to generalize and runtime. To avoid overfitting, we added a callback for early stopping that monitors validation accuracy with a preset patience of 5 epochs. After analyzing the trends of validation accuracy and loss, we settled on 30 epochs following a plateau.

## Results


