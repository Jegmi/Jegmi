---
use_math: true
---

# Which direction is causal?

"Every time the streets are wet, there was rain". Reading this statement, one could be forgiven for thinking that wet streets caused rain. Of course, we *know* that rain causes wet streets, not the other way around. But for other problems that's less obvious. For example, back pain could cause depression but depression could also cause back pain, e.g., when not leaving bed for days. In the absence of an intuitive understanding of causality, how can we infer causality from data? In this post, I will work through an easy example taken from [David MacKay's great book](https://www.inference.org.uk/itprnn/book.pdf) (exercise 35.3).

## A toy problem for causal inference

The following table $d_{ij}$ with the row and column indexes $i,j \in \{0,1\}$ summarises the outcomes  for $N=1000$ realisations of two binary random variables $A$ and $B$. The question is whether the data supports the hypothesis that $A \rightarrow B$, $A$ causes $B$ or the opposite $B \rightarrow A$. 

|       |  **A=0**  |  **A=1**  |  **Total**  |
|-------|-----------|-----------|-------------|
| **B=0** |   $d_{00}=760$ |   $d_{01}=5$ |    $d_{0\cdot}=765$ |
| **B=1** |   $d_{10}=190$ |   $d_{11}=45$ |    $d_{1\cdot}=235$ |
| **Total** |   $d_{\cdot0}=950$ |   $d_{\cdot1}=50$ |    $N=1000$ |

To answer the question we use Bayesian model comparison. Each direction of causality is represented by a model. For each model, we first compute the model evidence, $p(D \vert H_{A \rightarrow B})$ and $p(D \vert H_{B \rightarrow A})$ respectively. Assuming that both models are equally likely a-priori, the ratio of the model evidences, also called *Bayes factor*, answers the question which model is better supported by the data:

$$\gamma = \frac{p(D | H_{A \rightarrow B})}{p(D | H_{B \rightarrow A})}$$

If we had reason to believe that one model was more likely, we would apply Bayes theorem to obtain the posterior belief in the model: $p(H \vert D) \propto p(D \vert H) p(H)$.

## Specifying the model parameters and priors

Starting with $H_{A \rightarrow B}$, we assume that $A$ takes its values independently of $B$ but that $B$ depends on the realisation of $A$. Therefore, the joint probability factorises into the prior over $A$ and the conditional probability of $B$ given the $A$:

$$p(A,B) = p(B|A)p(A)$$
Since the random variables $A$ and $B$ are binary, the probability distributions are Bernoulli distributions, parameterised by the rate of success. For the prior we have:

$$p(A) = u^A (1- u)^{1-A}$$
The conditional probability of $B$ is described by a Bernoulli for each of the outcomes of $A$:

$$p(B|A=0) = q_0^B (1-q_0)^B $$

and 

$$p(B|A=1) = q_1^B (1-q_1)^B $$

The parameters $u, q_0, q_1 \in [0,1]$ describe the likelihood of the data. As a shorthand, we use $\theta = (u, q_0, q_1)$. An intermediate step to obtain the model evidence is to infer the posterior distribution over these parameters from the data $p(\theta \vert D)$, that is the probability of the parameters after having seen the data.

If the data did not contain a single positive count for $A$, then we would compute a posterior distribution over $u$ that contains most of the probability mass close to zero. However, since only a finite number of observation was made, we would still reserve some probability for event that $A=1$.

Our initial belief for the parameters $\theta$ is represented by uniform distributions. The uniform distribution means that we believe each value between 0 and 1 is equally likely for each of the parameters.

## From posterior to model evidence

Bayes' theorem states that prior $p(\theta)$ and likelihood $p(D \vert \theta)$ of the data are proportional to the posterior $p(\theta \vert D)$. The proportionality constant is the model evidence:

$$p(\theta | D) = \frac{p(D \vert \theta)p(\theta)}{p(D)}$$
Since the posterior on the lefthand side is a probability distribution, it integrates to one. Integrating over $\theta$ on both sides of the equations shows that the model evidence is obtained by integration over likelihood and prior:

$$1 = \frac{\int p(D|\theta)p(\theta) d\theta}{p(D)}$$
We can pull the evidence out of the integral because it is independent of the parameters.

## Conjugate priors make computing the evidence easy

It is hard perform integrations in high dimensions. Fortunately, closed-form solutions exist in the case of *conjugate priors*. A prior is conjugate to the likelihood if the product of both takes the same functional form as the prior. Updating our belief about the parameters with the likelihood of the data amounts to updating the prior parameters.

### The $\beta$-distribution is the conjugate prior for a Bernoulli

Let us illustrate the utility of conjugate priors with a simple example. Focusing on the binary variable $A$ (e.g. a coin) and ignoring $B$, we parametrise the probability of $A=1$ with $u$. We observe $N_A$ positive outcomes out of a total of $N$. Our prior belief about $u$ is uniform. The uniform distribution in the unit interval of is a special case of the $\beta$-distribution $p(u) = \beta(u; \alpha_0=1, \beta_0=1)$. The $\beta$-distribution for two positive parameters $\alpha, \beta > 0$ is: 

$$B(u; \alpha, \beta) = \frac{1}{Z(\alpha,\beta)} u^{\alpha-1} (1-u)^{\beta-1} $$

The normalisation constant is the beta-function (different from the $\beta$-distribution!) defined in terms of the $\Gamma$ function, a generalisation of the factorial $Z(\alpha, \beta) = \frac{\Gamma(\alpha) \Gamma(\beta)}{\Gamma(\alpha + \beta)}$.  We can see how $\alpha = \beta = 1$ makes the $\beta$-distribution uniform, that is independent of the variable $u$. Intuitively, the $\beta$-distribution represents about belief about the probability of positive (binary) events. The mean is given by $E[u] = \alpha / (\alpha + \beta)$ and the concentration $\alpha + \beta$ our belief to sample close to the mean. 

The following figure shows three examples of the distribution with increasing concentration. The top shows $\beta$-distributions in standard parametrisation. The bottom plot shows the logit-transforms $v = \log u/(1-u)$. The logit-representation maps the probability $u$ from the unit interval to parameter $v$ in the real numbers. The transformed distribution is well-behaved and its first moments, e.g. mean and variance, are intuitively easy to understand. Despite its Gaussian appearance, it is not Gaussian. The unimodal shape arises from the product of two logistic sigmoids $u(v) = (1+e^{-v})^{-1}$ with opposite signs: $$B(v;\alpha, \beta) = \frac{1}{Z(\alpha,\beta)} u^\alpha (1-u)^\beta$$
(The powers of the factors decrease by one after including the Jacobian of the transformation: $\partial u/ \partial v$.)
![[Pasted image 20240118135847.png]]


Now we return to the product of likelihood and prior, over which weh ave to integrate later to obtain the evidence. Indexing the (independent, identically distributed) data points with $i$:

$$p(D|u)p(u) = \frac{1}{Z(\alpha,\beta)} u^{\alpha-1} (1-u)^{\beta-1} \prod_i u^{A_i} (1-u)^{1-A_i}$$
Aggregating the product into counts $N_A$ and $N - N_A$ for positive and negative events respectively, we can easily group terms in powers of $u$ and $1-u$:

$$p(D|u)p(u) = \frac{1}{Z(\alpha,\beta)} u^{\alpha-1+N_A} (1-u)^{\beta-1 + N - N_A}$$
We recognise an unnormalised $\beta$-distribution with parameters $\alpha' = \alpha + N_A$ and $\beta' = \beta + N - N_A$. Introducing a normalisation constant, we have:

$$p(u) p(D|u) = \frac{Z(\alpha',\beta')}{Z(\alpha,\beta)} B(u; \alpha', \beta')$$
Using the fact that the $\beta$-distribution is properly normalised and hence integrates to one, we can obtain a close-form solution for the model evidence:

$$p(D) = \int p(D|u)p(u) du = \frac{Z(\alpha',\beta')}{Z(\alpha,\beta)}$$

Note that we have close-form solution for the posterior as well: $p(u \vert D) = B(u;\alpha',\beta')$.

## Model evidence for $A \rightarrow B$

Equipped with the $\beta$-distribution, we can now write down the evidence for the first model in our original problem $p(D \vert H_{A \rightarrow B})$. In the previous example, which introduced the $\beta$-distribution, we had a single parameter $u$. In our causal inference problem, we have three parameters $\theta = (u, q_0, q_1)$, representing the probability over $A$ and the conditional probability $p(B \vert A)$ respectively. These parameters appear in the likelihood with powers that depend on the occurrences of $A$ and combinations of $A$ and $B$ observed in the data. Grouping these factors together, we will discover three $\beta$-distributions. From the $\beta$-distributions we can then obtain the evidence. We reference the entries in the data table with $d_{ij}$. For example, $d_{01} = 5$ means that $B=0$ and $A=1$ was observed 5 times. We can ignore the prior over $\theta$ because it is flat (it is a constant):

$$p(\theta) p(D|\theta) = u^{d_{01}+d_{11}} (1-u)^{d_{00}+d_{10}} \times q_0^{d_{10}} (1-q_0)^{d_{00}} \times q_1^{d_{11}} (1-q_0)^{d_{01}}$$

The first two factors correspond to the prior over $A$, counting total occurrences of $A=1$ and $A=0$, independently of $B$. The next two factors count outcomes of $B$ when $A=0$ and the last two when $A=1$. They correspond to the conditional probabilities of $B$ given $A$. Recognising the $\beta$-distributions, e.g. $B(u; d_{01}+d_{11}+1, d_{00}+d_{10}+1)$), introducing their normalisation constants and integrating over $\theta$, we obtain the model evidence:

