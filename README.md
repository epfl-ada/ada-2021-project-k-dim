# ada-2021-project-k-dim

Internal Milestone up until project Milestone 3:

(02/11/2021)
- Present the ideas of each member of the group submitted in Milestone 1 of the project
- Discuss and choose the project idea

(04/11/2021)
- Start working on quotes of the year 2020
- Understand the way to load the sample dataset given in Google colab into dataframe

(TBD)
- Choose the relevant features to load and work on it
- Analyse the data according the choosen features

(TBD)
- Decide a proper pipeline for the project development

(TBD)
- Create the graph for the year 2020


Resume of (04/11/2021)

- Definitely buildiing graph
- using only relevent features (gender, relegion, occupation, ...)
- Threshhold: not considering the person that have less N nb of citation
- Deal with the data (discover the features, the categories in some features, ...)


(for 06/11/2021)
- Think of an implementation of the graph
- Think of a more suitable project goal (using the graph) than prediction (which can be done using NLP)
- See how to connect wikidata search result to the dataset


for next time:

DEALING WITH DATA:
- convert "remaining years" to "filtered_year.csv" and convert into pickle, faster to open

WHAT IS IN THE DATA: (we will speak only about filtered data, not full)
Quotations:
- formats => provide a (df.dtypes) resume
- distribution => provide histogram(x = nbr of quotations, y = nbr of speakers)
- missing values => only some speaker are "None", they are handled by filtering rows with single QID

ExtraFeatures:
- formats => provide a (df.dtypes)
- distribution => give information about occupancy
- 

============================================================================

Classification of the profession of the speaker by speaker’s quote.

Setting the target.
First, we will manually look at the unique professions of speakers. The list of professions can be obtained from the files in provided folder "speaker_attributes.parquet" in the "occupation" column, which contains the wiki-Qids. We will  translate Qid into a profession using the provided file "wikidata_labels_descriptions_quotebank.csv".
Next, we will highlight several of the most typical and popular professions from this list and manually combine related professions into one class (for example, combine a “biochemistry teacher” and a “physics teacher” into a “teacher” class). Let's say we get 10 classes. We will add an additional class “other”. Then we will define the vector of the target class using one-hot encoding - a vector of length 11.

Setting the feature vector.
We will use the pre-trained word vectors dictionary (for example, from “GloVe: Global Vectors for Word Representation” [https://nlp.stanford.edu/projects/glove/]) to convert words in a quote to a numeric vector. For one quote, we get one vector - the sum (or average) of the vectors in the quote. If a word is not in the dictionary, then we assign it a zero vector. To simplify the date we can limit the length of the quote vector by taking only the first few values (for example, 100).

Setting the model.
We will define a single-layer neural network with an input of dimension 100 and an output of dimension 11. At the output, we will use the softmax function, after which the output values will model the probability of a profession class.

Model training.
We will transform the files into one file, consisting of 112 columns: a column of quote ID, 100 columns - a vectorized quote, 11 columns - the target vector. Divide the file into train and test sets and balance the number of classes (remove several rows of the most popular classes so that the percentage of each class will be approximately the same). We will read training and test data by batch during model working. We will use cross-entropy loss as an optimized function, and will evaluate the model using the test set according to the accuracy metric.

Change options in this system:
