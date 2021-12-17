# Title: Quote Me If You Can

[Here is the Data story](https://quote-me-if-you-can.github.io/)!

## Abstract

Have you ever heard of [Catch Me If You Can](https://en.wikipedia.org/wiki/Catch_Me_If_You_Can) ? In this movie, the main character impersonates many professions mainly by appropriating their uniforms, specific behaviours and lingos, and everybody fall for it !  

<img title="Do you concur ?" width="400px" src="img/tenor.gif">

[source](https://media1.tenor.com/images/24eba459fc0a6e19c4d2d60ed678e2f9/tenor.gif?itemid=7219821)

For the uniform it is quite obvious, but what can we say about the lingo ? One will agree that each occupation has its « proper words », but a person of a specific profession will also express himself in a certain manner. Think of a politician, she does not only speaks about economy and education, but also uses some figure of speech, and her discourse is fluid and well conducted (this might be hard to catch in a quote, but it is an example).  
From this observation, we would like to see if it is possible to guess the profession of someone based on his quotations ?

## Research Questions(To change!)

A list of research questions we would like to address during the project:
* Is it possible to get the profession of the person based on his/her quotes?
* What are the most cited occupation clusters in the journals?
* Can we predict another target of a speaker: other features in the provided folder "speaker_attributes.parquet" (e.g. “nationality”, “gender”, etc)?
* By looking at the quotes of the person/year, is it possible to predict what profession the person was excercing by this time?


## Dataset 

* The full dataset is made of 178 million quotations together with a list of possible speaker ranked by probability, the name of the most probable speaker and its Wikidata Qid, when it has been published, and where it has been published. The later is important because this information has been exctracted out of 162 million English news articles published between 2008 and 2020 included, so one might want to keep records of it.
* We need to relate the `speaker` feature to its profession. This can be achived by using `speaker_attributes.parquet` and `wikidata_labels_descriptions_quotebank.csv`. `speaker_attributes.parquet` has a `occupation` column, which contains one or several wiki-Qids. We will translate those into a profession using `wikidata_labels_descriptions_quotebank.csv`

## Methods(To change what is below!)

#### Pre-processing
First, we chose to filter out any quote whose qids.length != 1. On one hand we get rid of the "homonym issue": James Fisher is not unique, which leads to many Qids. On the other hand, we deal with missing values: some speakers are "None". By doing so, we reduce the data size by 50%. For a full year, we have in average 22 million quotes. It then remains 11 million quotes per year, which should be enough.

Second, from the additional datasets we take only those people who has distinct occupation. This is for proper model training process, so it will be able to distinguish one job from another. Other rows with several occupations may be used for training, because the designed model will output probabilities of a person assigned to each of the cluster.

#### Clustering

We clustered the occupations 4 times:
1)	Into four main clusters: Research, Politics, Sport and Arts. This was done to check the performance of the model for the first time.
2)	Into 20 clusters. They were created from most featured occupations in additional dataset.
3)	20 clusters were reduced to 10 by merging the existed with similar ones. This was to check model behavior on less classes.
4)	Into 10 by clusters using machine learning algorithm with the whole occupations list given.

After each obtained clusterized occupations, we merged this information to the original quotes dataset and processed it to the model.
#### Model

We used pretrained BERT-Base model (BertTokenizer and BertModel with 768 hidden layers, introduced in [this paper](https://arxiv.org/abs/1810.04805)). We add one fully connected layer with 10 output dimensions. BertTokenizer transforms input string into tokens, then BertModel returns 768-dimensional representation of input string. Further, added layer produce 10 values which after applying sigmoid function predict the probabilities of each class.

The weighted binary cross entropy was used as a loss function. The weights were used to decrease the effect of unbalanced data. The weights were computed as normalized reverse frequencies of classes in the train data.

For transformer-based models, it is convenient to use a schedular which changes learning rate during training to make training more smooth: smoothness increases the learning rate from zero to set value during warmup training period and smoothly decrease it to zero for last part of training. We used linear schedular with warmup period equals to first 10% of total training steps.

After several training trials we found that weighted loss does not fully prevent overfitting to the most frequent classes. For training the final model, we decided to create fully balanced train and test datasets.


## Proposed timeline
* week 1: Preparing the data to be clustered. Clustering the occupations. Model designing.
* week 2: Merging clustered data with original quotes. Implementing the model.
* week 3: Model tuning. Writing the datastory. Updating Github repository.

## Organization within the team

* Mohamed: Clustering the occupations manually and automatically.
* Ivan: Preparing and merging the datasets, plotting graphs about occupation distributions.
* Konstantin: Implementing the model, plotting the results.
* David: Writing the datastory.

## Notebooks presented: 
1) [Milestone2_Processing](https://github.com/epfl-ada/ada-2021-project-k-dim/blob/main/code/Milestone_2/Milestone2_Processing.ipynb), which contains the work on the quotes datasets from milestone 2.
2) [Milestone2_wikidata](https://github.com/epfl-ada/ada-2021-project-k-dim/blob/main/code/Milestone_2/Milestone2_wikidata.ipynb), which contains the work on the additional metadata on the speakers in the Quotebank dataset from milestone 2.
3) [Data_Processing](https://github.com/epfl-ada/ada-2021-project-k-dim/blob/main/code/Milestone_3/Data_Processing.ipynb) which contains additional datasets preparation, merging the occupations to the original data and graph plotting from Milestone 3.
4) [Clusterization]() which contains 4 different types of clusterization performed on additional dataset, from Milestone 3.
5) [Model](https://github.com/epfl-ada/ada-2021-project-k-dim/blob/main/code/Milestone_3/Model.ipynb) which contains our model description, creating balanced train and test datasets, and model training, from Milestone 3. 
