# QPP4Children

Description to be added

# Abstract

Query performance prediction (QPP) methods have primarily been tailored to mainstream users, thus relying on the traditional concept of relevance. In the case of children, however, relevance goes beyond content-based resource-query matching, which is why we gauge the performance of existing QPP methods in estimating the fit of resources retrieved in response to child-formulated queries. Outcomes from our empirical exploration of various QPP methods using a traditional and a child-focused definition of relevance on 2 datasets reveal the limitations in the adaptability of existing methods to the context of child information retrieval.

# QPP implementation details

## Post retrieval strategies
Following \cite{shtok2012predicting}, we use the Clarity variant based on sum-normalised retrieval performance score from BM25 for computing document weights for the relevance model. We consider the top 100 documents for constructing a relevance model and clip the model at the top-100 terms cutoff \cite{shtok2010using}. For WIG, we set $k$ to 5 as per \cite{zhou2007query}. For SMV and NQC, $k$ is set to 100 \cite{shtok2012predicting,tao2014query}, and the corpus score is the average retrieval performance of the top-1000 documents \cite{tao2014query}. For \sigmax{}, $x$ is set to 50, as per \cite{cummins2011improved}. 

## Training neural-based strategies
As in \cite{meng2023query,meng2025query}, we train each BERT-based method for 10 epochs using a 5-fold cross-validation to pick the best checkpoint for predicting the performance of BM25 based on Pearson's correlation. All BERT-based methods use \texttt{bert-base-uncased} \cite{bert_base_uncased} with a constant learning rate (0.00002), and the Adam optimizer \cite{kingma2014adam}. For QPP-GenRE, we follow the fine-tuning process in \cite{meng2025query} and use \texttt{Llama-3.2-3B-Instruct} \cite{meta_llama_2024} with judging depth $n=1000$ due to its balance between QPP quality and computational efficiency compared to other LLMs \cite{meng2025query}. To enable multi-grade relevance score generation for \ER{}, we modify the original zero-shot prompt \cite{abbasiantaeb2025improving}.