$$p(D) = Z(d_{01}+d_{11}+1, d_{00}+d_{10}+1)\times Z(d_{10}+1, d_{00}+1) \times Z(d_{11}+1, d_{01}+1)$$

Note that the $\beta$-distributions that we integrated out were the posterior distributions over $\theta$ that people are often interested in for their own right. However, we proceed to simplify the normalisation constants by first writing them out as gamma functions

$$p(D) = \frac{\Gamma(d_{01}+d_{11}+1) \Gamma(d_{00}+d_{10}+1)}{\Gamma(d_{01}+d_{11} + d_{00} + d_{10}+2)} 
\times 
\frac{\Gamma(d_{10}+1) \Gamma(d_{00}+1)}{\Gamma(d_{10}+d_{00} + 2)}
\times 
\frac{\Gamma(d_{11}+1) \Gamma(d_{01}+1)}{\Gamma(d_{01}+d_{11} + 2)}
Z(d_{11}+1, d_{01}+1)$$

 using the factorial property $\Gamma(n+1)/\Gamma(n) = n$, we find terms that cancel out. To avoid confusion, we explicitly include the dependence on the model in the final result:

$$p(D|H_{A \rightarrow B}) = \frac{1}{\Gamma(N+2)} 
\times 
\frac{\Gamma(d_{10}+1) \Gamma(d_{00}+1)}{d_{10}+d_{00} + 1}
\times 
\frac{\Gamma(d_{11}+1) \Gamma(d_{01}+1)}{d_{01}+d_{11} + 1}$$

