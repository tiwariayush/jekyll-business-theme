---
title: Our Research
layout: page
description: "Postgres-Bayes: advanced Bayesian inference running directly inside PostgreSQL. Faster than Stan, more capable than MADlib, and fully open source. The research behind our JEI certification."
bodyClass: page-research
---

# Bringing Bayesian Intelligence to PostgreSQL

We're a **Jeune Entreprise Innovante (JEI)** based in France, and this is the research that earned us that status.

The core question is simple: why does probabilistic reasoning have to happen outside your database? You store data in PostgreSQL, then extract it, ship it to TensorFlow or PyMC3 or Stan, run your inference there, and push the results back. Every step adds latency, complexity, and points of failure.

We built [postgres-bayes](https://github.com/cartesiantrees/postgres-bayes) to eliminate that round trip. Advanced Bayesian inference, running directly inside PostgreSQL, where the data already lives.

## The problem with current approaches

Frameworks like TensorFlow, PyMC3, and Stan are powerful tools for probabilistic modeling. But they all share the same architectural assumption: data gets extracted from the database, processed externally, and results get written back.

This creates real problems in production:

- **Latency from data transfer.** Moving data between storage and compute adds hundreds of milliseconds at minimum. For real-time applications (fraud scoring, live recommendations, anomaly detection), that round trip is the bottleneck.
- **Pipeline complexity.** You end up maintaining two systems: the database and the ML framework. Synchronization, versioning, schema drift, deployment orchestration. Every piece is another thing that can break at 2am.
- **Resource overhead.** Running a separate compute layer for inference means duplicate infrastructure, duplicate memory usage, and operational costs that scale with both your data volume and your model complexity.

Commercial solutions exist (Microsoft SQL Server ML Services, Oracle Machine Learning, Google BigQuery ML), but they typically run external Python or R scripts under the hood, support mostly deterministic models, and offer limited or no support for advanced Bayesian methods. They're also proprietary, which limits extensibility.

## The research gap we're filling

The field of in-database machine learning (IDBML) has explored some of this territory. Work by Neumann et al. on in-database ML, Hellerstein et al.'s MADlib framework, and Kaufmann et al.'s SQL code generation all move computation closer to the data. But they focus on simpler models: linear regression, logistic regression, decision trees, basic classification.

Nobody has built a comprehensive framework for **advanced Bayesian inference inside a relational database**. Not hierarchical models, not MCMC methods, not variational inference, not Bayesian optimization. That's the gap postgres-bayes fills.

To put it concretely:

| Capability | MADlib | Teradata ML | BigQuery ML | Postgres-Bayes |
|---|---|---|---|---|
| Runs inside the database | ✓ | ✓ | Partial | ✓ |
| Bayesian inference (MCMC) | ✗ | ✗ | ✗ | ✓ |
| Hierarchical models | ✗ | ✗ | ✗ | ✓ |
| Variational inference | ✗ | ✗ | ✗ | ✓ |
| Real-time streaming inference | ✗ | ✗ | ✗ | ✓ |
| Open source | ✓ | ✗ | ✗ | ✓ |

## What we've built

The [postgres-bayes repository](https://github.com/cartesiantrees/postgres-bayes) contains the algorithms, simulations, and benchmarking framework.

### Core algorithms

Each method was adapted for database-native execution using PL/Python integration inside PostgreSQL:

**Markov Chain Monte Carlo (MCMC)** using Metropolis-Hastings for sampling posterior distributions. This is the workhorse for parameter estimation from observed data.

```python
def metropolis_hastings(data, prior_mu, prior_sigma, proposal_width, n_iterations):
    mu_current = np.random.rand()
    posterior_samples = [mu_current]

    for i in range(n_iterations):
        mu_proposal = np.random.normal(mu_current, proposal_width)
        p_accept = np.exp((likelihood_proposal + prior_proposal) - 
                          (likelihood_current + prior_current))
        
        if np.random.rand() < p_accept:
            mu_current = mu_proposal
        posterior_samples.append(mu_current)

    return np.array(posterior_samples)
```

**Gibbs Sampling** for joint distributions with correlated variables. Works well for multivariate data where variables depend on each other, like customer behavior influenced by multiple factors.

**Variational Inference (ADVI)** via PyMC3 for cases where MCMC is too slow. Approximates the posterior with a simpler distribution, trading some accuracy for significant speed gains.

**Approximate Bayesian Computation** with rejection sampling for models where you can't write down a likelihood function. Slower, but it makes otherwise impossible models tractable.

### Real-world simulations

**E-commerce recommendations** that update product recommendation probabilities in real time based on user behavior. Every click, view, or purchase refines the posterior for that user-product pair.

**Stock market regime detection** using Hidden Markov Models to classify market states as stable or volatile. This isn't price prediction; it's characterization that changes how you interpret other signals.

**Trading analysis** with Bayesian linear regression for estimating stock returns based on market indices and interest rates. Uses variational inference because markets move fast and MCMC can't keep up.

## How it compares: benchmarks

We benchmarked postgres-bayes against established tools using identical datasets and algorithms.

### vs. Stan (the standard Bayesian framework)

Stan is the most widely used tool for Bayesian inference. It's excellent, but it runs entirely outside the database.

| Metric | Stan | Postgres-Bayes | Improvement |
|---|---|---|---|
| Data latency | ~300 ms | ~150 ms | 50% reduction |
| Processing time | ~1.5 s | ~1.0 s | 33% faster |
| Resource efficiency | 70% | 85% | +15 points |

The gains come from eliminating the data transfer step, not from different algorithms. Stan and postgres-bayes use the same mathematical methods. The difference is architectural.

### vs. TensorFlow and PyMC3 (external processing)

| System | Execution time | RMSE | MAE |
|---|---|---|---|
| TensorFlow + PostgreSQL | 100 s | 0.15 | 0.10 |
| PyMC3 + PostgreSQL | 90 s | 0.14 | 0.11 |
| Postgres-Bayes | 70 s | 0.11 | 0.08 |

Postgres-bayes is 30% faster than TensorFlow's external processing pipeline and shows slightly better predictive accuracy, likely because the tighter integration reduces data transformation errors.

## Our methodology

The research followed a structured approach:

1. **Algorithm selection.** We reviewed Bayesian methods and selected those suited for predictive modeling, anomaly detection, and decision analysis in production environments.
2. **Database decomposition.** Each algorithm was decomposed into SQL-compatible components: sequences of queries and PL/pgSQL functions that integrate with PostgreSQL's query planner.
3. **PostgreSQL optimization.** We analyzed query execution plans, applied indexing strategies, and leveraged parallel query processing to make inference performant at scale.
4. **Benchmarking.** We built a framework measuring execution time, CPU usage, memory consumption, accuracy (MAE, RMSE), and scalability up to terabyte-scale datasets.

The hardest part was making sure the algorithms stayed accurate when broken into database-friendly pieces. Translating mathematical operations into SQL-compatible execution plans introduces subtle opportunities for bugs, and we spent significant time validating correctness against reference implementations.

## Industrial applications

The industries where this matters most are the ones where decisions need to be made in real time and the cost of a wrong decision is high:

**Finance.** Earnings prediction, market forecasting, risk assessment. Bayesian methods quantify uncertainty in a way that point estimates from standard ML models don't, which matters when you're sizing positions or setting reserves.

**Healthcare.** Predictive diagnosis, patient monitoring, treatment outcome modeling. When a model says "this patient has a 73% probability of readmission, with a 95% credible interval of 58-85%," that's more useful than just "high risk."

**IoT and anomaly detection.** Real-time sensor data that needs probabilistic analysis at the database level, not after a batch export to a separate system.

## What's next

We're continuing to expand the algorithm library and push deeper into PostgreSQL integration:

1. Moving more computation into stored procedures and custom PostgreSQL functions
2. Addressing remaining performance bottlenecks in our MCMC implementation
3. Adding more real-world validation scenarios
4. Exploring Bayesian optimization for hyperparameter tuning within the database
5. Improving documentation for contributors

## Explore the code

Everything is open source under the Apache 2.0 license:

**[github.com/cartesiantrees/postgres-bayes](https://github.com/cartesiantrees/postgres-bayes)**

The repository includes:
- `/algorithms` for core Bayesian implementations (MCMC, Gibbs, VI, ABC)
- `/simulations` for e-commerce, trading, and market regime detection examples
- `/data_generation` for creating test datasets
- `/docs` for additional documentation

## References

The research that informed postgres-bayes. These papers shaped our understanding of the landscape and helped us identify where the gaps were.

**Bayesian inference and probabilistic programming**

- Salvatier, J., Wiecki, T.V. and Fonnesbeck, C. *Probabilistic Programming in Python using PyMC3.* PeerJ Computer Science, 2016.
- Welling, M. and Teh, Y.W. *Bayesian Learning via Stochastic Gradient Langevin Dynamics.* ICML, 2011.
- Carpenter, B. et al. *Stan: A Probabilistic Programming Language.* Journal of Statistical Software, 2017.
- Wang, Z. et al. *Bayesian Optimization in a Billion Dimensions via Random Embeddings.* Journal of Artificial Intelligence Research, 2016.

**In-database machine learning**

- Neumann, T. et al. *In-Database Machine Learning.* VLDB, 2020.
- Hellerstein, J. et al. *MADlib: A Scalable In-Database Machine Learning Framework.* VLDB Endowment, 2012.
- Kaufmann, M. et al. *Efficient and Accurate In-Database Machine Learning with SQL Code Generation in Python.* IEEE, 2019.
- Ordonez, C. and Pitchaimalai, S.K. *Bayesian Classifiers Programmed in SQL.* IEEE TKDE, 2010.
- Sandha, S. et al. *In-Database Distributed Machine Learning with Teradata.* VLDB, 2019.

**Machine learning systems and frameworks**

- Abadi, M. et al. *TensorFlow: A System for Large-Scale Machine Learning.* OSDI, 2016.
- Kraska, T. et al. *MLbase: A Distributed Machine-Learning System.* CIDR, 2013.
- Dean, J. and Ghemawat, S. *MapReduce: Simplified Data Processing on Large Clusters.* Communications of the ACM, 2008.
- Wang, Y. et al. *SQLFlow: Bridging SQL and Machine Learning.* arXiv, 2020.

**Industrial applications of Bayesian methods**

- Nagpal, C. et al. *Latent Bayesian Inference for Robust Earnings Estimates.* NeurIPS Workshop on AI in Financial Services, 2020.
- Arthi, R. et al. *TinyML Healthcare Decision Systems.* IEEE ICHI, 2022.

## Get in touch

If you're working on problems where Bayesian methods might help, or you're interested in in-database machine learning, we'd like to hear from you. Some of our best insights have come from conversations with people dealing with real-world data problems we hadn't considered.

This work is difficult. We don't have all the answers. But we're convinced that probabilistic reasoning belongs inside the database, not bolted on as an afterthought.




# [Update] (Dec 2025)

## Where This Research Meets the LLM Era

The original postgres-bayes research was designed before large language models became the dominant paradigm in AI. Since then, the landscape has shifted dramatically. But rather than making our work obsolete, the rise of LLMs has made in-database probabilistic reasoning *more* relevant, not less. Here's why, and where we're taking the research next.

### The database is now central to AI infrastructure

When we started this work, databases were where you stored data before shipping it elsewhere for ML. That has changed. PostgreSQL is now a core component of the modern AI stack, largely because of [pgvector](https://github.com/pgvector/pgvector) (Kane, 2023), which adds vector similarity search directly inside PostgreSQL. This means embeddings from LLMs can be stored, indexed, and queried within the same database that holds your application data.

This is exactly the architectural direction we argued for: keep computation close to the data. The fact that the industry converged on PostgreSQL as a vector store for RAG systems validates the core thesis of postgres-bayes. The difference is that pgvector handles *similarity search* (finding the nearest embedding), while postgres-bayes handles *probabilistic inference* (reasoning under uncertainty). These are complementary capabilities that belong in the same system.

### Retrieval-Augmented Generation needs probabilistic grounding

RAG (Lewis et al., 2020) has become the standard architecture for grounding LLM responses in proprietary data. You embed your documents, store the vectors, retrieve relevant chunks at query time, and feed them to the LLM as context.

The problem is that current RAG implementations have no principled way to quantify retrieval confidence. When the vector search returns the top-5 chunks, how confident should you be that those chunks actually contain the answer? Standard cosine similarity gives you a score, but it doesn't give you a probability distribution over relevance. Bayesian retrieval scoring could provide calibrated confidence estimates, allowing the system to know when it's likely to hallucinate because the retrieved context is weak.

This is an active area we're exploring: integrating Bayesian relevance scoring into the retrieval step of RAG pipelines, running inside PostgreSQL alongside pgvector.

### LLM uncertainty is an unsolved problem

LLMs are confidently wrong. They produce fluent, authoritative text regardless of whether the underlying content is accurate. The field is actively searching for better ways to quantify and communicate uncertainty in LLM outputs.

Several research directions are converging here:

**Conformal prediction** (Angelopoulos et al., 2022; Quach et al., 2023) applies distribution-free statistical methods to produce prediction sets with guaranteed coverage. Instead of a single answer, you get a set of possible answers with a statistical guarantee that the correct answer is included with probability 1-α. This has been applied to LLM outputs for classification tasks and is being extended to open-ended generation.

**Bayesian interpretations of in-context learning.** Xie et al. (2021) showed that transformer in-context learning can be understood as implicit Bayesian inference over a latent concept space. This means the mechanism by which LLMs learn from few-shot examples has a probabilistic interpretation, connecting our Bayesian research directly to how modern LLMs process information.

**Calibration and self-knowledge.** Kadavath et al. (2022) explored whether language models know what they know, finding that larger models are somewhat calibrated but still far from reliable. Bayesian approaches to model calibration, which postgres-bayes implements for simpler models, could be adapted for LLM output scoring.

**Monte Carlo dropout for neural network uncertainty** (Gal and Ghahramani, 2016) showed that dropout at inference time approximates Bayesian inference. This technique, originally developed for smaller networks, is being revisited for estimating uncertainty in LLM representations, particularly in embedding spaces used for retrieval.

### LLMs as database interfaces

A parallel development is the use of LLMs to query databases through natural language. Text-to-SQL systems (Rajkumar et al., 2022; Li et al., 2023) allow users to ask questions in plain English and have the LLM generate appropriate SQL queries.

This intersects with our work in two ways. First, if the database can run probabilistic inference natively, then a natural language query like "what's the probability this customer churns in the next 30 days?" could be answered directly by the database rather than requiring an external ML pipeline. Second, LLM-generated SQL is error-prone, and Bayesian confidence estimation on query results could help flag cases where the generated query might be wrong or the result unreliable.

### Agentic systems and database reasoning

The emergence of AI agents (Schick et al., 2023; Yao et al., 2022) that can use tools, including databases, creates new requirements for in-database intelligence. When an agent queries a database as part of a multi-step reasoning chain, the quality of the information it retrieves determines the quality of its downstream decisions.

If the database can return not just "the answer is X" but "the answer is X with 85% confidence, and here's the posterior distribution," the agent can make better decisions about whether to act on that information or seek additional data. This is particularly relevant for autonomous agents in high-stakes domains (finance, healthcare, operations) where acting on uncertain information has real consequences.

### Where we're taking this

Based on these developments, our current and planned research directions include:

1. **Bayesian retrieval scoring for RAG.** Integrating probabilistic relevance estimation into vector search within PostgreSQL, so RAG systems can quantify retrieval confidence alongside the LLM's generation.

2. **Conformal prediction wrappers.** Building PostgreSQL functions that apply conformal prediction methods to model outputs stored in the database, providing distribution-free uncertainty guarantees.

3. **Probabilistic query interfaces.** Extending postgres-bayes to support natural language queries via LLM-generated SQL, where the database returns probability distributions rather than point estimates.

4. **Agent-friendly database APIs.** Designing database interfaces that expose uncertainty information in formats that AI agents can reason about, enabling more reliable autonomous decision-making.

5. **Embedding-space Bayesian inference.** Applying our MCMC and variational inference methods to vector embeddings stored in pgvector, enabling probabilistic clustering, anomaly detection, and drift detection in embedding spaces.

The thesis hasn't changed: probabilistic reasoning belongs inside the database. What's changed is that the database is now a much more central piece of the AI infrastructure than it was when we started. PostgreSQL isn't just where you store rows anymore. It's where you store embeddings, run similarity search, serve RAG pipelines, and increasingly, where AI agents go to get information. Making that information probabilistically grounded is more important now than ever.

### Additional references (LLM era)

- Lewis, P. et al. *Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks.* NeurIPS, 2020.
- Kane, A. *pgvector: Open-source vector similarity search for PostgreSQL.* GitHub, 2023.
- Xie, S.M. et al. *An Explanation of In-context Learning as Implicit Bayesian Inference.* ICLR, 2022.
- Kadavath, S. et al. *Language Models (Mostly) Know What They Know.* arXiv, 2022.
- Gal, Y. and Ghahramani, Z. *Dropout as a Bayesian Approximation: Representing Model Uncertainty in Deep Learning.* ICML, 2016.
- Angelopoulos, A. et al. *Conformal Risk Control.* ICLR, 2023.
- Quach, V. et al. *Conformal Language Modeling.* ICLR, 2024.
- Schick, T. et al. *Toolformer: Language Models Can Teach Themselves to Use Tools.* NeurIPS, 2023.
- Yao, S. et al. *ReAct: Synergizing Reasoning and Acting in Language Models.* ICLR, 2023.
- Rajkumar, N. et al. *Evaluating the Text-to-SQL Capabilities of Large Language Models.* arXiv, 2022.
- Li, J. et al. *Can LLM Already Serve as A Database Interface? A Big Bench for Large-Scale Database Grounded Text-to-SQL.* NeurIPS, 2023.
- Mialon, G. et al. *Augmented Language Models: a Survey.* TMLR, 2023.