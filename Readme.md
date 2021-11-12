# Quote Me If You Can

>**TO REMOVE Abstract: A 150 word description of the project idea and goals. What’s the motivation behind your project? What story would you like to tell, and why?**

Have you ever heard of [Catch Me If You Can](https://en.wikipedia.org/wiki/Catch_Me_If_You_Can) ? In this movie, the main character impersonates many professions mainly by appropriating their uniforms, specific behaviours and lingos, and everybody fall for it !  

<img title="Do you concur ?" width="400px" src="img/tenor.gif">

[source](https://media1.tenor.com/images/24eba459fc0a6e19c4d2d60ed678e2f9/tenor.gif?itemid=7219821)

For the uniform it is quite obvious, but what can we say about about the lingo ? One will agree that each occupation has its « proper words », but a person of a specific profession will also express himself in a certain manner. Think of a politician, she does not only speaks about economy and education, but also uses some figure of speech, and her discourse is fluid and well conducted (this might be hard to catch in a quote, but it is an example…).  
From this observation, we would like to see if it possible to guess the profession of someone based on his quotation ?

### A closer look at the data set 

As a first step we chose to filter out any quote whose `qids`.length != 1. 
* Why ? On one hand we get rid of the "homonym issue": _James Fisher_ is not unique, which leads to many Qids. On the other hand, we deal with missing values: some speakers are "None".  By doing so we reduce the data size by **INSERT** %, which is acceptable.
* How ? As the data size is far form greater than our RAM, we filtered out for each year "chunk by chunk" and store the result in 5 new csv files. We can work on those file by appliying the same method.
>**add something about rate? sometimes even if proba <0.5, speaker is assigned (exemple:152638 in data extract)**

After that, we perform a first analysis on the distribution of the number of quote. We show here the distribution of # of quotations among year 2020, for chunk 1 out of 6. Each chunk is of size 500,000. The distribution is assumed similar for all chunks: 

> **Add a title to the graph ?**

<img title="2020: first 500000 quotes" width="400px" src="img/2020first500000.PNG">

By looking at this graph, we assume that # of quotation/person is enough for our purpose. Furthermore, year 2020 has "only" 6 chunks as it finishes in may **CORRECT ?**. The other years have about **INSERT** chunks.

>That you can handle the data in its size. OK
>That you understand what’s in the data (formats, distributions, missing values, correlations, etc.). OK

### Additionnal features

We need to relate the `speaker` feature to its profession. This can be achived by using `speaker_attributes.parquet` and `wikidata_labels_descriptions_quotebank.csv`. `speaker_attributes.parquet` has a `occupation` column, which contains one or several wiki-Qids. We will translate those Qids into a profession using `wikidata_labels_descriptions_quotebank.csv`

>That you considered ways to enrich, filter, transform the data according to your needs.OK 

### Pipeline
#### Targets
We select speakers with a sole profession (this is about 88% of `occupation`). This is to ensure that a quote can be related to only one occupation. The selected profession are listed, and the list is manualy boiled down by:  
 a) selecting the most typical and popular professions  
 b) combining related professions into one class (for example, combine a “biochemistry teacher” and a “physics teacher” into a “teacher” class).
The final list is of size C+1, for C classes and an extra additional class “other”. Finally, we convert those classes to a vector using one-hot encoding.

#### Features
The selected quotes are [lemmatized](https://pythonwife.com/lemmatization-in-nlp/) using [spaCy](https://spacy.io/) (**OR OTHER ?** [seeHere] (https://www.analyticsvidhya.com/blog/2020/08/top-4-sentence-embedding-techniques-using-python/)). The resulting lists are then converted to a collection of numeric vector by means of a pre-trained word vectors dictionary (for example, from [“GloVe: Global Vectors for Word Representation”](https://nlp.stanford.edu/projects/glove/)). If a word is not in the dictionary, a zero vector is assign. Finally, by summing the vectors together, we get one vector per quote. As this vector is high dimensional, **a source here ? How many dimension ?** we can limit the length of the quote vector by taking only the first D values. This _could_ avoid overfitting or provoke underfitting. An optimal D could be selected through a cross validation procedure on a reduced data set. 

#### Model
With the help of a neural network, we can link our D dimensionnal input vector to our C+1 dimensionnal output vector. Softmax is used as final activation, after which the output values will model the probability that the quote's speaker belongs to the predicted class.

#### Train
Our "pre-processed" data set has a `Qid` colum, D columns for the vectorized quote, C+1 columns for the corresponding one-hot label, and is of length N. Split it randomly into a train and test sets (depending on its size we could think of a validation set), and balance the number of classes (remove several rows of the most popular classes so that the percentage of each class will be approximately the same **Is it necessary ? We could use several strategy, look in paper for Quobert**). The train data is loaded by batch and cross-entropy loss is chosen (classification) as an optimized function, and will evaluate the model using the test set according to the accuracy metric.

### Result
What a nice model we got ! We got it because we chosen Qid with sole profession, remeber ? At this stage we could see if our model works, or not. But let's assume it works.  
* Let's consider now Qid with multiple profession, we won't dive into a multiclass neural network but, by looking a the quotes of the person per year or month (depending on available data), we could predict what profession the persone was excercing _by this time_.
* Multiclass Neural Network: can we reverse the process ? Can we choose a class as input and, predict a collection of word related to this class ? (unsupervised)
* Try to predict another target of a speaker: other columns in the files in the provided folder "speaker_attributes.parquet" (for example, “nationality”, “gender”, “ethnic_group” and so on);

### MISSING
**STORY**
Research Questions: A list of research questions you would like to address during the project. **ALMOST**
Proposed timeline **TODEFINE**
Organization within the team: A list of internal milestones up until project Milestone 3. **SO ?**
Questions for TAs (optional): Add here any questions you have for us related to the proposed project. **ANY ?**
    
That you have a reasonable plan and ideas for methods you’re going to use, giving their essential mathematical details in the notebook. OK?...
That your plan for analysis and communication is reasonable and sound, **potentially discussing alternatives to your choices that you considered but dropped.**
    
Notebook containing initial analyses and data handling pipelines. We will grade the correctness, quality of code, and quality of textual descriptions. **YES ?**