## Model evidence for $B  \rightarrow A$

The analogous calculation for the model $p(D \vert H_{B \rightarrow A})$ yields:

$$p(D|H_{B \rightarrow A}) = Z(d_{10}+d_{11}+1, d_{00}+d_{01}+1)\times Z(d_{01}+1, d_{00}+1) \times Z(d_{11}+1, d_{10}+1)$$

which further reduces to

$$p(D|H_{B \rightarrow A}) = \frac{1}{\Gamma(N+2)} 
\times 
\frac{\Gamma(d_{01}+1) \Gamma(d_{00}+1)}{d_{01}+d_{00} + 1}
\times 
\frac{\Gamma(d_{11}+1) \Gamma(d_{10}+1)}{d_{10}+d_{11} + 1}$$

Note that terms that were computed from individual entries in the data table, e.g. $\Gamma(d_{01} + 1)$ and terms that depend on the sum of the data table, like $\Gamma(N+2)$ are shared between both models and cancel out once we compute the ratio of the model evidences.

## The hypothesis $A$ causes $B$  is likely

Finally, we compute the Bayes factor, i.e., the ratio of model evidences. It reveals weak evidence in favour of the hypothesis that $A$ causes $B$:

 $$\gamma = \frac{p(D | H_{A \rightarrow B})}{p(D | H_{B \rightarrow A})} 
 = \frac{(d_{01}+d_{00} + 1)(d_{10}+d_{11} + 1)}{(d_{10}+d_{00} + 1)(d_{01}+d_{11} + 1) } = \frac{(765 + 1)(235 + 1)}{(950 + 1)(50 + 1)} = 3.8$$

