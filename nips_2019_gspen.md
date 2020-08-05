## Introduction

Structured Prediction 1. model local structure explicitly; 2. model global structure implicitly

## Background

The Structured Support Vector Machine (SSVM) objective
$$
\min_w \sum_{(x^i, y^i)}\max_{\hat{y}\in\mathcal{Y}}\{F(x^i,\hat{y};w)+L(\hat{y},y^i)\}-F(x^i,y^i;w)\Leftrightarrow\min_w \sum_{(x^i, y^i)}\max_{\hat{y}\in\mathcal{Y}}\{F(x^i,\hat{y};w)+L(\hat{y},y^i)-F(x^i,y^i;w)\}
$$
where $L(\hat{y}, y^i)$ is a task-specific and discrete loss, such as the Hamming loss ($HL=1-Accuracy\geq0$).

When $F(x^i,\hat{y};w)-F(x^i,y^i;w)>0$, maximizing the model $F(x^i,y;w)$ over $y$ obtains $\hat{y}$ instead of $y^i$. When $F(x^i,\hat{y};w)-F(x^i,y^i;w)\leq0$, maximizing the model $F(x^i,y;w)$ over $y$ obtains $y^i$. $L(\hat{y},y^i)>0$.

### Unstructured Models

$$
F(x,y;w)=\sum_{k=1}^K f_k(x,y_k;w)
$$

Because the scores for each output variable does not depend on the scores assigned to other output variables, the inference assignment is determined efficiently by independently finding the maximum score for each variable $y_k$. 

### Classical Structured Models

Classical structured models incorporate dependencies between variables by considering functions that take more than one output space variable $y_k$ as input, i.e., each function depends on a subset $r\subseteq\{1,\dots,K\}$ of the output variables. We refer to the subset of variables via $y_r=(y_k)_{k\in r}$ and use $f_r$ to denote the corresponding function. The overall score for a configuration $y$ is a sum of these functions, i.e.,
$$
F(x,y;w)=\sum_{r\in\mathcal{R}}f_r(x,y_r;w)
$$
Hereby, $\mathcal{R}$ is a set containing all of the va	riables subsets which are required to compute $F$. This formulation allows to explicitly model relations between variables, but it comes at the price of more complex inference which is NP-hard in general.

Inference, i.e., maximization of the score, is equivalent to the integer linear program
$$
\max_{p\in\mathcal{M}}\sum_{r\in\mathcal{R}}\sum_{y_r\in\mathcal{Y}_r}p_r(y_r)f_r(x,y_r;w)
$$
where each $p_r$ represents a marginal probability vector for region $r$ and $\mathcal{M}$ represents the set of $p_r$ whose marginal distributions are globally consistent, which is often called the marginal polytope. D