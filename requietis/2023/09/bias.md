@def title = "Understanding Bias in RL"
@def tags = ["rl", "technical"]
@def isblog = true
@def showall = true
@def rss_pubdate = Date(2024, 9, 13)
@def rss_title = "Understanding the Bias in RL"
@def rss_description = "Understanding Bias-Variance Tradeoff in RL through Stein's Paradox."

@@main-heading {{fill title}}@@
\toc

The quest of understanding or inferring data beyond what has been acquired has been an age old statistics problem. And with understanding data, it’s non-trivial to be able to measure the “goodness” of a system with the task of predicting unseen data. We call this “goodness” term as the error.\sidenote{}{or rather the “badness”} The two underlying features of this error are bias and variance aka the structural error and estimation error.

1. **Structural Error** - This type of error is incurred when the true underlying relationship between the data points is of a different structure than the predicted function’s structure. For example, if the data is non-linear but the predicted function is linear.
2. **Estimation Error** - In machine learning, this is the same as the training loss, and is usually incurred when the training dataset is very small. Increasing the number of training examples decreases this error. Estimation loss is always incurred when fitting the model such that it is _more generalizable_.

@@important
**Note**: There is a trade-off between these two errors. Using a rich high- dimensional function for prediction might reduce structural mistakes, but the same dataset would be used to train many more parameters thereby incurring an estimation loss. On the other hand, if you have the minimum number of parameters to help estimation error, structural error is introduced.
@@

## Estimators

In \citep{fisher1922}, Fisher introduced the method of Maximum Likelihood Estimation (MLE) which can be understood using the following application of it in a toy problem:

@@problem
Given an n-variate normal distribution with a (co)variance of 1 and a sample from the distribution, can you estimate it’s mean?
@@

The MLE method proposes that a _good_ estimate of the mean can only be the sample itself, which does hold true in lower-dimensional gaussians but in higher dimensions, this fails drastically.\sidenote{}{notion of “good” explained later}

A good estimator can be defined as one that reduces the L2-norm ($\mathcal L$) distance between the true mean and the predicted mean.

\begin{equation}
\mathcal{L}(x)=\lVert x-\mu\rVert^2.
\end{equation}

This has been the explanation for a simple toy problem. Now let’s extend it for $n$ i.i.d. samples $X_1,\ldots,X_n$ sampled from a gaussian $X_i\sim\mathcal{N}(\theta_i,1)$ for each $i=1,\ldots,n$. Now, to estimate $\theta$ for all such $n$ random variables with only the noisy samples for each $i$, a trivial estimator can be $\hat\theta_i=X_i$.

Now, determining the “goodness” of this estimator can be done using the estimated loss value of this estimator: $$\mathcal L(\hat\theta)=\mathbb{E}\left[\lVert\hat\theta-\theta\rVert^2\right].$$

This trivial estimator is suboptimal whenever $n\geq3$ (inadmissible) which means that there exists an estimator $\hat\theta'$ s.t. $\mathcal L(\hat\theta')\leq\mathcal L(\hat\theta)$ with $n\geq 3$.

### Implicit Bias and Variance

Here we show that the notion of bias and variance is implicit in the definition of an error that classify both structural and estimation error. To see, let’s look at the formula of variance:

$$\text{Var}[\theta]=\mathbb{E}[\theta^2]-\mathbb{E}[\theta]^2.$$

Introducing the mean $X_i:$ $$\text{Var}[\theta-X]=\mathbb{E}\left[(\theta-X)^2\right]-\mathbb{E}[\theta-X]^2.$$

Subsequently, we realize the last equation as:
$$\mathbb{E}\left[(\theta-X)^2\right]=\underbrace{\mathbb{E}[\theta-X]^2}_{\text{bias}^2}+\underbrace{\text{Var}[\theta-X]}_\text{variance}\\
\implies \text{error}=\text{bias}^2+\text{variance},$$

where $\mathbb{E}\left[(\theta-X)^2\right]$ is the L2-norm term. We see the mathematics behind the implicit nature of bias and variance in the loss function (L2-norm here). Here we also see that the bias produces a bigger change to the error and thus the derivative of the error w.r.t. bias is higher than the variance.

An error term such as the L2-norm $\sum_{i=1}^n\mathbb{E}\left[\lVert\hat\theta_i-\theta_i\rVert^2\right]$ that sums the different loss values of each estimation induces a bias-variance tradeoff. The loss function on itself has an inadmissible estimator i.e. $\hat\theta_i=X_i$. But with the case of the L2-norm, a shrinkage factor introduced by James and Stein decreases variance by introducing a bit of bias, therweby reducing the L2-norm.