Why would one model would be more likely than the other? The reason is that the model $H_{A \rightarrow B}$ is more compatible with the uniform priors over the parameters $\theta$. As MacKay points out, it seems absurd given that uniform priors should make all values for $\theta$ equally probable. However, the evidence is the integral of the product of likelihood and prior. If prior has low probability mass in areas where the likelihood function assigns weight, the product will be low compared to the case where the likelihood function has most of its mass close to mass in the prior. 

MacKay argues that after the logit-transformation (used above) the uniform $\beta$-distribution is actually proportional to $u(1-u)$, an inverted parabola. The parabola takes its maximum close to $u = 0.5$. If the likelihood function of $u$, $q_0$ and $q_1$ peaks at the edges of the unit interval, the result might be lower evidence. We can make this argument quantitative and gain additional intuition by comparing the prior to the posterior.

## Comparing prior and posterior with the KL-divergence

The evidence for the hypothesis that $A$ causes $B$ over the opposite comes from its better compatibility with the flat priors. Let us dig deeper into that.

We have a parameter distribution before, the prior, and after including the data, the posterior. For both hypotheses the similarity between posterior and prior provides a measure of how much model is compatible with our prior assumptions, i.e., that the probabilities $\theta$ are uniform. 

### The KL-divergence

The similarity between two probability distributions can be measured with the Kullback-Leibler (KL) divergence:

$$D_{KL}(p(\theta) \vert \vert q(\theta) ) = \int  p(\theta) \log \frac{p(\theta)}{q(\theta)} d\theta$$

The KL-divergence is non-zero $D_{KL}(p(\theta) \vert \vert q(\theta) ) \geq 0$ and only zero when both arguments are equal. However, it is not a distance because it is not symmetric in its arguments. So how do the argument differ? The first arguments acts as reference distribution against which the second distribution is evaluated. Errors between the distributions are measured via the difference of their logs $\log  p(\theta) - \log q(\theta)$. Error counts more in regions where $p(\theta)$ has significant probability mass. David MacKay gives another perspective from coding/decoding perspective: the KL-divergence measures the information lost when approximating $p(\theta)$ with $q(\theta)$.

### Posterior as reference distribution

When comparing prior and posterior, which one should act as reference distribution? Choosing the prior as reference asks the question, how much did we learn. Choosing the posterior as reference asks how close we stayed to our original, that is prior, assumptions about the parameters.

For our problem, we need to know how closely the posterior aligns with the original assumptions encoded in the prior. We pick the posterior as reference. 

For each of the parameters in $\theta$, the posterior is a product of $\beta$-distributions. This is good news because the parameters $\theta$ are independent. For joint distribution of independent parameters, the KL-divergence is simply the sum of the KL-divergence of the individual factors:

$$D_{KL}(p(\theta) \vert \vert p(\theta | D)) = \sum_{\theta_i \in \theta} D_{KL}(p(\theta) \vert \vert p(\theta_i | D))$$

### A closed form solution for the $\beta$-distributions

For two beta distributions with parameters $\alpha_1, \beta_1$ and $\alpha_2, \beta_2$ the KL-divergence has a close-form solution:

$$D_{KL}(B(\alpha_1, \beta_1) \vert \vert B(\alpha_2, \beta_2)) = \log \frac{Z(\alpha_2, \beta_2)}{Z(\alpha_1, \beta_1)} + (\alpha_1 - \alpha_2) \psi(\alpha_1) - \psi(\alpha_1 + \beta_1) + (\beta_1 - \beta_2) (\psi(\beta_1) - \psi(\alpha_1 + \beta_1))$$

where $Z(\alpha, \beta)$ is the normalisation constant (also called beta-function, see introduction of the $\beta$-distribution above) and $\psi(x) = \partial_x \log \Gamma(x)$ is the digamma function, the logarithmic derivative of the gamma function. 

