# Quote Me If You Can

>**TO REMOVE Abstract: A 150 word description of the project idea and goals. What’s the motivation behind your project? What story would you like to tell, and why?**

Have you ever heard of [Catch Me If You Can](https://en.wikipedia.org/wiki/Catch_Me_If_You_Can) ? In this movie, the main character impersonates many professions mainly by appropriating their uniforms, specific behaviours and lingos, and everybody fall for it !  

<img title="Do you concur ?" width="400px" src="img/tenor.gif">

[source](https://media1.tenor.com/images/24eba459fc0a6e19c4d2d60ed678e2f9/tenor.gif?itemid=7219821)

For the uniform it is quite obvious, but what can we say about about the lingo ? One will agree that each occupation has its « proper words », but a person of a specific profession will also express himself in a certain manner. Think of a politician, she does not only speaks about economy and education, but also uses some figure of speech, and her discourse is fluid and well conducted (this might be hard to catch in a quote, but it is an example…).  
From this observation, we would like to see if it possible to guess the profession of someone based on his quotation ?

### A closer look at the data set 

As a first step we chose to filter out any quote whose `qids`.length != 1. 
* Why ? On one hand we get rid of the "homonym issue": _James Fisher_ is not unique, which leads to many qID. On the other hand, we deal with missing values: some speakers are "None".  By doing so we reduce the data size by **INSERT** %, which is acceptable.
* How ? As the data size is far form greater than our RAM, we filtered out for each year "chunk by chunk" and store the result in 5 new csv files. We can work on those file by appliying the same method.
>**add something about rate? sometimes even if proba <0.5, speaker is assigned (exemple:152638 in data extract)**

After that, we perform a first analysis on the distribution of the number of quote. We show here the distribution of # of quotations among year 2020, for chunk 1 out of 6. Each chunk is of size 500,000. The distribution is assumed similar for all chunks: 

> **Add a title to the graph ?**

<img title="2020: first 500000 quotes" width="400px" src="img/2020first500000.PNG">

By looking at this graph, we assume that # of quotation/person is enough for our purpose. Furthermore, year 2020 has "only" 6 chunks as it finishes in may **CORRECT ?**. The other years have about **INSERT** chunks.



Here is how we would process:  



    Research Questions: A list of research questions you would like to address during the project.
    Proposed additional datasets (if any): List the additional dataset(s) you want to use (if any), and some ideas on how you expect to get, manage, process, and enrich it/them. Show us that you’ve read the docs and some examples, and that you have a clear idea on what to expect. Discuss data size and format if relevant. It is your responsibility to check that what you propose is feasible.
    Methods
    Proposed timeline
    Organization within the team: A list of internal milestones up until project Milestone 3.
    Questions for TAs (optional): Add here any questions you have for us related to the proposed project.

Notebook containing initial analyses and data handling pipelines. We will grade the correctness, quality of code, and quality of textual descriptions.