## Stein's paradox

We see that stein’s estimator given below utilizes the then-unknown fact that bias can be leveraged for variance to improve the error: $$\theta_\text{JS}=\left(1-\frac{(p-2)\sigma^2}{\lVert X\rVert^2_2}\right)X_i.$$

Instead of the common $\theta=X_i$, there is a new term $\left(1-\frac{(p-2)\sigma^2}{\lVert X\rVert^2_2}\right)$ called the “shrinkage factor” that skews some data points $X_i$ towards the origin.[^1] 


## Stein's influence in RL

### Policy gradients 

Recall that the stochastic reinforcement learning objective is to learn a policy distribution $\pi_\theta(\mathbf s_t)$ that maps states to actions.\sidenote{}{I will be assuming that the reader (you) knows a bit about RL but in case you don’t, refer to any deep RL book and you shall receive that wisdom soon enough.} In policy gradients, we train a policy such as a deep neural network with weights $\theta$ and the weights of the network that maximize the objective function which is represented as $\theta^*$. We can represent the gradient of the objective function that the model optimizes as $J(\theta)$ (formulated below):

\begin{equation}J(\theta)=E_{\tau\sim\pi_\theta(\tau)}[r(\tau)]\end{equation}

where each rollout $\tau$ is the trajectory of state-action pairs seen during a rollout in $T$ time-steps, and the probability of a trajectory $\tau$ can be defined as:

$$\pi_\theta(\tau)=p(s_0,a_0,\ldots,s_{T-1},a_{T-1})=p(s_0)\pi_\theta(a_0|s_0)\prod_{t=1}^{T-1}p(s_t|s_{t-1},a_{t-1})\pi_\theta(a_t|s_t)$$

and the total reward accumulated during the rollout of $\tau$:

$$r(\tau)=r(s_0,a_0,\ldots,s_{T-1},a_{T-1})=\sum\limits_{t=1}^{T-1}r(s_t,a_t).$$

The policy gradient approach is to directly take the gradient of this objective (Eq. $2$):

\begin{align}\nabla_\theta J(\theta)&=\nabla_\theta\int \pi_\theta(\tau)r(\tau)d\tau
\\&=\int\pi_\theta(\tau)\nabla_\theta\log\pi_\theta(\tau)r(\tau)d\tau\\&=E_{\tau\sim\pi_\theta(\tau)}[\nabla_\theta\log\pi_\theta(\tau)r(\tau)].
\end{align}

In practice, the expectation over trajectories $\tau$ can be approximated from a batch of $N$ sampled trajectories:

\begin{align}
\nabla_\theta J(\theta)&\approx\frac{1}{N}\sum\limits_{i=1}^N\nabla_\theta\log\pi_\theta(\tau_i)r(\tau_i)

\\&=\frac{1}{N}\sum\limits_{i=1}^N\left(\sum\limits_{t=1}^T\nabla_\theta\log\pi_\theta(a_{it}|s_{it})\right)\left(\sum\limits_{t=1}^{T-1}r(s_{it},a_{it})\right).
\end{align}

Here we see that the policy $\pi_\theta$ is a probability distribution over the action space, conditioned on the state. In the agent-environment loop, the agent samples an action $a_t$ from $\pi_\theta(\cdot|s_t)$ and the environment responds with a reward $r(s_t,a_t)$.

#### What is wrong with PG?

We see that the variance of the sample reward collected along a few trajectories in the space of all possible trajectories is high meaning that predicting the return of a trajectory becomes worse with better performing trajectories.[^2]

@@row
@@container
@@top ![](/assets/reward_variance.png) @@
@@
@@caption
The different colored trajectories are plotted along with the running reward variance. It's clear here that different trajectories lead to significantly different returns.\sidenote{}{even in agents like PPO (such as the one used in the image), learned trajectories have high return variance.}
@@
~~~
<div style="clear: both"></div>
~~~
@@

A relatively high performing trajectory changes the distribution much more than a relatively less performing one. The trick is to minimize absolute return while maintaining the relationship of the relative rewards amongst trajectories. So the task is to reduce variance. One way we achieve this is by discarding the reward achieved at a previous time step.

