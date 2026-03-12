---
title: Our Research
layout: page
description: "Explore Cartesian Trees' research on Bayesian inference in PostgreSQL — MCMC, Gibbs sampling, variational inference, and real-world simulations for e-commerce, trading, and market analysis. Open source under Apache 2.0."
bodyClass: page-research
---

# Bringing Bayesian Intelligence to PostgreSQL

We're a **Jeune Entreprise Innovante (JEI)** based in France, and this is the research that got us there.

For the past couple of years, we've been working on a question that sounds simple but goes surprisingly deep: *what if your database could reason under uncertainty?* Databases are excellent at storing and retrieving facts, but real-world decisions rarely happen with complete information. That's where Bayesian inference comes in—and we've been building it directly into PostgreSQL.

Our work is open source. You can explore everything we've built at [github.com/cartesiantrees/postgres-bayes](https://github.com/cartesiantrees/postgres-bayes).

---

## Why We Started This

Anyone who's deployed machine learning in production knows the frustration. You train a model in Python, export it, deploy it somewhere else, and then spend half your time shuttling data back and forth between your database and your inference engine. It's clunky, it's slow, and when something breaks at 2am, good luck figuring out which piece failed.

We kept asking ourselves: why can't the database just do the inference? Why should probabilistic reasoning live outside the place where your data actually lives?

That question led us down a rabbit hole of research—adapting Bayesian algorithms for relational database architectures, figuring out how to make MCMC sampling work within PostgreSQL's execution model, and testing whether any of this could actually perform well enough to be useful.

Turns out, it can. And that work earned us JEI status from the French government—recognition that what we're doing qualifies as genuine R&D innovation.

---

## What We've Built

Our [postgres-bayes repository](https://github.com/cartesiantrees/postgres-bayes) contains the algorithms, simulations, and tooling we've developed. Here's what's in there:

### Core Algorithms

We've implemented several foundational Bayesian methods, each adapted for database integration:

**Markov Chain Monte Carlo (MCMC)** — Our implementation uses the Metropolis-Hastings algorithm to sample from posterior distributions. We built this to estimate parameters of Gaussian distributions from observed data, which forms the backbone of many practical applications.

```python
def metropolis_hastings(data, prior_mu, prior_sigma, proposal_width, n_iterations):
    mu_current = np.random.rand()
    posterior_samples = [mu_current]

    for i in range(n_iterations):
        # Propose a new value from a normal distribution
        mu_proposal = np.random.normal(mu_current, proposal_width)
        
        # Calculate likelihood and prior for current and proposed
        # Accept or reject based on the ratio
        p_accept = np.exp((likelihood_proposal + prior_proposal) - 
                          (likelihood_current + prior_current))
        
        if np.random.rand() < p_accept:
            mu_current = mu_proposal
        posterior_samples.append(mu_current)

    return np.array(posterior_samples)
```

**Gibbs Sampling** — For joint distributions with correlated variables, we implemented Gibbs sampling. This works particularly well when you have bivariate or multivariate data where variables depend on each other—think customer behavior influenced by multiple factors simultaneously.

**Variational Inference (ADVI)** — When you need speed over exactness, variational methods approximate the posterior with a simpler distribution. We use PyMC3's automatic differentiation variational inference for cases where MCMC would be too slow.

**Approximate Bayesian Computation** — For the tricky cases where you can't even write down a likelihood function, we've implemented ABC with rejection sampling. It's slower, but it opens doors to models that would otherwise be impossible.

### Real-World Simulations

Algorithms are one thing; proving they work on real problems is another. We've built simulations for several domains:

**E-commerce Recommendations** — A Bayesian system that updates product recommendation probabilities based on user behavior. Every click, view, or purchase refines the posterior probability for that user-product pair.

```python
def bayesian_update_for_user(user_id):
    activities = fetch_user_activities(user_id)
    for activity in activities:
        product_id = activity['product_id']
        action_type = activity['action_type']
        updated_probability = calculate_updated_probability(action_type)
        update_recommendation_probability(user_id, product_id, updated_probability)
```

**Stock Market Regime Detection** — Using Hidden Markov Models, we identify whether the market is in a "stable" or "volatile" regime. This isn't prediction—it's characterization. Knowing which regime you're in changes how you should interpret other signals.

**Trading Analysis** — Bayesian linear regression for predicting stock returns based on market indices and interest rates. We use variational inference here because markets move fast and you can't wait for MCMC to converge.

---

## Our Methodology

Building this wasn't just about writing code. We followed a structured research approach:

### Algorithm Adaptation

1. We reviewed Bayesian methods and selected those suited for predictive modeling, anomaly detection, and decision analysis
2. Each algorithm got decomposed into SQL-compatible components—sequences of queries and PL/pgSQL functions
3. We optimized for PostgreSQL specifically: analyzing query execution plans, applying indexing strategies, leveraging parallel query processing

The hardest part? Making sure the algorithms stayed accurate when broken into database-friendly pieces. It's easy to introduce subtle bugs when you're translating mathematical operations into SQL.

### Testing and Validation

We built a proper benchmarking framework:

- **Performance metrics**: Query execution time, CPU usage, memory consumption
- **Accuracy metrics**: Precision and recall against known outcomes
- **Scalability tests**: Increasing data volumes to find breaking points

We tested against three baselines: standard Postgres queries, external Python-based analysis, and established statistical tools. The results have been encouraging—competitive performance with the added benefit of keeping everything inside the database.

---

## The Technical Stack

For those who want to run our code or build on it, here's what we're using:

- **PostgreSQL** with psycopg2 and asyncpg for database connectivity
- **PyMC3** for probabilistic programming and ADVI
- **NumPy** for numerical operations
- **hmmlearn** for Hidden Markov Models
- **Faker** for generating realistic test data
- **Matplotlib/Seaborn** for visualization

Everything's in the [requirements.txt](https://github.com/cartesiantrees/postgres-bayes/blob/main/requirements.txt).

---

## What's Next

We're continuing to expand the algorithm library and improve PostgreSQL integration. Current focus areas:

1. **Deeper SQL integration** — Moving more computation into stored procedures and custom PostgreSQL functions
2. **Performance optimization** — There are still bottlenecks in our MCMC implementation we're working through
3. **Additional simulations** — We're adding more real-world scenarios to validate the approach
4. **Documentation** — Making it easier for others to use and contribute

The JEI status gives us the freedom to focus on getting this right rather than rushing something to market. Good research takes time, and we'd rather ship something solid.

---

## Explore the Code

Everything is open source under the Apache 2.0 license:

**[github.com/cartesiantrees/postgres-bayes](https://github.com/cartesiantrees/postgres-bayes)**

The repository includes:
- `/algorithms` — Core Bayesian implementations (MCMC, Gibbs, VI, ABC)
- `/simulations` — E-commerce, trading, and market regime detection examples
- `/data_generation` — Tools for creating test datasets
- `/docs` — Additional documentation

---

## Get in Touch

We're always interested in talking to people working on similar problems or facing challenges where Bayesian methods might help. If you've got a use case, a question, or just want to discuss probabilistic databases, reach out.

Some of our best insights have come from conversations with people dealing with real-world data problems we hadn't thought about. This work is difficult—we don't have all the answers—but we're convinced that probabilistic reasoning belongs inside the database, not bolted on as an afterthought.
