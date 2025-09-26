# Query Expansion with Synonymous Terms for Improving Retrieval Results

One of the most important steps in building an information retrieval system is applying text analysis algorithms to the data collection. These algorithms determine how our text is processed and generate the terms that will go into our index. When a user submits a query, it undergoes similar processing, and its terms are compared with the terms in the index. Documents that contain the query terms are returned as relevant to the user.

One of the biggest challenges in this process is that users may express their information needs in different ways. For example, "walk in the mountains" can also be expressed as "trekking" or "hiking." If a user submits the query "hiking," but the index contains documents with the word "trekking," those documents will not be returned, even though they are relevant. The user may not have their information need satisfied.

A way to address this problem is for the retrieval system to know the synonyms of the query terms.  

On this project we create a search engine that uses query expansion with synonymous terms so that it can interpret the user’s information need in different ways and address problems like the one described above. Two methods to find synonyms are applied: one using a specialized synonym dictionary and another using the popular neural network algorithm word2vec. Each methodology has its own advantages and limitations. Finally, we evaluate our search engine on the IR2025 collection using the evaluation tool `trec_eval`.

---

## Phase 1 – Classical Retrieval 

In the first phase, we will apply one of the classical retrieval models to the IR2025 text collection. IR2025 includes a set of queries along with their correct relevant answers. Our system will simply return the most relevant documents for each query based on the chosen retrieval model, without using query expansion with synonyms.

Steps:

1. We preprocess the IR2025 text collection to make it suitable for use by the ElasticSearch search engine.  
2. We create an index from the collection using ElasticSearch, choosing an appropriate analyzer and similarity function. Each document should be stored in a field of the ElasticSearch index.  
3. We execute the queries on the index and collect the top-k retrieved documents for `k = 20, 30, 50`.  
4. We evaluate the results by comparing them with the correct answers using `trec_eval` and the evaluation measures MAP (mean average precision) and avgPre@k (average precision at top k documents) for `k = 5, 10, 15, 20`.  
5. We analyze the results.

---

## Phase 2 – Query Expansion with WordNet Synonyms 

In the second phase, we expand the IR2025 queries with synonyms obtained from WordNet. WordNet is an English lexical database that groups words into sets of synonyms called synsets, provides short definitions and example usages, and records relationships between these sets or their members.

Steps:

1. We use the NLTK library in Python to obtain synonyms. This method allows us to select synonyms from WordNet based on semantic relationships with the query terms (e.g., hypernyms, hyponyms, meronyms, or combinations). We can also limit the number of synonyms and specify parts of speech. Query expansion is done without using ElasticSearch.

2. We expand the IR2025 queries using synonyms from WordNet.

3. We repeat steps 3–5 from Phase 1 for the synonym-expanded queries, recording whether the number of correctly retrieved documents increased. 

---

## Phase 3 – Query Expansion with Word2Vec Synonyms 

In the third phase, we expand the IR2025 queries using synonyms obtained from a word2vec model. Word2vec is a feedforward neural network algorithm that learns vector representations of words, allowing us to find words with similar meanings or words appearing in similar contexts. The model estimates the probability of selecting a word (output) given its context (input), and outputs the nearest neighbors of a word based on context similarity, identifying semantically related words.

Steps:

1. We train a word2vec model using the IR2025 collection as input and the `gensim` library in Python. The model can be configured in terms of architecture (Skip-gram or CBOW), context window size, and vector dimensionality. The context window defines the number of words considered before and after a target word. The dimensionality determines model complexity and size.  

2. We expand the IR2025 queries using the synonyms produced by our model.  

3. We repeat steps 3–5 from Phase 1 for the word2vec-expanded queries.  
