# Appendix G - RAGAS Evaluation Equations

The equations are taken from RAGAS documentation.

#### Faithfulness
This measures the factual consistency of the generated answers in a given context. It is calculated from the answer and retrieved context. The answers were scaled to the (0,1) range. Higher the better.
The generated answer is regarded as faithful if all claims made in the answer can be inferred from the given context. To calculate this, a set of claims from the generated answers was first identified. Each of these claims is then cross-checked with the given context to determine whether it can be inferred from the context. The faithfulness score is given by:

Faithfulness Score = $\frac{abs \times CGAGC}{| TCGA |}$

>CGAGC = # of claims in the generated answer that can be inferred from the given context
TCGA  = Total number of claims in generated answers


#### Context Utilization
Context utilization is a reference-free version of context precision metrics. Context utilization is a metric that evaluates whether all of the answer-relevant items present in the context are ranked higher or not. Ideally, all relevant chunks must appear in the top ranks. This metric is computed using question, answer, and context, with values ranging between 0 and 1, where higher scores indicate better precision.

Context Utilization $@K$ = $\frac{\sum_{k=1}^K (P_k \times v_k)}{TOP_{R_k}} $

Precision$@k$ = $\frac{Tp_k}{(Tp_k + Fp_k)}$

Where $K$ is the total number of chunks in contexts and $v_k ∈ {0, 1}$ is the relevance indicator at rank $k$.

>TOP$_{R_k}$ = Total number of relevant items in the top K results
Tp$_k$ = True positives @k
Fp$_k$ = False positives @k

#### Answer Relevance
The evaluation metric, answer relevance, focuses on assessing how pertinent the generated answer is to the given prompt. A lower score was assigned to answers that were incomplete or contained redundant information, and higher scores indicated better relevance. This metric was computed using questions, contexts, and answers.
Answer Relevancy is defined as the mean cosine similarity of the original question to a number of artificial questions that were generated (reverse engineered) based on the answer:

answer relevancy = $\frac{1}{N}$ $\sum_{i=1}^N cos(E_{gi}, E_o)$

answer relevancy = $\frac{1}{N}$ $\frac{E_{gi} - E_o}{||E_{gi}||||E_o||}$

Where:
* $E_{gi}$ is the embedding of the generated question $i$.
* $E_o$ is the embedding of the original question.
N ||Egi ||||Eo||
* $N$ is the number of generated questions, the default of which is 3.

Please note that, although in practice the score will range between 0 and 1 most of the time, this is not mathematically guaranteed, due to the nature of the cosine similarity ranging from -1 to 1.

#### Context Relevance

The ontext $c(q)$ is considered relevant to the extent that it exclusively contains the required information.
to answer the question. In particular, this metric aims to penalize the inclusion of redundant information. To estimate context relevance, given a question $q$ and its context $c(q)$, the LLM extracts a subset of sentences, $S_{ext}$ from $c(q)$, which are crucial to answer $q$, using the following prompt:
> Please extract relevant sentences from the provided context that can potentially help answer the following question. If no relevant sentences are found, or if you believe the question cannot be answered from the given context, return the phrase “Insufficient Information”. While extracting candidate sentences you’re not allowed to make any changes to sentences from given context.

The context relevance score is then computed as:
Context relevance = $\frac{N_{exs}}{c(q)_{ts}}$

> Number of extracted sentences = $N_{exs}$
Total number of sentences in $c(q) = c(q)_{ts}$


<span style="color:violet">*Content for this sub section comes from Es et al., the author of RAGAS: Automated Evaluation of Retrieval augmented generation.</span>

