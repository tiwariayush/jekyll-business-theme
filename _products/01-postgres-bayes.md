---
title: "postgres-bayes"
weight: 1
description: "Bayesian inference algorithms running directly inside PostgreSQL. MCMC, Gibbs sampling, variational inference, and real-world simulations for e-commerce, trading, and market analysis."
repo: "https://github.com/cartesiantrees/postgres-bayes"
status: "Active"
stack: "Python, PostgreSQL, PyMC3, NumPy, hmmlearn"
license: "Apache 2.0"
---

## Bayesian reasoning where your data lives

postgres-bayes is our open-source research project that brings probabilistic inference directly into PostgreSQL. Instead of pulling data out of your database, running models in a separate Python process, and pushing results back, the inference runs where the data already lives.

This project is the research that earned us JEI (Jeune Entreprise Innovante) status from the French government.

## What's in the repo

**Core algorithms**, each adapted for database integration:

- **Markov Chain Monte Carlo (MCMC)** using the Metropolis-Hastings algorithm for sampling posterior distributions. We built this to estimate parameters of Gaussian distributions from observed data, which forms the backbone of most practical applications.
- **Gibbs Sampling** for joint distributions with correlated variables. Works well when you have multivariate data where variables depend on each other, like customer behavior influenced by multiple factors.
- **Variational Inference (ADVI)** via PyMC3 for cases where you need speed over exactness. Approximates the posterior with a simpler distribution.
- **Approximate Bayesian Computation** with rejection sampling for models where you can't write down a likelihood function at all.

**Real-world simulations** that prove the algorithms work on actual problems:

- **E-commerce recommendations** that update product recommendation probabilities based on user behavior. Every click, view, or purchase refines the posterior for that user-product pair.
- **Stock market regime detection** using Hidden Markov Models to identify whether the market is in a stable or volatile state.
- **Trading analysis** with Bayesian linear regression for predicting returns based on market indices and interest rates.

## Why this matters

Standard ML models give you a prediction. Bayesian models give you a prediction *and* tell you how confident you should be in it. That distinction matters when the cost of a wrong decision is high: fraud detection, medical recommendations, financial risk assessment, inventory planning under uncertainty.

Most tools for this kind of work run in Python notebooks, disconnected from the production database. We wanted to change that.

## Get started

The repo includes everything you need to run the algorithms and simulations:

- `/algorithms` for the core Bayesian implementations
- `/simulations` for the e-commerce, trading, and market examples
- `/data_generation` for creating test datasets
- `/docs` for additional documentation

[View on GitHub](https://github.com/cartesiantrees/postgres-bayes)
