# Rebuild-My-Professor

Here is the repository for our course project at Vanderbilt: *CS 3891 Special Topics* (2020 Spring). We applied machine learning techniques in this project and worked on **rating tags prediction** and **rating visualization on radar chart** based on ratemyprofessors.com.

For rating tags prediction, we employ multi-label classification techniques such as **[TODO]**.

For radar chart visualization, we first perform topic modelling to gain insight into the data and choose dimensions of the radar chart. Then we manually label data for each category and apply BERT embedding to train a multi-class classification model. Finally, we calculate the score based on normalization and display the visualization on a simple web server.

## Dataset

We wrote a [web scraper](https://github.com/quao627/RMP) that connects to the site's public API to obtain data for our training. Check out the [Jupyter notebook demo](https://github.com/quao627/RMP/blob/main/demo.ipynb) for a sample output. Also, check out a [simple analysis](https://github.com/quao627/RMP/blob/main/RMP_EDA.ipynb) on the sample data.

We scraped the top 100 universities of [2021 Best U.S. University Rankings](https://www.usnews.com/best-colleges/rankings/national-universities).

- [School folder](https://drive.google.com/drive/folders/1JB2iQgBEiDR3a2NMkzh2_SfgiodStT8-?usp=sharing) contains all professors' information of each university, such as professor's name and overall rating score.
- **[Rating folder](https://drive.google.com/drive/folders/13WYlwAweXeQ3ZcRLOfh7f4kaiBL7sRNB?usp=sharing)** has all the review details for every professor of each university, such as comments, review date, or help count.
- In the above two folders, the file name corresponds to the university's I.D. number on the site, and [this sheet](https://docs.google.com/spreadsheets/d/1OmqnJeSWYc9WafFIwVyKHBm1RW-Mzgijc7RMauLujDQ/edit?usp=sharing) pairs each university's name with its I.D.

## Motivation

### About ratemyprofessors.com

Rate My Professors is a popular site among college students, allowing students to rate their professors and campuses. It was founded in May 1999 by a Menlo Park, CA-based software engineer named John Swapceinski. It is currently the largest professor rating site, boasting over 1.7 million professor reviews in over 7500 campuses across the U.S., U.K., and Canada. [1] It is beneficial as it provides references to students and feedback to professors. For example, first-year college students can view the professors' performances on the campus, which aids them to choose courses.

Professor ratings are closely related to college student choices. Given the site's enormous data, we focus on what insight we may get and how to improve the user experience by applying machine learning techniques. Through discussion of several ideas, we chose the following two to implement in the course:

### Tags Prediction

**Goal**: Automate the process of assigning tags to reviews based on their content. In other words, classify content into one or more classes.

**Motivation**: Many reviews on the Ratemyprofessor site don't have tags because the site introduces tagging feature afterwards. Therefore, the summary statistics on each professor's Ratemyprofessor web page may be misleading due to incomplete tagging.

**Expected Outcome:** The model performance will be evaluated using metrics that we commonly use for classification tasks such as cross-entropy or F1-score.

### Radar Chart Visualization

**Goal**: Summarize the ratings into numeric values on multiple dimensions and visualize the summarization through Radar Chart.

**Motivation**: Currently, on the site, we have tags associated with each professor. However, we wonder if there is a more intuitive way of summarizing the data? We propose translating each professor's ratings into numeric values of multiple dimensions through visualization tools and machine learning. For example, we can have a Radar Chart with six angles, which stands for the different dimensions. One dimension might be the 'amount of homework.' A higher number indicates there's much homework. We assigned scores to each of them and highlight the area enclosed by the shape. We believe that through visualization techniques like this, students obtain information more efficiently.

**Expected Outcome:** Since we don't have a ground truth for the task, we might manually label the data for classification purposes. Besides, to test if our tool helps increase efficiency, we may invite students to use the product and hear their feedback. Moreover, it might be helpful to have a tooltip integrated with the site to display our visualization.

## Project: Tags Prediction

## Project: Radar Chart

### Methods

Here's an illustration regarding how our process works.
![img](./Radar-Chart/workflow-sketch.png)

1. **Data Prep**:
    See above Dataset Section.

2. **Topic Modelling**:
    We want to investigate what dimension we want our radar chart to reflect on. In other words, we want to know what classes we want for the model training process. We choose **clustering algorithms** to gain more insight into the structure of the data. We first apply **Bert embeddings** to the sentences and then apply the **UMAP** algorithm to reduce the dimensionality of the embeddings. Afterwards, we use **HDBSCAN** to perform clustering. We analyze the resulted clusters and summarize the topics.
   - Failed Attempt: We also try popular topic modelling methods such as LDA, but it doesn't yield relevant results due to the significant similarities between the sentences of our task.

3. **Multiclass Classification Model Development**:
   After manually checking the clusters and choosing our topics, we manually label some data within the relevant clusters for training. To build the model, we apply **Bert embedding and a linear regression layer** for output. We also tried **Bert embedding with the LSTM model**.

4. **Score Calculation**:
   After model training, we apply normalization techniques and design a score calculation mechanism for later radar chart visualization.

5. **Web Visualization**:
   We set up a small python web server and display the final results using d3.js.

#### Topic Modelling

**[Collab notebook for topic modelling part](./Radar-Chart/topic-modelling-dim-reduction.ipynb)**

For this part, we are asking what topic we can frequently find in these sentences. We use pre-trained Bert embedding as it has shown exceptional results in various NLP tasks. Besides, Bert is pre-trained using a large corpus of data, and we believe they have a more accurate representation of words and sentences.

After applying the embeddings, each sentence becomes a 768-dimensional vector. However, most clustering algorithms work better on lower-dimensional data. Hence we use UMAP to reduce the dimensionality of the embeddings. Afterwards, we apply HDBSCAN for clustering, which stands for Hierarchical Density-Based Spatial Clustering of Applications with Noise.

After obtaining the clusters, we use the class-based TF-IDF to extract some keywords from each cluster. We also manually check sentences to observe what topics appear frequently. Besides, we reduce the number of topics by merging the most similar topic vectors.

**Topics**
We decide on six categories (5 topics + other):

1. (Others)
2. Participation matters
3. have extra credit
4. engaging lecture
5. helpful office hour
6. heavy workload

#### Multiclass Classification

- **[Collab notebook for using [CLS] token from bert](./Radar-Chart/bert-cls-token.ipynb)**
- **[Collab notebook for bert + lstm](./Radar-Chart/bert-cls-token.ipynb)**

After deciding the topics, we manually label those clusters to prepare data for training. We apply two different model architecture. The first model uses Bert's pre-trained `[CLS]` token with a linear layer, whereas our second model uses Bert's pre-trained embedding on all words and connect it to LSTM for training.

**Metrics & Results**
We evaluate both models using the training loss vs validation loss curve and class accuracy. The overall accuracy for both models is around 86%. Check out both notebook for more detailed information.

#### Score Calculation & Nomalization

[TODO Qu Ao]

### Visualization & Web Server

The code for the web server and visualization are in the folder (TODO link here).
How to run

### Future work

Training data argumentation
lighter weight model

## References

[1] [Rate My Professors.](https://sites.google.com/view/ratemyprofessors-pc
) (2020). Google Sites.


[Google Drive Folder](https://drive.google.com/drive/folders/15wGLUvjiGtFXMZ0XzpDYeSH1XEqWjUBB?usp=sharing)
