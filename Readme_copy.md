# Title: Quote Me If You Can

### Abstract

Have you ever heard of [Catch Me If You Can](https://en.wikipedia.org/wiki/Catch_Me_If_You_Can) ? In this movie, the main character impersonates many professions mainly by appropriating their uniforms, specific behaviours and lingos, and everybody fall for it !  

<img title="Do you concur ?" width="400px" src="img/tenor.gif">

[source](https://media1.tenor.com/images/24eba459fc0a6e19c4d2d60ed678e2f9/tenor.gif?itemid=7219821)

For the uniform it is quite obvious, but what can we say about the lingo ? One will agree that each occupation has its « proper words », but a person of a specific profession will also express himself in a certain manner. Think of a politician, she does not only speaks about economy and education, but also uses some figure of speech, and her discourse is fluid and well conducted (this might be hard to catch in a quote, but it is an example).  
From this observation, we would like to see if it is possible to guess the profession of someone based on his quotations ?

### Research Questions(To change!)

A list of research questions we would like to address during the project:
* Let's consider now Qids with multiple professions, we won't dive into a multiclass neural network but, by looking at the quotes of the person/year, is it possible to predict what profession the person was excercing _by this time_?
* Can we use our model to assign a word, or collection of words to a class ? 
* Can we predict another target of a speaker: other features in the provided folder "speaker_attributes.parquet" (e.g. “nationality”, “gender”, etc)?


### Dataset 

* The full dataset is made of 178 million quotations together with a list of possible speaker ranked by probability, the name of the most probable speaker and its Wikidata Qid, when it has been published, and where it has been published. The later is important because this information has been exctracted out of 162 million English news articles published between 2008 and 2020 included, so one might want to keep records of it.
* We need to relate the `speaker` feature to its profession. This can be achived by using `speaker_attributes.parquet` and `wikidata_labels_descriptions_quotebank.csv`. `speaker_attributes.parquet` has a `occupation` column, which contains one or several wiki-Qids. We will translate those into a profession using `wikidata_labels_descriptions_quotebank.csv`

### Methods(To change what is below!)
'structure below text in methods and update them'

### Pipeline
#### Targets
We select speakers with a sole profession (about 88% of `occupation`). This is to ensure that a quote can be related to only one occupation. The selected profession are listed, and the list is manualy boiled down by:  
 a) selecting the most typical and popular professions  
 b) combining related professions into one class (e.g. combine a “biochemistry teacher” and a “physics teacher” into a “teacher” class).
The final list is of size C+1, for C classes and an extra additional class “other” which contains the remaining professions. Finally, we convert those classes to a vector using one-hot encoding.

#### Features
The selected quotes are [lemmatized](https://pythonwife.com/lemmatization-in-nlp/) using [spaCy](https://spacy.io/) (see [alternatives](https://www.analyticsvidhya.com/blog/2020/08/top-4-sentence-embedding-techniques-using-python/)). The resulting lists are then converted to a collection of numeric vector by means of a pre-trained word vectors dictionary (e.g. from [“GloVe: Global Vectors for Word Representation”](https://nlp.stanford.edu/projects/glove/)). If a word is not in the dictionary, a vector of zeros is assigned. Finally, by summing the vectors together, we get one vector per quote. As this vector is high dimensional (dim=300), we can limit the length of the quote vector by taking only the first D values. This _could_ avoid overfitting or provoke underfitting. An optimal D could be selected through a cross validation procedure on a reduced dataset. 

#### Model
With the help of a neural network, we can link our D dimensionnal input vector to our C+1 dimensionnal output vector. Softmax is used as final activation function, after which the output values will model the probability that the quote's speaker belongs to the predicted class.

#### Train
Our "pre-processed" dataset has a `Qid` colum, D columns for the vectorized quote, C+1 columns for the corresponding one-hot label, and is of length N. In order to define both train and test sets, we follow the 80%-20% rule. The train data is loaded by batch and cross-entropy loss is chosen (classification) as an optimized function, and will evaluate the model using the test set according to the accuracy metric.
Eventually, we will take into consideration that we might encounter an unbalanced data issue, since we will have C+1 classes among the data.


### Proposed timeline
* week 1: Preparing the data to be clustered. Clustering the occupations. Model designing.
* week 2: Merging clustered data with original quotes. Implementing the model.
* week 3: Model tuning. Writing the datastory. Updating Github repository.

### Organization within the team

* Mohamed: Clustering the occupations manually and automatically.
* Ivan: Preparing and merging the datasets, plotting graphs about occupation distributions.
* Konstantin: Implementing the model, plotting the results.
* David: Writing the datastory.

### Notebooks presented: 
1) [Milestone2_Processing](https://github.com/epfl-ada/ada-2021-project-k-dim/blob/main/code/Milestone_2/Milestone2_Processing.ipynb), which contains the work on the quotes datasets from milestone 2.
2) [Milestone2_wikidata](https://github.com/epfl-ada/ada-2021-project-k-dim/blob/main/code/Milestone_2/Milestone2_wikidata.ipynb), which contains the work on the additional metadata on the speakers in the Quotebank dataset from milestone 2.
3) [Data_Processing](https://github.com/epfl-ada/ada-2021-project-k-dim/blob/main/code/Milestone_3/Data_Processing.ipynb) which contains additional datasets preparation, merging the occupations to the original data and graph plotting.
4)[Clusterization]()
5)[Model]()
