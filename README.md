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


