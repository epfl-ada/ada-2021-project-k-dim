# Title: Quote Me If You Can

[Here is the Data story](https://quote-me-if-you-can.github.io/)

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


After obtaining clusterized occupations, we merge this information to the original quotes dataset and process it to the model.
#### Model


#### Train



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
4) [Clusterization]()
5) [Model](https://github.com/epfl-ada/ada-2021-project-k-dim/blob/main/code/Milestone_3/Model.ipynb)
