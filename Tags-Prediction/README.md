# Rebuild-My-Professor

[Google Drive Folder](https://drive.google.com/drive/folders/15wGLUvjiGtFXMZ0XzpDYeSH1XEqWjUBB?usp=sharing)

Here is the repository for our course project at Vanderbilt: CS 3891 Special Topics (2020 Spring). We applied machine learning techniques in this project and specifically rating tags prediction based on ratemyprofessors.com in this folder.

## Why rating tags prediction?

**Goal:**
- Automate the process of assigning tags to reviews based on their content. Eg. Tough grader, Get ready to read, etc

**Motivation**
- Among all the Vanderbilt ratings, for example, only 40% of them have tags. However, every professor’s main page contains a summary statistics on the top, which in this case could be biased. 
- Additionally, the model can help label reviews that were published before they had this feature.

**Approach**
- For rating tags prediction, we employ multi-label classification techniques such as simple logisitic regression, LSTM, and BiLSTM with CNN.



---

The project is composed of two parts: word embedding and multi-label classification. 

## Word Embedding

We tried different approaches for word embedding process, finally came down to two methods: TF-IDF and self-trained Word2Vec.

  1) TF-IDF
 
  TF-IDF is a statistical measure that evaluates how relevant a word is to a document in a collection of documents. This is done by multiplying two metrics: how many times a word appears in a document, and the inverse document frequency of the word across a set of documents. Here we are using this embedding method for our base model.
  
  2) Self-trained Word2Vec

  Here we are using our self trained word2vec model for process the dataset. Basically, Word2Vec represents each distinct word with different vectors. Due to the certain limitation of our dataset, like 'A' means a grade but not a determiner, we decided to train the word2vec model ourselves.

## Multi-label Classification

First of all, we need to understand what is multi-label classification. 
There are three kinds of classification tasks:
1. Binary classification: two exclusive classes
2. Multi-class classification: more than two **exclusive** classes
3. Multi-label classification: **non-exclusive** classes

Rating tags prediction, in our case, would be a sample of multi-label classfication.
To constructing a multi-label classification model we exxperienced with several choices:

- Simple Machine Learning Algorithms: Logistic regression 
- Deep Learning Methods: LSTM, BiLSTM, CNN

## Model Structures

### Base Model
[Colab Notebook](https://github.com/xuezzou/Rebuild-My-Professor/blob/main/Tags-Prediction/BaseModel.ipynb)

Our base model uses TFIDF together with Logisitic regression and reached an average validation accuracy of 0.7899 with an average f1-score of 0.3584.


### Final Model
[Colab Notebook](https://github.com/xuezzou/Rebuild-My-Professor/blob/main/Tags-Prediction/Tag_Classification_with_BiLSTM%2BCNN.ipynb)

We picked self-trained Word2Vec word embedding together with BiLSTM with CNN using binary cross entropy as loss function as our final model. It reached an average validation accuracy of 0.8275, recall of 0.7702, and an average f1-score of 0.7795.