### Numerical evaluation of the KL-divergence for $H_{A \rightarrow B}$

Finally, we plug in the numbers. Recall that the posterior is a product of independent $\beta$-distributions, corresponding to the parameters $\theta = (u, q_0, q_1)$. We use this make our life easy and evaluate the KL-divergence individually. Each factor in the posterior is fully defined by its (hyper-)parameters $\alpha, \beta$.

The table below contains the values for $\alpha$ and $\beta$ for each of the factors in the posterior.  These are used in the first argument, $B(\alpha_1, \beta_1)$, in the KL-divergence. The second argument of the KL-divergence is a $\beta$-distribution with $\alpha_2 = \beta_2 = 1$, the flat prior. After evaluating each factor, we sum their contribution in the final row. 

|       |  **$\alpha$**  |  **$\beta$**  |  **KL Divergence**  |
|-------|-----------------|----------------|----------------------|
| $B(u; \alpha, \beta)$ |   $51$ |    $951$ |    $3.56$      |
| **$B(q_0; \alpha, \beta)$** |   $191$ |    $761$ |    $2.93$      |
| **$B(q_1; \alpha, \beta)$** |   $46$ |    $6$ |    $1.75$      |
| $\sum$ |   - |    - |    $8.23$      |

Next we evaluate the same for the hypothesis that $B$ causes $A$.

### Comparison to the alternate hypothesis $H_{B \rightarrow A}$

We repeat the procedure above and obtain:

|       |  **$\alpha$**  |  **$\beta$**  |  **KL Divergence**  |
|-------|-----------------|----------------|----------------------|
| $B(u; \alpha, \beta)$ |   $236$ |    $766$ |    $2.89$      |
| **$B(q_0; \alpha, \beta)$** |   $6$ |    $761$ |    $4.39$      |
| **$B(q_1; \alpha, \beta)$** |   $46$ |    $191$ |    $2.25$      |
| $\sum$ |   - |    - |    $9.53$      |

For the first model, $H_{A \rightarrow B}$ the divergence is $8.23$ while it is $9.53$ for $H_{B \rightarrow A}$. This confirms our understanding that the first hypothesis was selected because it is more compatible with the priors than the second hypothesis.

## Summary

In this post, we saw how to compare two models with opposite assumptions about the dependence of two binary variables $A$ and $B$. The assumption that $A$ was independent and $B$ conditionally depended on $A$ was weakly supported by the data. 

We used two methods to arrive at this conclusion. First, we evaluated the Bayes ratio between the evidences of both models, which resulted in $\gamma = 3.8$ in favour of the first model. It might seem strange that one model is preferred given that both have flat priors and therefore assign equal probability to all values of the inferred rate parameters $\theta$, e.g. the probability of $A=1$. The result makes sense when we consider that the posterior distribution over $p(\theta)$ matters, not any particular point estimate of $\theta$. This argument was confirmed by the second method for comparing both models. Using the KL-divergence we determined how compatible the posterior of both models were with the priors. In line with our expectations, we found that the second model had a posterior with a larger divergence, $9.53$ compared to $8.23$.

Here we only evaluated two models, corresponding to two directions of causality. If we include latent (unobserved) variables, the number of plausible models grows quickly. In the introduction we motivated causal inference by the question whether depression caused back pain or the other way around. We could include a binary, latent variable if patients spend more than 24 hours in bed, presuming that this causes back pain. Since depression is just one of many potential reasons to spend 24 hours in bed, the causal link between depression and back pain would be indirect and hence much weaker. To avoid an explosion of the number of models, well founded hypotheses about relevant latent variables are crucial. 

From a methodological perspective, we saw the utility of conjugate priors to obtain close-form solutions for the model evidence (and the posterior over the parameters). Here we used the specific example of the Bernoulli distribution, which describes binary random variables, with a $\beta$-distribution as prior and posterior. The $\beta$-distribution represents the believe in the probability of a certain outcome of binary variable. It is worth pointing out that the $\beta$-distribution is also the conjugate prior for the binomial distribution.

Further material on causal inference by Brady Neal https://www.bradyneal.com/causal-inference-course.

