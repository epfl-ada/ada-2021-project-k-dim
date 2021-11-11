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
#### Target
First, we select speakers with a sole profession (this is about 88% of `occupation`). This is to ensure that a quote can be related to only one occupation. The selected profession are listed, and the list is manualy boiled down by:  
 a) selecting the most typical and popular professions  
 b) combining related professions into one class (for example, combine a “biochemistry teacher” and a “physics teacher” into a “teacher” class).
The final list is of size A+1, for A classes and an extra additional class “other”. Finally, we convert those classes to a vector using one-hot encoding.

#### Features
We will use a pre-trained word vectors dictionary (for example, from [“GloVe: Global Vectors for Word Representation”](https://nlp.stanford.edu/projects/glove/)) to convert words in a quote to a numeric vector. For one quote, we get one vector - the sum (or average) of the vectors in the quote. If a word is not in the dictionary, then we assign it a zero vector. To simplify the date we can limit the length of the quote vector by taking only the first few values (for example, 100).

Setting the model. We will define a single-layer neural network with an input of dimension 100 and an output of dimension 11. At the output, we will use the softmax function, after which the output values will model the probability of a profession class.

Model training. We will transform the files into one file, consisting of 112 columns: a column of quote ID, 100 columns - a vectorized quote, 11 columns - the target vector. Divide the file into train and test sets and balance the number of classes (remove several rows of the most popular classes so that the percentage of each class will be approximately the same). We will read training and test data by batch during model working. We will use cross-entropy loss as an optimized function, and will evaluate the model using the test set according to the accuracy metric.

Change options in this system: Depending on the result, we can:

    try to predict another target of a speaker: other columns in the files in the provided folder "speaker_attributes.parquet" (for example, “nationality”, “gender”, “ethnic_group” and so on);
    change the number of predicted classes (the degree of grouping);
    change the dimension of the feature vector for words (take a dimension greater than 100);
    use NLP models with the attention mechanism to extract probably the most important words, which strengthen the model;
    increase the complexity of our neural network by adding hidden layers to get more complex dependencies in the features;
    use cross-validation to tune these parameters and get the best combination.




    Research Questions: A list of research questions you would like to address during the project.
    Proposed additional datasets (if any): List the additional dataset(s) you want to use (if any), and some ideas on how you expect to get, manage, process, and enrich it/them. Show us that you’ve read the docs and some examples, and that you have a clear idea on what to expect. Discuss data size and format if relevant. It is your responsibility to check that what you propose is feasible.
    Methods
    Proposed timeline
    Organization within the team: A list of internal milestones up until project Milestone 3.
    Questions for TAs (optional): Add here any questions you have for us related to the proposed project.
    
    
    
    
    That you have a reasonable plan and ideas for methods you’re going to use, giving their essential mathematical details in the notebook.
    That your plan for analysis and communication is reasonable and sound, potentially discussing alternatives to your choices that you considered but dropped.


Notebook containing initial analyses and data handling pipelines. We will grade the correctness, quality of code, and quality of textual descriptions.