@@important
By using the causality trick, we reduce expectation thereby reducing variance and by adding baselines, we affect sample rewards by normalizing them, thereby reducing variance.\sidenote{}{Similarly, in the off-policy policy gradient methods, i.e. where the model is trained on a different dataset and learns new parameters 𝜃' (similar to fine-tuning in DL terms).}
@@

#### What else is wrong with Policy Gradients?

We can notice that there is no regularizing term in the objective function (dissimilar to better ML objectives) and so the gradient can change parameters a lot. 

The key problem is that **some parameters are delicate** as in they change a lot with a small change in the gradient step, so adding a regularizing term with a weight to each parameter is essential to a reliable performance. 

@@soln
The solution is to rescale the *gradient* instead of the parameters while training. So the regularizing term is on the policy instead! This was shown in \citep{schaal2008}.
@@


[^1]: Doing so introduces more bias while reducing variance, but then why doesn’t this work for dimensions $n\leq2$?

[^2]: You can find the code for this [here](https://github.com/sheeerio/rl-implementation/blob/main/deep-rl-algorithms/ac-pg/ppo/ppo.py).

## Variance Reduction

### Reward-to-go

One way to reduce the variance of the policy gradient is to exploit causality: the notion that the policy cannot affect rewards in the past. This yields the following modified objective, where the sum of rewards here does not include the rewards achieved prior to the time step at which the policy is being queried. This sum of rewards is a sample estimate of the Q function ($\hat{Q}^\pi_{i,t}$), and is referred to as the “reward-to-go”. The Q-function can also be definedas the estimate of expected reward if we take action $\mathbf a_{i,t}$ in state $\mathbf s_{i,t}$.

### Discounting

Multiplying a discount factor $\gamma$ to the rewards can be interpreted as encouraging the agent to focus more on the rewards that are closer in time, and less on the rewards that are further in the future. This can also be thought of as a means for reducing variance (because there is more variance possible when considering futures that are further into the future). We saw in lecture that the discount factor can be incorporated in two ways, as shown below.

The first way applies the discount on the rewards form full trajectory:

\begin{equation}\nabla_\theta J(\theta)\approx\frac{1}{N}\sum\limits_{i=1}^N\left(\sum\limits_{t=0}^{T-1}\nabla_\theta\log\pi_\theta(a_{it}|s_{it})\right)\left(\sum\limits_{t'=0}^{T-1}\gamma^{t'-t}r(s_{it'},a_{it'})\right).
\end{equation}

### Baselines

Another variance reduction method is to subtract a baseline (that is a constant with respect to $\tau$) from the sum of rewards:

\begin{equation}\nabla_\theta J(\theta)=\nabla_\theta E_{\tau\sim\pi_\theta(\tau)}[r(\tau)-b].
\end{equation}

This leaves the policy gradient unbiased because $$\nabla_\theta E_{\tau\sim\pi_\theta(\tau)}[b]=0.$$

This value function will be trained to approximate the sum of future rewards starting from a particular state: 
\begin{equation}
V_{\phi}^\pi(s_t)\approx\sum\limits_{t'=t}^{T-1}E_{\pi_\theta}[r(s_{t'},a_{t'})|s_t],
\end{equation}

so the approximate policy gradient now looks like this:

\begin{equation}
\nabla_\theta J(\theta)\approx\frac{1}{N}\sum\limits_{i=1}^N\sum\limits_{t=0}^{T-1}\nabla_\theta\log\pi_\theta(a_{it}|s_{it})\left(\left(\sum\limits_{t'=t}^{T-1}\gamma_{t'-t}r(s_{it'},a_{it'})\right)-V_{\phi}^\pi(s_{it})\right).
\end{equation}

### Generalized Advantage Estimation 

The quantity $\left(\sum_{t'=t}^{T-1}\gamma^{t'-t}r(s_{t'},a_{t'})\right)-V_{\phi}^\pi(s_t)$ from the pervious policy gradient expression (removing the $i$ index for clarity) can be interpreted as an estimate of the advantage function:

\begin{equation}
A^\pi(s_t,a_t)=Q^\pi(s_t,a_t)-V^\pi(s_t),
\end{equation}

where $Q^\pi(s_t,a_t)$ is estimated using Monte Carlo returns and $V^\pi(s_t)$ is estimated using the learned value function $V_\phi^\pi$. $A^\pi(s_t,a_t)$ is called as the advantage function. We can further reduce variance by also using $V^\pi_\phi$ in place of the Monte Carlo returns to estimate the advantage function as:

\begin{equation}A^\pi(s_t,a_t)\approx\delta_t=r(s_t,a_t)+\gamma V_\phi^\pi(s_{t+1})-V^\pi_\phi(s_t),
\end{equation}

with the edge case $\delta_{T-1}=r(s_{T-1},a_{T-1})-V^\pi_\phi(s_{T-1})$. However, this comes at the cost of introducing bias to our policy gradient estimate, due to modelling errors in $V_\phi^\pi$. We can instead use a combination of $n$-step Monte Carlo returns and $V_\phi^\pi$ to estimate the advantage function as:

\begin{equation}A_n^\pi(s_t,a_t)=\sum\limits_{t'=t}^{t+n}\gamma^{t'-t}r(s_{t'},a_{t'})+\gamma^nV_\phi^\pi(s_{t+n+1})-V^\pi_\phi(s_t).
\end{equation}

Increasing $n$ incorporates the Monte Carlo returns more heavily in the advantage estimate, which lowers bias and increases variance, while decreasing $n$ does the opposite. Note that $n=T-t-1$ recovers the unbiased but higher variance Monte Carlo advantage estimate used in (14), while $n=0$ recovers the lower variance but higher bias advantage estimate $\delta_t$ used in (16).

We can combine multiple $n$-step advantage estimates as an exponentially weighted sum, which is known as the generalized advantage estimator (GAE). Let $\lambda\in[0,1]$. Then we define:

\begin{equation}A^\pi_\text{GAE}(s_t,a_t)=\frac{1-\lambda^{T-t-1}}{1-\lambda}\sum\limits_{n=1}^{T-t-1}\lambda^{n-1}A_n^\pi(s_t,a_t),
\end{equation}

where $\frac{1-\lambda^{T-t-1}}{1-\lambda}$ is a normalizing constant. Note that a higher $\lambda$ emphasizes advantage estimates with higher values of $n$, and a lower $\lambda$ does the opposite. Thus, $\lambda$ serves as a control for the bias-variance tradeoff, where increasing $\lambda$ decreases bias and increases variance. In the infinite horizon case ($T=\infty$), we can show:

\begin{align}
A_\text{GAE}^\pi(s_t,a_t)&=\frac{1}{1-\lambda}\sum\limits_{n=1}^\infty\lambda^{n-1}A_n^\pi(s_t,a_t)\\&=\sum\limits_{t'=t}^\infty (\gamma\lambda)^{t'-t}\delta_{t'},
\end{align}

where we have omitted the derivation for brevity (see the [GAE paper](https://arxiv.org/pdf/1506.02438.pdf) for details). In the finite horizon case, we can write:

\begin{equation}
A^\pi_\text{GAE}(s_t,a_t)=\sum\limits_{t'=t}^{T-1}(\gamma\lambda)^{t'-t}\delta_{t'},
\end{equation}

which serves as a way we can efficiently implement the generalized advantage estimator, since we can recursively compute:

\begin{equation}
A^\pi_\text{GAE}(s_t,a_t)=\delta_t+\gamma\lambda A^\pi_\text{GAE}(s_{t+1},a_{t+1}).
\end{equation}

Similarly, in the off-policy policy gradient methods, i.e. where the model is trained on a different dataset and learns new parameters $\theta'$.\sidenote{}{similar to fine-tuning in DL terms.}

So we see that:

@@important
By using the causality trick, we reduce expectation thereby reducing variance
and by adding baselines, we affect sample rewards by normalizing them, thereby reducing variance. 

In GAE, we see that using the advantage function $A^\pi(s_t,a_t)$ is the replace for the baseline function. 

If the advantage function is incorrect, then the entire policy gradient can be biased which is OK because we can tradeoff a bit of bias for the large reduction in variance
@@

## References

* \biblabel{fisher1922}{Fisher (1922)} R.A. **Fisher** [On the Mathematical Foundations of Theoretical Statistics](http://www.medicine.mcgill.ca/epidemiology/hanley/bios601/Likelihood/Fisher1922.pdf), The Royal Society 1922.

* \biblabel{schaal2008}{Peters & Schaal, 2008} **Peters**, Jan, and **Schaal**, Stefan., [Reinforcement learning of motor skills with policy gradients](https://www.sciencedirect.com/science/article/abs/pii/S0893608008000701), Neural Networks 2008.

## Further Reading

* **Antognini**, [Understanding Stein's paradox](https://joe-antognini.github.io/machine-learning/steins-paradox), 2021.

* **Jones**, [James-Stein estimator](https://andrewcharlesjones.github.io/journal/james-stein-estimator.html), 2020.

* **Chau**, [Demystifying Stein's Paradox](https://jchau.org/2021/01/29/demystifying-stein-s-paradox/), 2021.