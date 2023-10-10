# JyltHfGSAETLOOVj


# Potential talents


## Problem statement:

As a talent sourcing and management company, we are interested in finding talented individuals for sourcing these candidates to technology companies. Finding talented candidates is not easy, for several reasons. The first reason is one needs to understand what the role is very well to fill in that spot, this requires understanding the client’s needs and what they are looking for in a potential candidate. The second reason is one needs to understand what makes a candidate shine for the role we are in search for. Third, where to find talented individuals is another challenge.

The nature of our job requires a lot of human labor and is full of manual operations. Towards automating this process we want to build a better approach that could save us time and finally help us spot potential candidates that could fit the roles we are in search for. Moreover, going beyond that for a specific role we want to fill in we are interested in developing a machine learning powered pipeline that could spot talented individuals, and rank them based on their fitness.

We are right now semi-automatically sourcing a few candidates, therefore the sourcing part is not a concern at this time but we expect to first determine best matching candidates based on how fit these candidates are for a given role. We generally make these searches based on some keywords such as “full-stack software engineer”, “engineering manager” or “aspiring human resources” based on the role we are trying to fill in. These keywords might change, and you can expect that specific keywords will be provided to you.

Assuming that we were able to list and rank fitting candidates, we then employ a review procedure, as each candidate needs to be reviewed and then determined how good a fit they are through manual inspection. This procedure is done manually and at the end of this manual review, we might choose not the first fitting candidate in the list but maybe the 7th candidate in the list. If that happens, we are interested in being able to re-rank the previous list based on this information. This supervisory signal is going to be supplied by starring the 7th candidate in the list. Starring one candidate actually sets this candidate as an ideal candidate for the given role. Then, we expect the list to be re-ranked each time a candidate is starred.

## Data Description:

The data comes from our sourcing efforts. We removed any field that could directly reveal personal details and gave a unique identifier for each candidate.

## Attributes:
* id : unique identifier for candidate (numeric)

* job_title : job title for candidate (text)

* location : geographical location for candidate (text)

* connections: number of connections candidate has, 500+ means over 500 (text)

Output (desired target):
* fit - how fit the candidate is for the role? (numeric, probability between 0-1)

Keywords: “Aspiring human resources” or “seeking human resources”



## Goal(s):

Predict how fit the candidate is based on their available information (variable fit)

## Success Metric(s):

* Rank candidates based on a fitness score.

* Re-rank candidates when a candidate is starred.


# Solution:

After a number of implementations and comparisons (TF-IDF, Word2Vec, Google Word2Vec, Glove, Fasttext and BERT), I was able to find an NLP embedding algorithm which is particularly well suited to our specific use case data , ie. the Word2Vec algorithm trained on the words in this dataset. The candidates in the dataset were ranked based on a fitness score which was based on the cosine similarity metric and normalized connections. The solution also handles manual starring of candidates with reranking based on Learning To Rank algorithm (LambaMart with LightGBM) in which our final predicted_score is better for the candidates that were starred.

Plus, the solution above becomes better with each starring. This is because as and when the starred candidate sample size increases, it helps our LTR algorithm to learn the patterns in the data better. And hence the better results (which can be seen when number of starred candidates was increased to 10).

We can also filter out candidates which should not be in the list by setting a threshold for our fitness score. We can take a safe limit of fitness score threshold as 0.40 as not related or candidates who should not be in the list. 

Human bias in project can be prevented by updating the dataset with keeping a track of list of candidates which actually get selected for specific queries and updating the score to give higher precedence to selected candidates. The ranking algorithm can be reranked and starred candidates can be substituted to selected candidates. This pattern can be learnt and applied to later searches after a generous amount of queries and selected candidates are recorded. Then this procedure will be automated and starring will not be needed, hence no human intervention.

Future enhancements: To implement this on large scale, Fasttext or BERT can be used in the above solution. And the feature set can also be improved by including years of experience, educational qualifications, query related skill set, awards or certificates attained and many more.
