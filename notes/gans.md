# Generative Adversarial Nets

## Introduction

* The paper proposes an adversarial approach for estimating generative models where one model (generative model) tries to learn a data distribution and another model (discriminative model) tries to distinguish between samples from the generative model and original data distribution.
* [Link to the paper](https://arxiv.org/abs/1406.2661)

## Adversarial Net

* Two models - Generative Model(*G*) and Discriminative Model(*D*)
* Both are multi-layer perceptrons.
* *G* takes as input a noise variable *z* and outputs data sample *x(=G(z))*.
* *D* takes as input a data sample *x* and predicts whether it came from true data or from *G*.
* *G* tries to minimise *log(1-D(G(z)))* while *D* tries to maximise the probability of correct classification.
* Think of it as a minimax game between 2 players and the global optimum would be when *G* generates perfect samples and *D* can not distinguish between the samples (thereby always returning 0.5 as the probability of sample coming from true data).
* Alternate between *k* steps of training *D* and 1 step of training *G* so that *D* is maintained near its optimal solution.
* When starting training, the loss *log(1-D(G(z)))* would saturate as *G* would be weak. Instead maximise *log(D(G(z)))*
* The paper contains the theoretical proof for global optimum of the minimax game.

## Experiments

* Datasets
    * MNIST, Toronto Face Database, CIFAR-10
* Generator model uses RELU and sigmoid activations.
* Discriminator model uses maxout and dropout.
* Evaluation Metric
    * Fit Gaussian Parzen window to samples obtained from *G* and compare log-likelihood.

## Strengths

* Computational advantages
    * Backprop is sufficient for training with no need for Markov chains or performing inference.
    * A variety of functions can be used in the model.
* Since *G* is trained only using the gradients from *D*, fewer chances of directly copying features from the true data.
* Can represent sharp (even degenerate) distributions.

## Weakness

* *D* must be well synchronised with *G*.
* While *G* may learn to sample data points that are indistinguishable from true data, no explicit representation can be obtained.

## Possible Extensions

* Conditional generative models.
* Inference network to predict *z* given *x*.
* Implement a stochastic extension of the deterministic [Multi-Prediction Deep Boltzmann Machines](https://papers.nips.cc/paper/5024-multi-prediction-deep-boltzmann-machines.pdf)
* Using discriminator net or inference net for feature selection.
* Accelerating training by ensuring better coordination between *G* and *D* or by determining better distributions to sample *z* from during training.