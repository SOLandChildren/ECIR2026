# Query Performance Prediction using a Child-focused Definition of Relevance

Query performance prediction (QPP) methods have primarily been tailored to mainstream users, thus relying on the traditional concept of relevance. In the case of children, however, relevance goes beyond content-based resource-query matching, which is why we gauge the performance of existing QPP methods in estimating the fit of resources retrieved in response to child-formulated queries. Outcomes from our empirical exploration of various QPP methods using a traditional and a child-focused definition of relevance on 2 datasets reveal the limitations in the adaptability of existing methods to the context of child information retrieval.

## QPP implementation details

### Resources
The following resources must be downloaded to reproduce our results.
1. Datasets:
   a. [kid-friend-en](https://zenodo.org/records/14684724/files/kid-friend-en.zip?download=1) from [9] **Note: The dataset exists in multiple languages, for English, download kid-friend-en.zip**
   b. requik from [10,11] can be requested from the original authors  
   c. [CLEAR corpus](https://www.commonlit.org/blog/introducing-the-clear-corpus-an-open-dataset-to-advance-research-28ff8cfea84a/)  
   d. [SQuAD dataset](https://huggingface.co/datasets/rajpurkar/squad)  

2. Codebases:
   a. [spache-allen](https://github.com/BSU-CAST/ecir22-readability)
   b. [QPP-GenRE](https://github.com/ChuanMeng/QPP-GenRE)
   c. [Other QPP methods](https://github.com/ChuanMeng/QPP4CS)
   d. [QPP Evaluation](https://github.com/RicardoMarcal/qpp-risk-evaluator)

### Post retrieval strategies
Following Shtok et al. [1], we use the Clarity variant based on sum-normalised retrieval performance score from BM25 for computing document weights for the relevance model. We consider the top 100 documents for constructing a relevance model and clip the model at the top-100 terms cutoff [2]. For WIG, we set $k$ to 5 as per [3]. For SMV and NQC, $k$ is set to 100 [1,4], and the corpus score is the average retrieval performance of the top-1000 documents [4]. For $\sigma$<sub>max</sub>, $x$ is set to 50, as per [5]. 

### Training neural-based strategies
As in [6,7], we train each BERT-based method for 10 epochs using a 5-fold cross-validation to pick the best checkpoint for predicting the performance of BM25 based on Pearson's correlation. All BERT-based methods use [bert-base-uncased](https://huggingface.co/google-bert/bert-base-uncased) with a constant learning rate (0.00002), and the Adam optimizer. For QPP-GenRE, we follow the fine-tuning process in [7] and use the [Llama-3.2-3B-Instruct](https://huggingface.co/meta-llama/Llama-3.2-3B-Instruct) model with judging depth $n=1000$ due to its balance between QPP quality and computational efficiency compared to other LLMs [7]. To enable multi-grade relevance score generation for $CR$, we modify the original zero-shot prompt [8].

"You are a search quality rater evaluating the relevance of text samples. Given a question and a passage, you must provide a score on an integer scale of 0 to 3 to indicate to what extent the given document meets the information needs of the user. The scores have the following meanings:

0:  not topically relevant 
1:  topically relevant but not readable for a 5th grader or below nor educational
2:  topically relevant and educational or topically relevant and readable for a 5th grader or below
3:  topically relevant, readable for a 5th grader or below, and educational 
       
Question: {question}  
Passage: {passage}  
Output:"

## References:
[1] Anna Shtok, Oren Kurland, David Carmel, Fiana Raiber, and Gad Markovits. 2012. Predicting Query Performance by Query-Drift Estimation. ACM Trans. Inf. Syst. 30, 2, Article 11 (May 2012), 35 pages. https://doi.org/10.1145/2180868.2180873  
[2] Anna Shtok, Oren Kurland, and David Carmel. 2010. Using statistical decision theory and relevance models for query-performance prediction. In Proceedings of the 33rd international ACM SIGIR conference on Research and development in information retrieval (SIGIR '10). Association for Computing Machinery, New York, NY, USA, 259–266. https://doi.org/10.1145/1835449.1835494  
[3] Yun Zhou and W. Bruce Croft. 2007. Query performance prediction in web search environments. In Proceedings of the 30th annual international ACM SIGIR conference on Research and development in information retrieval (SIGIR '07). Association for Computing Machinery, New York, NY, USA, 543–550. https://doi.org/10.1145/1277741.1277835  
[4] Yongquan Tao and Shengli Wu. 2014. Query Performance Prediction By Considering Score Magnitude and Variance Together. In Proceedings of the 23rd ACM International Conference on Conference on Information and Knowledge Management (CIKM '14). Association for Computing Machinery, New York, NY, USA, 1891–1894. https://doi.org/10.1145/2661829.2661906  
[5] Ronan Cummins, Joemon Jose, and Colm O'Riordan. 2011. Improved query performance prediction using standard deviation. In Proceedings of the 34th international ACM SIGIR conference on Research and development in Information Retrieval (SIGIR '11). Association for Computing Machinery, New York, NY, USA, 1089–1090. https://doi.org/10.1145/2009916.2010063  
[6] Chuan Meng, Negar Arabzadeh, Mohammad Aliannejadi, and Maarten de Rijke. 2023. Query Performance Prediction: From Ad-hoc to Conversational Search. In Proceedings of the 46th International ACM SIGIR Conference on Research and Development in Information Retrieval (SIGIR '23). Association for Computing Machinery, New York, NY, USA, 2583–2593. https://doi.org/10.1145/3539618.3591919  
[7] Chuan Meng, Negar Arabzadeh, Arian Askari, Mohammad Aliannejadi, and Maarten de Rijke. 2025. Query Performance Prediction Using Relevance Judgments Generated by Large Language Models. ACM Trans. Inf. Syst. 43, 4, Article 106 (July 2025), 35 pages. https://doi.org/10.1145/3736402  
[8] Zahra Abbasiantaeb, Chuan Meng, Leif Azzopardi, and Mohammad Aliannejadi. 2025. Improving the Reusability of Conversational Search Test Collections. In Advances in Information Retrieval: 47th European Conference on Information Retrieval, ECIR 2025, Lucca, Italy, April 6–10, 2025, Proceedings, Part I. Springer-Verlag, Berlin, Heidelberg, 196–213. https://doi.org/10.1007/978-3-031-88708-6_13
[9] 
